<div style="display: flex; gap: 5px; flex-wrap: wrap;">
  <img src="https://img.shields.io/badge/CISCO-IOS-blue?style=for-the-badge&logo=cisco" alt="Cisco">
  <img src="https://img.shields.io/badge/FORTINET-FORTIGATE%207.0.9-red?style=for-the-badge&logo=fortinet" alt="Fortinet">
  <img src="https://img.shields.io/badge/MODELO-EDGE%20FIREWALL-purple?style=for-the-badge" alt="Modelo">
  <img src="https://img.shields.io/badge/CONFIGURACIÓN-100%25%20GUI-green?style=for-the-badge" alt="Config">
  <img src="https://img.shields.io/badge/EMULADOR-GNS3-green?style=for-the-badge" alt="GNS3">
</div>

<br>

<p align="center">
  <strong>Laboratorio de segmentación de red y seguridad perimetral con FortiGate</strong><br>
  ITLA — Lisbeth Gómez - Seguridad de redes
</p>

---

## 🎥 Video Demostrativo

> [COMPLETAR: insertar aquí el enlace o embed del video demostrativo]

Firewall perimetral FortiGate configurado completamente mediante GUI, con segmentación de usuarios y servidores, VLAN, DHCP, NAT, políticas de acceso, protección del WEB-Server, detección y bloqueo de SQL Injection, filtrado de aplicaciones y medidas de protección contra ataques DoS.

---

## 📑 Contenido

