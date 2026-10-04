# Scripts

Documentación de los scripts y configuraciones de laboratorio para equipos Cisco (IOS / IOSv) y contenedores Docker usados como hosts finales en topologías GNS3.

Pensado para prácticas de switching, routing, seguridad perimetral (FortiGate) y despliegue de servicios (Apache + PHP + MariaDB) en un entorno controlado.

> ⚠️ **Aviso:** Estas configuraciones y scripts son para entornos de laboratorio (GNS3, Cisco Modeling Labs, EVE-NG, Packet Tracer). No usar tal cual en producción sin revisar seguridad, contraseñas y políticas. Algunos archivos contienen vulnerabilidades intencionadas con fines didácticos.

---

## 📁 Estructura

```
cisco-lab-configs/
├── scripts.md
├── configs/
│   └── switch-running-config.txt
└── scripts/
    ├── build-web-server.sh
    └── build-db-server.sh
```

> **Nota sobre la topología:** este README documenta el switch y los scripts de forma genérica. En el despliegue actual del laboratorio, WEB-Server y DB-Server están conectados directamente a las interfaces del FortiGate (no detrás del switch); el switch con VLAN 10 se usa únicamente para los equipos de usuarios (PCs).

---

## 📜 Contenido

### 1. `configs/switch-running-config.txt` — Switch Cisco IOSv

Configuración de un switch de acceso con VLANs, port-security y protección de STP.

| Elemento | Detalle |
|---|---|
| Hostname | Switch |
| Versión IOS | 15.2 |
| STP mode | PVST |
| VLANs | 1 (default), 10 |
| Trunk | Gi0/0 → VLANs 1,10 con encapsulación dot1q |
| Access VLAN 10 | Gi0/2, Gi0/3, Gi1/0 |
| Access default VLAN | Gi0/1 |
| Port-Security | Máx. 2 MACs, violación *restrict* en puertos access |
| PortFast + BPDU Guard | Gi1/0 |
| Puerto en shutdown | Gi0/3 |
| SSH | Cifrado AES128/192/256-CTR |
| HTTP/HTTPS server | Habilitados |

**Puertos clave:**

| Interfaz | Modo | VLAN | Notas |
|---|---|---|---|
| Gi0/0 | Trunk | 1,10 | Uplink |
| Gi0/1 | Access | 1 | Port-security |
| Gi0/2 | Access | 10 | Port-security |
| Gi0/3 | Access | 10 | Port-security + shutdown |
| Gi1/0 | Access | 10 | PortFast edge + BPDU Guard |

---

### 2. `scripts/build-web-server.sh` — WEB-Server (Apache + PHP + HTTPS)

Imagen Docker con Ubuntu 22.04 + Apache2 + PHP + MySQLi + HTTPS para usarse como host final en GNS3.

**Direccionamiento (matrícula 0701):**

| Parámetro | Valor |
|---|---|
| Red | 10.7.1.128/28 |
| WEB-Server | 10.7.1.130 |
| Gateway | 10.7.1.129 |
| DB_IP (default) | 10.7.1.146 |

**Características:**

- HTTPS con certificado autofirmado en `/etc/ssl/lab/web.{crt,key}` → ideal para probar SSL Inspection / DPI en FortiGate.
  - Para que el FortiGate pueda inspeccionar el tráfico HTTPS con DPI, el certificado (o la CA de inspección del FortiGate) debe importarse/confiarse manualmente en el navegador del cliente; de lo contrario el navegador mostrará advertencia de certificado no confiable.
- `search.php` intencionadamente vulnerable a SQL Injection para practicar detección con IPS.
  - Payload de prueba de ejemplo: `' OR '1'='1` en el campo de búsqueda, o `' UNION SELECT username,password FROM users-- -` para exfiltración.
- `test.exe` en la raíz web para probar el File Filter del FortiGate.
- IP asignada en runtime vía variables de entorno (`IP_ADDR`, `PREFIX`, `GATEWAY`, `DB_IP`).

---

### 3. `scripts/build-db-server.sh` — DB-Server (MariaDB)

Imagen Docker con Ubuntu 22.04 + MariaDB Server, preconfigurada con `labdb` y usuario `webuser` restringido por host.

**Direccionamiento (matrícula 0701):**

| Parámetro | Valor |
|---|---|
| Red | 10.7.1.144/28 |
| DB-Server | 10.7.1.146 |
| Gateway | 10.7.1.145 |

**Características:**

- Base `labdb` con tabla `users` (alice, bob).
- `webuser` solo se puede conectar desde `10.7.1.130` (IP del WEB-Server).
- `bind-address = 0.0.0.0` porque la IP final se asigna en runtime.

**🩹 Fix v2 — crash `Exited (1)` en el primer boot:**

El `docker build` dejaba un PID/socket fantasma de MariaDB en la capa congelada, lo que hacía fallar el arranque real. La v2 aplica tres defensas:

1. En el `Dockerfile`: `service mariadb stop` + `rm -f /var/run/mysqld/mysqld.{pid,sock}` antes de congelar la capa.
2. En `start-network.sh`: limpieza defensiva de PID/socket antes de arrancar.
3. `tail -F` (mayúscula) en vez de `-f`, que reintenta si el archivo aún no existe.

Además, espera activa hasta 15 s a que `eth0` exista (GNS3 a veces conecta el cable con retraso) y redirige todo el log a `/var/log/start-network.log` para diagnóstico.

---

## 🚀 Cómo usar estos scripts

### Aplicar configuración Cisco (IOS/IOSv)

1. Copia el contenido del archivo `.txt` al portapapeles.
2. Accede por consola o SSH al equipo:

   ```
   Switch> enable
   Switch# configure terminal
   ```

3. Pega la configuración (modo `conf t`).
4. Guarda los cambios:

   ```
   Switch# write memory
   ```

### Construir las imágenes Docker en la GNS3 VM

```bash
bash build-db-server.sh
bash build-web-server.sh
```

### Arrancar ambos contenedores

```bash
docker run -d --name db-server \
  -e IP_ADDR=10.7.1.146 -e PREFIX=28 -e GATEWAY=10.7.1.145 \
  db-server-lab

docker run -d --name web-server \
  -e IP_ADDR=10.7.1.130 -e PREFIX=28 \
  -e GATEWAY=10.7.1.129 -e DB_IP=10.7.1.146 \
  web-server-lab
```

---

## 🛠️ Troubleshooting rápido

| Síntoma | Posible causa | Solución |
|---|---|---|
| `web-server` no resuelve/conecta a la DB | Contenedores en redes GNS3 distintas o `DB_IP` mal pasado | Verificar que ambos estén en el mismo segmento y que `DB_IP` coincida con la IP real del DB-Server |
| DB-Server queda en `Exited (1)` | PID/socket fantasma de un build anterior | Confirmar que se está usando la imagen v2 (ver Fix v2 arriba) |
| FortiGate no detecta la SQLi con DPI | SSL inspection no configurada o certificado no confiado | Habilitar deep inspection en la política y confiar el certificado en el cliente |
| No hay salida a `eth0` al iniciar el contenedor | GNS3 conecta el cable con retraso | El script ya espera hasta 15 s; si persiste, revisar `/var/log/start-network.log` |

---

## Requisitos previos

- GNS3 (o EVE-NG / CML) con soporte para apéndices Docker.
- Docker instalado en la GNS3 VM.
- Imágenes Cisco IOSv importadas para el switch.
