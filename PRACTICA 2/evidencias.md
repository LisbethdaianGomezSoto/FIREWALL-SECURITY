<p align="center">
  <img src="https://img.shields.io/badge/FORTIGATE-EVIDENCIAS-red?style=for-the-badge&logo=fortinet" alt="Evidencias">
</p>

<h1 align="center">📸 Evidencias del Laboratorio</h1>

<p align="center">
  <em>Capturas de pantalla y registros que respaldan cada configuración y prueba realizada.</em><br>
  Volver al <a href="../README.md">README principal</a>
</p>

---

## 📑 Índice de evidencias

| # | Sección | Estado |
| :---: | :--- | :---: |
| 1 | [FortiGate — Configuración](#1-fortigate--configuración) | 🔄 |
| 2 | [Switch — Configuración](#2-switch--configuración) | 🔄 |
| 3 | [WEB-Server](#3-web-server) | 🔄 |
| 4 | [DB-Server](#4-db-server) | 🔄 |
| 5 | [Pruebas de conectividad y políticas](#5-pruebas-de-conectividad-y-políticas) | 🔄 |
| 6 | [Protección del WEB-Server (DPI, SQLi, cuarentena)](#6-protección-del-web-server) | 🔄 |
| 7 | [Filtrado de archivos .exe](#7-filtrado-de-archivos-exe) | ✅ |
| 8 | [Rate limiting / DoS](#8-rate-limiting--dos) | 🔄 |

> Cambia el ícono de cada fila a ✅ conforme vayas subiendo la evidencia real de esa sección.

---

## 1. FortiGate — Configuración

### 1.1 Interfaces
<!-- ![Interfaces](../screenshots/fortigate/01-interfaces.png) -->

Captura de **Network > Interfaces** mostrando `port1`, `port2.10`, `port3` y `port4` con sus direcciones IP configuradas.

---

### 1.2 Ruta por defecto
<!-- ![Ruta por defecto](../screenshots/fortigate/02-ruta-defecto.png) -->

Captura de **Network > Static Routes**, o de la salida `get router info routing-table all`, mostrando la ruta `0.0.0.0/0` vía `port1`.

---

### 1.3 NAT
<!-- ![NAT](../screenshots/fortigate/03-nat-policy.png) -->

Política `USERS-TO-INTERNET` con la opción NAT activada (Use Outgoing Interface Address).

---

### 1.4 Objetos de red
<!-- ![Objetos](../screenshots/fortigate/04-objetos-direccion.png) -->

**Policy & Objects > Addresses** mostrando `Red-Usuarios`, `WEB-Server` y `DB-Server`.

---

### 1.5 Tabla de políticas
<!-- ![Políticas](../screenshots/fortigate/05-tabla-politicas.png) -->

Tabla completa de **Firewall Policy** con las 4 políticas en el orden correcto de evaluación.

---

### 1.6 Servidor DHCP
<!-- ![DHCP](../screenshots/fortigate/06-dhcp-server.png) -->

Configuración del DHCP Server en `port2.10`, rango `10.7.1.10 – 10.7.1.120`.

---

### 1.7 Perfil DPI
<!-- ![DPI](../screenshots/fortigate/07-dpi-perfil.png) -->

Perfil **SSL/SSH Inspection** en modo Full SSL Inspection.

---

### 1.8 Sensor IPS
<!-- ![IPS](../screenshots/fortigate/08-ips-sensor.png) -->

Sensor `IPS-SQLI` con la firma de SQL Injection y la acción configurada (Block / Quarantine).

---

### 1.9 File Filter
<!-- ![File Filter](../screenshots/fortigate/09-file-filter.png) -->

Perfil `BLOCK-EXE` con la regla sobre archivos `.exe` vía HTTP.

---

### 1.10 DoS Policy
<!-- ![DoS Policy](../screenshots/fortigate/10-dos-policy.png) -->

**Policy & Objects > IPv4 DoS Policy** con los umbrales de anomalía configurados.

---

## 2. Switch — Configuración

### 2.1 Running-config
<!-- ![Running-config](../screenshots/switch/01-running-config.png) -->

Salida completa de `show running-config` en la consola de SW-1.

---

### 2.2 VLAN
<!-- ![VLAN](../screenshots/switch/02-vlan-brief.png) -->

Salida de `show vlan brief` mostrando la VLAN 10 con sus puertos asignados.

---

### 2.3 Port-security
<!-- ![Port-security](../screenshots/switch/03-port-security.png) -->

Salida de `show port-security interface` para los puertos de usuarios.

---

## 3. WEB-Server

### 3.1 Sitio cargando
<!-- ![WEB-Server](../screenshots/web-server/01-pagina-https.png) -->

Página del WEB-Server cargando correctamente vía `https://10.7.1.130`.

---

### 3.2 Configuración de red
<!-- ![Red WEB-Server](../screenshots/web-server/02-configuracion-red.png) -->

IP configurada dentro del contenedor/VM del WEB-Server.

---

## 4. DB-Server

### 4.1 Servicio activo
<!-- ![DB-Server activo](../screenshots/db-server/01-servicio-mariadb.png) -->

Servicio MariaDB corriendo dentro del DB-Server.

---

### 4.2 Configuración de red
<!-- ![Red DB-Server](../screenshots/db-server/02-configuracion-red.png) -->

IP configurada dentro del contenedor/VM del DB-Server.

---

## 5. Pruebas de conectividad y políticas

### 5.1 NAT / Salida a Internet — ✅ Confirmado
<!-- ![Ping Internet](../screenshots/tests/01-nat-ping-internet.png) -->

`ping 8.8.8.8` desde la PC de usuarios, con respuesta exitosa.

---

### 5.2 Acceso HTTPS al WEB-Server — ✅ Confirmado
<!-- ![HTTPS WEB-Server](../screenshots/tests/02-https-web-cargando.png) -->
<!-- ![Log Accept](../screenshots/tests/02-log-accept-web.png) -->

Página cargando en el navegador + log de **Forward Traffic** con resultado *Accept* bajo la política `WEB-TO-USUARIOS`.

---

### 5.3 Bloqueo Usuarios → DB-Server — ✅ Confirmado
<!-- ![PowerShell Deny](../screenshots/tests/03-block-db-powershell.png) -->
<!-- ![Log Deny](../screenshots/tests/03-log-deny-db.png) -->

`Test-NetConnection -Port 3306` con `TcpTestSucceeded: False` + log *Deny* bajo `BLOQUEO-USER-DB`.

---

### 5.4 WEB-Server → DB-Server (3306 permitido) — ✅ Confirmado
<!-- ![Web a DB 3306](../screenshots/tests/04-web-a-db-3306-ok.png) -->

Conexión TCP exitosa desde la consola del WEB-Server hacia el puerto 3306 del DB-Server.

---

### 5.5 Bloqueo WEB-Server → otros puertos — ✅ Confirmado
<!-- ![Web a DB bloqueado](../screenshots/tests/05-web-a-db-otro-bloqueado.png) -->

Conexión fallida desde el WEB-Server hacia el puerto 22 (u otro distinto de 3306) del DB-Server.

---

## 6. Protección del WEB-Server

### 6.1 DPI — Certificado interceptado
<!-- ![Certificado DPI](../screenshots/tests/06-dpi-certificado.png) -->

Certificado del sitio visto desde el navegador, mostrando que fue emitido por Fortinet.

---

### 6.2 Payload de SQL Injection bloqueado
<!-- ![SQLi payload](../screenshots/tests/07-sqli-payload-bloqueado.png) -->

Solicitud con el payload `' OR '1'='1` rechazada por el FortiGate.

---

### 6.3 Log de IPS
<!-- ![Log IPS](../screenshots/tests/07-ips-log.png) -->

**Log & Report > Intrusion Prevention** con el evento detectado y la firma que hizo match.

---

### 6.4 Cuarentena del atacante
<!-- ![Cuarentena](../screenshots/tests/07-cuarentena.png) -->

**Dashboard > Quarantine** mostrando la IP atacante en cuarentena.

---

## 7. Filtrado de archivos .exe

### 7.1 Descarga bloqueada — ✅ Confirmado

![Descarga .exe bloqueada](../screenshots/tests/08-exe-bloqueado.png)

Al intentar descargar `test.exe` desde `https://10.7.1.130/prueba.exe`, la conexión fue interrumpida por el FortiGate (`ERR_CONNECTION_RESET`), confirmando que el perfil File Filter `BLOCK-EXE` bloqueó la descarga correctamente.

---

## 8. Rate limiting / DoS

### 8.1 Ráfaga de tráfico generada
<!-- ![Flood cliente](../screenshots/tests/09-dos-flood-cliente.png) -->

Consola de la PC de usuarios generando la ráfaga de conexiones/paquetes de prueba.

---

### 8.2 Detección de la anomalía
<!-- ![Log Anomaly](../screenshots/tests/09-dos-log-anomaly.png) -->

**Log & Report > Anomaly** mostrando la detección del umbral superado y la acción de bloqueo aplicada.

---

<p align="center">
  <sub>Cómo agregar una captura: sube la imagen a <code>screenshots/&lt;carpeta&gt;/</code> con el nombre indicado, y en este archivo quita el <code>&lt;!--</code> y <code>--&gt;</code> que envuelve la línea de la imagen correspondiente para que se muestre.</sub>
</p>