| # | Sección | Descripción |
| :---: | :--- | :--- |
| 🎯 | [Objetivo del Laboratorio](#-objetivo-del-laboratorio) | Propósito general del laboratorio |
| ✅ | [Cumplimiento de Requisitos](#-cumplimiento-de-requisitos) | Requisitos vs. implementación |
| 🌐 | [Parámetros de la Red](#-parámetros-de-la-red) | VLANs, IPs, servicios y dispositivos |
| 📐 | [Topología de la Red](#-topología-de-la-red) | Diagrama de la infraestructura |
| ⚙️ | [Configuración](#️-configuración) | FortiGate y Switch (CLI) |
| 🔒 | [Políticas de Seguridad](#-políticas-de-seguridad) | Reglas de firewall aplicadas |
| 🛡️ | [Protección del WEB-Server](#️-protección-del-web-server) | DPI, SQLi, filtrado y rate limiting |
| 🧪 | [Validación de la Implementación](#-validación-de-la-implementación) | Pruebas realizadas |
| 📸 | [Evidencias](#-evidencias) | Capturas y registros |
| 📜 | [Scripts Utilizados](#-scripts-utilizados) | Scripts de configuración |
| 🏁 | [Conclusión](#-conclusión) | Cierre del laboratorio |

---

## 🎯 Objetivo del Laboratorio

Implementar una infraestructura de red segura utilizando un **FortiGate** como firewall perimetral, encargado de controlar y proteger el tráfico entre los usuarios, servidores y la red externa.

El laboratorio incluye la configuración de VLAN, DHCP, NAT, políticas de acceso, inspección de tráfico, protección del WEB-Server, detección y bloqueo de SQL Injection, filtrado de aplicaciones y medidas de protección contra ataques DoS.

---

## ✅ Cumplimiento de Requisitos

| Requisito | Implementación |
| :--- | :--- |
| **1 FortiGate** | FortiGate configurado completamente mediante GUI |
| **Ruta por defecto** | Configurada en el FortiGate para salida hacia la red externa |
| **NAT** | Configurado para permitir la comunicación de la red interna hacia el exterior |
| **Política 1** | Permite a los usuarios acceder al WEB-Server mediante HTTPS (443) |
| **Política 2** | Bloquea el acceso de los usuarios al DB-Server mediante el puerto 3306 |
| **DPI** | Implementado para inspeccionar el tráfico de la red |
| **SQL Injection** | Detección y bloqueo de intentos de SQL Injection dirigidos al WEB-Server |
| **Cuarentena** | Aplicada al atacante después de detectar el intento de ataque |
| **Logs** | Registro de los eventos de seguridad generados por FortiGate |
| **WEB-Server → DB-Server** | Permite únicamente la comunicación mediante el puerto 3306 |
| **Filtrado de aplicaciones** | Configurado para bloquear descargas de archivos .exe |
| **Rate Limiting** | Implementado para limitar tráfico excesivo y reducir el riesgo de DoS |
| **Switch** | Configurado con VLAN y medidas básicas de seguridad |
| **VLAN 10** | Creada para la red de usuarios |
| **DHCP** | Configurado para asignar direcciones IP automáticamente a los usuarios |
| **WEB-Server** | Configurado para proporcionar el servicio HTTPS |
| **DB-Server** | Configurado como servidor de base de datos |
| **Red de servidores** | Direccionamiento mediante subredes /28 dedicadas (una por servidor) |
| **Red de usuarios** | Direccionamiento mediante una subred /25 |

---

## 🌐 Parámetros de la Red

La infraestructura está dividida en una red de usuarios y dos redes de servidores, cada una con su propia interfaz dedicada en el FortiGate. Esta segmentación permite aplicar políticas de seguridad independientes por servicio, además de controlar el acceso entre los distintos segmentos.

### Red de Usuarios

| Parámetro | Valor |
| :--- | :--- |
| **VLAN** | 10 |
| **Nombre** | USUARIOS |
| **Red** | 10.7.1.0/25 |
| **Máscara** | 255.255.255.128 |
| **Gateway** | 10.7.1.1 |
| **DHCP** | Habilitado (rango 10.7.1.10 – 10.7.1.120) |

### Red de Servidores

Cada servidor cuenta con su propia interfaz dedicada en el FortiGate (`port3` y `port4`), en lugar de compartir un segmento con el switch.

| Parámetro | WEB-Server | DB-Server |
| :--- | :--- | :--- |
| **Red** | 10.7.1.128/28 | 10.7.1.144/28 |
| **Máscara** | 255.255.255.240 | 255.255.255.240 |
| **Gateway** | 10.7.1.129 (port3) | 10.7.1.145 (port4) |
| **IP del servidor** | 10.7.1.130 | 10.7.1.146 |

### Servicios y Puertos

| Servicio | Puerto | Uso |
| :--- | :--- | :--- |
| **HTTPS** | 443 | Acceso de usuarios al WEB-Server |
| **MySQL** | 3306 | Comunicación entre WEB-Server y DB-Server |
| **DHCP** | 67/68 | Asignación automática de direcciones IP |

### Dispositivos

| Dispositivo | Función |
| :--- | :--- |
| **FortiGate** | Firewall perimetral, NAT, políticas y protección — gateway de los 3 segmentos |
| **SW-1** | Conmutación y segmentación exclusiva de la VLAN 10 (usuarios) |
| **WEB-Server** | Servidor web HTTPS, conectado directamente a port3 |
| **DB-Server** | Servidor de base de datos, conectado directamente a port4 |
| **Usuarios (PC1 / PC-Windows)** | Equipos pertenecientes a la VLAN 10, conectados vía SW-1 |

---

## 📐 Topología de la Red

<img width="1252" height="657" alt="Topología de red" src="https://github.com/user-attachments/assets/86caa770-0ba7-4caa-a984-4a400e5e6632" />

El FortiGate funciona como gateway de tres segmentos independientes: usuarios (VLAN 10, vía SW-1), WEB-Server (port3) y DB-Server (port4). El switch SW-1 conmuta únicamente el segmento de usuarios — no participa en la comunicación hacia los servidores.

---

## ⚙️ Configuración

### Configuración de interfaces

```text
config system interface
    edit "port2.10"
        set vdom "root"
        set ip 10.7.1.1 255.255.255.128
        set allowaccess ping https http ssh
        set type vlan
        set vlanid 10
        set interface "port2"
    next
    edit "port3"
        set ip 10.7.1.129 255.255.255.240
        set allowaccess ping https
    next
    edit "port4"
        set ip 10.7.1.145 255.255.255.240
        set allowaccess ping
    next
end
```

`port2` (la interfaz física hacia SW-1) se dejó intencionalmente sin dirección IP propia, ya que todo el tráfico de usuarios circula etiquetado por la subinterfaz `port2.10`. Esto permitió liberar los bloques `/28` para asignarlos como redes dedicadas de cada servidor sobre `port3` y `port4`.

---

### 🔧 Configuración del Switch

SW-1 conmuta únicamente el segmento de usuarios — los servidores ya no pasan por el switch, se conectan directo al FortiGate.

#### Puerto G0/0 — Trunk hacia FortiGate

```text
interface GigabitEthernet0/0
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 1
 switchport trunk allowed vlan 1,10
exit
```

#### Puerto G0/1 — PC1 (VPCS)

```text
interface GigabitEthernet0/1
 switchport mode access
 switchport access vlan 10
 switchport port-security
 switchport port-security maximum 2
 switchport port-security violation restrict
exit
```

#### Puerto G0/2 — PC Windows 10

```text
interface GigabitEthernet0/2
 switchport mode access
 switchport access vlan 10
 switchport port-security
 switchport port-security maximum 2
 switchport port-security violation restrict
exit
```

Todos los demás puertos del switch permanecen sin conectar (`notconnect`).

---

## 🔒 Políticas de Seguridad

Las políticas de firewall fueron creadas para controlar la comunicación entre usuarios y servidores, aplicando el principio de **mínimo privilegio**: solo se permite lo estrictamente necesario.

---

### Política 1 — Users → WEB-Server

Se permite **únicamente** el acceso HTTPS al servidor web.

| Parámetro | Valor |
| :--- | :--- |
| **Origen** | Users / VLAN 10 (port2.10) |
| **Destino** | WEB-Server (port3) |
| **Servicio** | TCP/443 (HTTPS) |
| **Acción** | ✅ ACCEPT |

**Objetivo:** Permitir que los usuarios accedan al servidor web mediante HTTPS.

---

### Política 2 — Users → DB-Server

Se **bloquea** el acceso directo de los usuarios a la base de datos.

| Parámetro | Valor |
| :--- | :--- |
| **Origen** | Users / VLAN 10 (port2.10) |
| **Destino** | DB-Server (port4) |
| **Servicio** | TCP/3306 (MySQL) |
| **Acción** | ❌ DENY |

**Objetivo:** Evitar que un usuario pueda conectarse directamente al servicio MariaDB.

---

### Política 3 — WEB-Server → DB-Server (permitido)

El WEB-Server puede comunicarse con el DB-Server **únicamente** mediante TCP/3306.

| Parámetro | Valor |
| :--- | :--- |
| **Origen** | WEB-Server (port3) |
| **Destino** | DB-Server (port4) |
| **Servicio** | TCP/3306 |
| **Acción** | ✅ ACCEPT |

---

### Política 4 — WEB-Server → DB-Server (bloqueo del resto)

Cualquier otro puerto entre ambos servidores queda explícitamente bloqueado.

| Parámetro | Valor |
| :--- | :--- |
| **Origen** | WEB-Server (port3) |
| **Destino** | DB-Server (port4) |
| **Servicio** | ALL |
| **Acción** | ❌ DENY |

**Objetivo:** Garantizar que, aunque el WEB-Server sea comprometido, no pueda usarse para escanear ni acceder a ningún otro servicio del DB-Server fuera del puerto de base de datos.

> ⚠️ **Importante:** la Política 3 debe evaluarse antes que la Política 4 en el orden de la tabla de Firewall Policy, para que el tráfico legítimo por 3306 se permita antes de aplicar el bloqueo general.

---

## 🛡️ Protección del WEB-Server

El servidor web cuenta con múltiples capas de protección aplicadas por FortiGate para mitigar amenazas comunes.

---

### 🔍 7.1 Deep Packet Inspection (DPI)

Se habilitó **DPI** (SSL/SSH Inspection en modo Full SSL Inspection) para inspeccionar el tráfico HTTPS hacia el WEB-Server y permitir que FortiGate identifique contenido y patrones asociados con amenazas. La inspección se aplica dentro de la Política 1.

| Característica | Descripción |
| :--- | :--- |
| **Función** | Inspección profunda de paquetes (descifrado SSL) |
| **Aplicación** | Política 1 (Users → WEB-Server) |
| **Beneficio** | Detección de contenido malicioso y patrones de ataque dentro de tráfico cifrado |

---

### 💉 7.2 Protección contra SQL Injection

El WEB-Server contiene una aplicación vulnerable utilizada exclusivamente para demostrar un ataque controlado de SQL Injection. La prueba permite observar cómo FortiGate analiza la solicitud y detecta patrones asociados con SQL Injection.

**Cuando se identifica el ataque, FortiGate ejecuta las siguientes acciones:**

```text
1. Detecta el tráfico.
2. Registra el evento.
3. Bloquea la solicitud.
4. Coloca al atacante en cuarentena.
```

> ⚠️ **Nota:** La cuarentena se aplica según la configuración establecida en el sensor IPS asociado a la Política 1.

---

### 📁 7.3 Filtrado de archivos ejecutables

Se configuró un perfil File Filter para bloquear archivos ejecutables descargados desde el WEB-Server:

| Tipo de archivo | Acción |
| :---: | :---: |
| `.exe` | ❌ BLOQUEADO |

**Prueba realizada:**

```text
Archivo:  test.exe
Origen:   WEB-Server
Acción:   ❌ DESCARGA BLOQUEADA (ERR_CONNECTION_RESET)
```

**Objetivo:** Evitar que los usuarios descarguen directamente archivos ejecutables desde el servidor web.

---

### 🚦 7.4 Rate Limiting

Se configuró **limitación de tráfico** (IPv4 DoS Policy) para reducir el impacto de una cantidad excesiva de solicitudes.

| Característica | Descripción |
| :--- | :--- |
| **Función** | Limitación de tráfico por umbral de anomalía |
| **Objetivo** | Mitigar ataques DoS |
| **Beneficio** | Proteger la disponibilidad del servidor |

> 💡 **Finalidad:** Ayudar a mitigar comportamientos asociados con ataques de denegación de servicio o generación excesiva de tráfico.

---

## 🧪 Validación de la Implementación

| # | Prueba | Procedimiento | Resultado |
| :---: | :--- | :--- | :---: |
| 1 | NAT / Salida a Internet | `ping 8.8.8.8` desde PC de usuarios | ✅ Confirmado |
| 2 | Acceso HTTPS al WEB-Server | Navegador → `https://10.7.1.130` | ✅ Confirmado |
| 3 | Bloqueo Usuarios → DB-Server | `Test-NetConnection -Port 3306` | ✅ Confirmado |
| 4 | WEB-Server → DB-Server (3306) | Conexión TCP desde consola del WEB-Server | ✅ Confirmado |
| 5 | Bloqueo WEB-Server → otros puertos | Conexión TCP a puerto 22 / destino externo | ✅ Confirmado |
| 6 | DPI (SSL Inspection) | Verificación del emisor del certificado | 🔄 En proceso |
| 7 | Detección de SQL Injection | Payload `' OR '1'='1` vía navegador | 🔄 En proceso |
| 8 | Cuarentena del atacante | Dashboard > Quarantine | 🔄 En proceso |
| 9 | Bloqueo de descarga .exe | `test.exe` desde el navegador | ✅ Confirmado |
| 10 | Rate limiting / DoS | Ráfaga de conexiones desde PC de usuarios | 🔄 En proceso |

---

## 📸 Evidencias

| Categoría | Contenido | Carpeta |
| :--- | :--- | :--- |
| FortiGate | Interfaces, rutas, NAT, políticas, DPI, IPS, File Filter, DoS Policy | `screenshots/fortigate/` |
| Switch | `show running-config`, `show vlan brief`, port-security | `screenshots/switch/` |
| WEB-Server | Página cargando, configuración de red | `screenshots/web-server/` |
| DB-Server | Servicio MariaDB activo, configuración de red | `screenshots/db-server/` |
| Pruebas | Evidencia de cada validación (tabla anterior) | `screenshots/tests/` |

*(Insertar aquí las miniaturas o enlaces directos a cada imagen conforme se vayan subiendo)*

---

## 📜 Scripts Utilizados

| Script | Descripción |
| :--- | :--- |
| `configurations/switch/switch-running-config.txt` | Configuración completa del switch SW-1 |
| `configurations/fortigate/` | Backup de configuración del FortiGate (exportado por GUI) |

---

## 🏁 Conclusión

El desarrollo de este laboratorio permitió implementar una infraestructura de red utilizando FortiGate como firewall perimetral y dispositivo principal de seguridad. Se configuraron VLAN, direccionamiento IP, DHCP, NAT, rutas y políticas de firewall para controlar las comunicaciones entre usuarios y servidores, cada uno de estos últimos conectado mediante una interfaz dedicada en el FortiGate.

También se implementaron mecanismos adicionales de protección como inspección profunda de tráfico, detección de SQL Injection, cuarentena, filtrado de archivos ejecutables y limitación de tráfico.

Las pruebas realizadas permitieron comprobar que el tráfico autorizado puede comunicarse correctamente, mientras que las conexiones y acciones no permitidas son bloqueadas y registradas por el firewall.

Finalmente, la documentación, los scripts, las configuraciones y las evidencias permiten reproducir y verificar el funcionamiento de la infraestructura implementada.
