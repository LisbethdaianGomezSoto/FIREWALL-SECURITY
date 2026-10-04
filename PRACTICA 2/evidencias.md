<p align="center">
  <img src="https://img.shields.io/badge/FORTIGATE-EVIDENCIAS-red?style=for-the-badge&logo=fortinet" alt="Evidencias">
</p>

<h1 align="center">📸 Evidencias del Laboratorio</h1>

<p align="center">
  <em>Capturas de pantalla y registros que respaldan cada configuración y prueba realizada.</em><br>
</p>

---


## 📑 Índice de evidencias

| # | Sección | Contenido | Estado |
| :---: | :--- | :--- | :---: |
| 1 | [FortiGate — Configuración](#1-fortigate--configuración) | Interfaces, ruta, NAT, objetos, políticas, DHCP, IPS, File Filter y DoS | ✅ |
| 2 | [Switch — Configuración](#2-switch--configuración) | VLAN y Port-security | ✅ |
| 3 | [WEB-Server](#3-web-server) | Sitio cargando y configuración de red | ✅ |
| 4 | [DB-Server](#4-db-server) | Servicio activo y configuración de red | ✅ |
| 5 | [Pruebas de conectividad y políticas](#5-pruebas-de-conectividad-y-políticas) | NAT, HTTPS, bloqueos y acceso al puerto 3306 | ✅ |
| 6 | [Protección del WEB-Server](#6-protección-del-web-server) | SQLi, log de IPS y cuarentena | ✅ |
| 7 | [Filtrado de archivos .exe](#7-filtrado-de-archivos-exe) | Descarga bloqueada | ✅ |
| 8 | [Rate limiting / DoS](#8-rate-limiting--dos) | Ráfaga de tráfico y detección | ✅ |



---

## 1. FortiGate — Configuración 

### 1.1 Interfaces 

 <img width="527" height="213" alt="image" src="https://github.com/user-attachments/assets/b0d0fb30-a15b-4ef6-b515-7b453ab73c85" />


---

### 1.2 Ruta por defecto 

<img width="1122" height="133" alt="image" src="https://github.com/user-attachments/assets/d7958afd-3367-43ff-8842-3c3aa5a8c07d" />
 

---

### 1.3 NAT 

<img width="1115" height="140" alt="image" src="https://github.com/user-attachments/assets/82b716e3-9a8a-415a-b1f6-1b06e6a11c4a" />


---

### 1.4 Objetos de red 

<img width="402" height="142" alt="image" src="https://github.com/user-attachments/assets/5cdb67af-a51c-47ab-8b94-d2a8dd82b155" />


---

### 1.5 Tabla de políticas 

<img width="1107" height="233" alt="image" src="https://github.com/user-attachments/assets/1aff7967-dc9e-4dd1-ac27-97f86e8f4774" />


---

### 1.6 Servidor DHCP 

<img width="477" height="390" alt="image" src="https://github.com/user-attachments/assets/29687b29-211b-4720-9e4b-05813912dcbf" />

---



### 1.7 Sensor IPS 


<img width="747" height="461" alt="image" src="https://github.com/user-attachments/assets/3afbaf36-86c1-46d2-b3b3-ac2c0f03e3fa" />


---

### 1.8 File Filter 

<img width="553" height="331" alt="image" src="https://github.com/user-attachments/assets/a5070278-d535-4b23-9b90-dfbb493eb827" />


---


### 1.9 DoS Policy 

<img width="1092" height="106" alt="image" src="https://github.com/user-attachments/assets/cc7ad1b1-6b07-48fd-a799-0aca70cb3892" />


---


## 2. Switch — Configuración 


### 2.1 VLAN 

<img width="518" height="155" alt="image" src="https://github.com/user-attachments/assets/7740fe3d-52d9-4fea-80b2-29aeb5f230ad" />


---


### 2.2 Port-security 

<img width="512" height="162" alt="image" src="https://github.com/user-attachments/assets/125578d0-a786-4d12-ac72-693ea8563104" />


---

## 3. WEB-Server

### 3.1 Sitio cargando 

<img width="1026" height="633" alt="Captura de pantalla 2026-09-25 072051" src="https://github.com/user-attachments/assets/1e6d3b2d-935b-49ca-ba61-e8a55f8bef4e" />

---

### 3.2 Configuración de red 

<img width="315" height="82" alt="image" src="https://github.com/user-attachments/assets/2d6a529e-f264-432c-bea2-8f1a6685463d" />


---

## 4. DB-Server

### 4.1 Servicio activo 


<img width="643" height="212" alt="Captura de pantalla 2026-09-25 195218" src="https://github.com/user-attachments/assets/9174e17c-0f09-4be0-b42d-0b8d1ea4d033" />


---

### 4.2 Configuración de red 

<img width="196" height="95" alt="image" src="https://github.com/user-attachments/assets/9d6ba2ad-c66f-48ff-b6dc-bcc628ae49a1" />


---

## 5. Pruebas de conectividad y políticas

### 5.1 NAT / Salida a Internet 

<img width="501" height="122" alt="Captura de pantalla 2026-09-25 153557" src="https://github.com/user-attachments/assets/26da7d6e-faf3-49ab-a43c-18c0539591a2" />


---

### 5.2 Acceso HTTPS al WEB-Server 


<img width="1026" height="633" alt="Captura de pantalla 2026-09-25 072051" src="https://github.com/user-attachments/assets/216d6d26-bef3-400d-8e98-3b674379a3bd" />


<img width="817" height="527" alt="Captura de pantalla 2026-09-24 230221" src="https://github.com/user-attachments/assets/29c1f800-f263-4dbc-b867-5c21cadd87aa" />


---

### 5.3 Bloqueo Usuarios → DB-Server 

<img width="517" height="225" alt="Captura de pantalla 2026-09-25 200004" src="https://github.com/user-attachments/assets/bc73f98e-9427-4ae0-9fbb-60708cc62aa4" />


<img width="1272" height="320" alt="Captura de pantalla 2026-09-25 195912" src="https://github.com/user-attachments/assets/dfa35608-237e-41cb-a550-401c97791932" />


---

### 5.4 WEB-Server → DB-Server (3306 permitido) 

<img width="567" height="36" alt="image" src="https://github.com/user-attachments/assets/67dbbede-43e2-4e64-94dd-58b38ab0989e" />


---

### 5.5 Bloqueo WEB-Server → otros puertos 

<img width="237" height="36" alt="image" src="https://github.com/user-attachments/assets/fa816de2-7591-4c7e-b56a-71179cc87904" />


---

## 6. Protección del WEB-Server

### 6.1 Payload de SQL Injection bloqueado 

<img width="933" height="331" alt="image" src="https://github.com/user-attachments/assets/6804e5c3-e30a-4ba1-9b4b-916ff18a049e" />


---

### 6.2 Log de IPS 

<img width="1305" height="333" alt="Captura de pantalla 2026-09-25 201715" src="https://github.com/user-attachments/assets/36d38204-29f8-4df6-94b3-1dd8db32978c" />


---

### 6.3 Cuarentena del atacante 

<img width="1233" height="461" alt="Captura de pantalla 2026-09-25 202315" src="https://github.com/user-attachments/assets/18cffc1d-92ba-4580-ad26-6761de23a0d6" />


---

## 7. Filtrado de archivos .exe

### 7.1 Descarga bloqueada 

<img width="590" height="366" alt="image" src="https://github.com/user-attachments/assets/168da10d-7d52-4930-83ec-d7ca119f5d88" />


---

## 8. Rate limiting / DoS

### 8.1 Ráfaga de tráfico generada 

<img width="573" height="512" alt="Captura de pantalla 2026-09-25 203532" src="https://github.com/user-attachments/assets/43ec8859-27cf-4a59-bc58-ff3b6b82a0a5" />

---

### 8.2 Detección de la anomalía 


<img width="1346" height="270" alt="Captura de pantalla 2026-09-25 173854" src="https://github.com/user-attachments/assets/1deb318e-29bf-417c-bdc7-855a005ff12c" />


---

<p align="center">
 
