# TRABAJO-FINAL-DE-REDES-WAN
# 🌐 APLICATIVO REDES WAN — UCompensar

> **Herramienta educativa de automatización y configuración de protocolos de enrutamiento WAN (OSPF, BGP, IS-IS) para estudiantes de Ingeniería de Sistemas y Telecomunicaciones.**

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue?logo=python)](https://www.python.org/)
[![Streamlit](https://img.shields.io/badge/Streamlit-1.x-FF4B4B?logo=streamlit)](https://streamlit.io/)
[![Licencia MIT](https://img.shields.io/badge/Licencia-MIT-green)](LICENSE)
[![Estado](https://img.shields.io/badge/Estado-Activo-brightgreen)]()

---

## 📋 Tabla de Contenidos

1. [Descripción del Proyecto](#-descripción-del-proyecto)
2. [Problemática](#-problemática)
3. [Justificación](#-justificación)
4. [Funcionalidades](#-funcionalidades)
5. [Arquitectura de la Herramienta](#-arquitectura-de-la-herramienta)
6. [Tecnologías Utilizadas](#-tecnologías-utilizadas)
7. [Instalación y Ejecución](#-instalación-y-ejecución)
8. [Guía de Uso — Paso a Paso](#-guía-de-uso--paso-a-paso)
9. [Prueba con OSPF en Cisco Packet Tracer](#-prueba-con-ospf-en-cisco-packet-tracer-router-2811)
10. [Integrantes](#-integrantes)

---

## 📌 Descripción del Proyecto

**APLICATIVO REDES WAN** es una herramienta web interactiva desarrollada en **Python con Streamlit**, diseñada como guía educativa para que los estudiantes de la **Universidad Compensar (UCompensar)** puedan aprender, simular y generar configuraciones reales de los protocolos de enrutamiento dinámico más utilizados en redes WAN empresariales:

| Protocolo | Tipo | Caso de Uso Principal |
|-----------|------|-----------------------|
| **OSPF** | Link-State (IGP) | Redes corporativas jerárquicas |
| **BGP** | Path-Vector (EGP) | Interconexión de sistemas autónomos / Internet |
| **IS-IS** | Link-State (ISO CLNS) | Redes de operadores y proveedores de servicio |

La aplicación genera automáticamente comandos de configuración reales para equipos **Cisco (IOS/IOS-XE)** y **Huawei (VRP)**, adaptados a los parámetros de red que el estudiante ingresa en tiempo real.

---

## 🔍 Problemática

Los laboratorios de redes WAN exigen que el estudiante configure manualmente protocolos complejos como OSPF, BGP e IS-IS sobre simuladores como Cisco Packet Tracer o GNS3. Este proceso implica:

- Calcular manualmente direcciones de red, máscaras y wildcards.
- Memorizar la sintaxis específica de cada vendor (Cisco vs. Huawei).
- No contar con una referencia inmediata que adapte la configuración a su propio direccionamiento IP.
- Invertir tiempo en errores de sintaxis en lugar de enfocarse en entender la lógica del protocolo.

**Esta herramienta resuelve ese cuello de botella** proporcionando una interfaz guiada que genera los scripts de configuración correctos de forma dinámica y visual.

---

## ✅ Justificación

Automatizar la generación de configuraciones de red en un entorno académico es fundamental porque:

1. **Reduce la curva de aprendizaje**: El estudiante puede enfocarse en entender los conceptos del protocolo, no en depurar errores de tipeo.
2. **Es reproducible**: Cada configuración generada puede ser copiada directamente en Packet Tracer o un equipo real.
3. **Cubre dos vendors líderes del mercado**: Cisco y Huawei representan el mayor porcentaje del equipamiento WAN empresarial en Colombia y América Latina.
4. **Integra teoría y práctica**: Cada pestaña de protocolo incluye una explicación teórica acompañada del script de configuración correspondiente.
5. **Visualiza la topología**: El mapa SVG dinámico permite al estudiante entender la disposición física de los equipos que está configurando.

---

## ✨ Funcionalidades

### 🎛️ Panel Lateral — Parámetros Globales
- Entrada de **dirección IP / Gateway** configurable.
- Selector de **máscara CIDR** (rango /24 a /30).
- **Cálculo automático** de dirección de red, máscara tradicional y wildcard usando la librería `ipaddress` de Python.
- Campos para **ID y nombre de VLAN**.
- **Slider interactivo** para definir la cantidad de equipos en la topología (1 a 10).

### 📡 Pestañas de Protocolos

**OSPF (Open Shortest Path First)**
- Explicación teórica del protocolo (tipo, métrica, algoritmo Dijkstra).
- Script de configuración completo para Cisco IOS o Huawei VRP, con variables dinámicas inyectadas (VLAN, IP, máscara, wildcard, área 0).

**BGP (Border Gateway Protocol)**
- Explicación teórica (vector de ruta, sistemas autónomos, atributos).
- Generación automática de la IP del vecino BGP (neighbor/peer) calculada lógicamente desde la IP de gateway.
- Script de configuración para iBGP en Cisco IOS o Huawei VRP.

**IS-IS (Intermediate System to Intermediate System)**
- Explicación teórica (protocolo de capa 2 ISO, preferido por ISPs).
- Selector de tipo de nodo: **Routers de Núcleo** o **Switches Multicapa (Capa 3)**.
- Script de configuración adaptado al tipo de nodo y vendor seleccionado.

### 🏷️ Catálogo de Hardware Dinámico
Según el vendor seleccionado, la aplicación ofrece modelos reales del mercado:

| Cisco | Huawei |
|-------|--------|
| ISR 4331 | NetEngine AR6140 |
| Catalyst 9300 | CloudEngine S5735-L |
| ASR 1001-X | NetEngine 8000 M1A |
| Nexus 9300 | CloudEngine S6730-H |

### 🗺️ Mapa de Topología SVG Dinámico
- Renderizado en tiempo real según la cantidad de equipos del slider.
- **Color adaptativo por protocolo**: Celeste (OSPF), Ámbar (BGP), Púrpura (IS-IS).
- **Animación de fibra óptica**: Haz láser continuo entre los nodos.
- **Figuras adaptativas**: En IS-IS con Switches, los nodos cambian de círculos (router) a cuadrados con ícono de switch y etiquetas `SW_L3_N`.

---

## 🏗️ Arquitectura de la Herramienta

```
APLICATIVO-REDES-WAN/
│
├── app.py                  # Aplicación principal Streamlit
├── requirements.txt        # Dependencias del proyecto
├── README.md               # Documentación del repositorio
└── assets/                 # (Opcional) Imágenes y recursos estáticos
```

### Flujo de datos interno

```
Usuario ingresa parámetros (IP, CIDR, VLAN, vendor)
        │
        ▼
calcular_infraestructura()   ← librería ipaddress
        │ ip_red, mascara, wildcard
        ▼
Pestañas OSPF / BGP / IS-IS
        │ variables dinámicas
        ▼
Generación de script (IOS o VRP)
        │
        ▼
Visualización terminal-box + Topología SVG
```

---

## 🛠️ Tecnologías Utilizadas

| Herramienta | Versión | Uso |
|-------------|---------|-----|
| Python | 3.9+ | Lenguaje principal |
| Streamlit | 1.x | Framework de interfaz web |
| ipaddress | stdlib | Cálculo de direccionamiento IP |
| HTML/CSS embebido | — | Estilos Glassmorphism / modo oscuro |
| SVG + Animaciones CSS | — | Topología de red dinámica |
| Cisco Packet Tracer | 8.x | Simulador para pruebas de configuración |

---

## 🚀 Instalación y Ejecución

### Requisitos previos
- Python 3.9 o superior instalado.
- `pip` disponible en el sistema.

### 1. Clonar el repositorio

```bash
git clone https://github.com/edwarm1301-coder/APLICATIVO-REDES-WAN.git
cd APLICATIVO-REDES-WAN
```

### 2. Instalar dependencias

```bash
pip install streamlit
```

> La librería `ipaddress` viene incluida en la biblioteca estándar de Python; no requiere instalación adicional.

### 3. Ejecutar la aplicación

```bash
streamlit run app.py
```

### 4. Abrir en el navegador

La aplicación se abre automáticamente en:
```
http://localhost:8501
```

---

## 📖 Guía de Uso — Paso a Paso

### Paso 1 — Configurar los parámetros globales
En el **panel lateral izquierdo**:
1. Ingresa la dirección IP del gateway (por defecto `172.16.10.1`).
2. Selecciona la máscara CIDR en el selector desplegable (ej. `/24`).
3. Define el ID de VLAN (ej. `100`) y el nombre de la VLAN (ej. `VLAN_PRODUCCION`).
4. Mueve el slider para definir cuántos equipos tendrá tu topología (1 a 10).

> El sistema calculará automáticamente la dirección de red, la máscara tradicional y el wildcard.

### Paso 2 — Seleccionar el vendor
En el cuerpo principal, elige el sistema operativo del equipo:
- **CISCO (IOS / IOS-XE)**
- **HUAWEI (VRP)**

El catálogo de modelos de hardware se actualizará dinámicamente.

### Paso 3 — Explorar los protocolos
Haz clic en cada pestaña para ver la configuración del protocolo correspondiente:

- **📡 OSPF** → Copia el script generado en la interfaz CLI de tu router Cisco o Huawei en Packet Tracer.
- **🌐 BGP** → Revisa la IP del vecino generada automáticamente y aplica la configuración.
- **🌌 IS-IS** → Selecciona si los equipos son Routers de Núcleo o Switches Multicapa; el script y la topología se adaptan en tiempo real.

### Paso 4 — Observar la topología SVG
Al desplazarte hacia abajo verás el mapa de topología física con:
- Los nodos representando tus equipos configurados.
- Animación de flujo de datos (fibra óptica láser).
- Colores que indican el protocolo activo.

### Paso 5 — Aplicar en Packet Tracer
Copia el script de la caja de terminal oscura y pégalo en la CLI del equipo configurado en tu simulación de Cisco Packet Tracer.

---

## 🔬 Prueba con OSPF en Cisco Packet Tracer (Router 2811)

Como caso de prueba, se realizó la configuración del protocolo **OSPF** en un equipo **Cisco Router 2811** dentro de la plataforma **Cisco Packet Tracer**.

### Topología de prueba

| Parámetro | Valor |
|-----------|-------|
| Dirección IP Gateway | `172.16.10.1` |
| Máscara CIDR | `/24` |
| Dirección de Red | `172.16.10.0` |
| Wildcard | `0.0.0.255` |
| VLAN ID | `100` |
| Nombre VLAN | `VLAN_PRODUCCION` |
| Protocolo | OSPF — Área 0 |
| Equipo | Cisco Router 2811 |

### Script de configuración generado por la herramienta

```ios
! === CONFIGURACIÓN OSPF EN CISCO IOS ===
! Modelo de Equipo: ISR 4331 (Router de Servicios Integrados Empresarial)
vlan 100
 name VLAN_PRODUCCION
!
interface GigabitEthernet0/1
 description Enlace_Trunk_Hacia_LAN
 no shutdown
!
interface GigabitEthernet0/1.100
 description Subinterfaz Gateway VLAN VLAN_PRODUCCION
 encapsulation dot1Q 100
 ip address 172.16.10.1 255.255.255.0
!
router ospf 1
 router-id 1.1.1.1
 network 172.16.10.0 0.0.0.255 area 0
```

### Comandos de verificación OSPF

Una vez aplicada la configuración en el Router 2811, valida con estos comandos `show`:

```ios
! Verificar vecindades OSPF establecidas
show ip ospf neighbor

! Verificar la tabla de enrutamiento (rutas O = OSPF)
show ip route ospf

! Ver información general del proceso OSPF
show ip ospf

! Inspeccionar las interfaces participantes en OSPF
show ip ospf interface brief
```

### Resultado esperado

Al ejecutar `show ip ospf neighbor` deberías ver el estado **FULL** en la adyacencia con el router vecino, confirmando que el protocolo OSPF convergió correctamente en el Área 0.

```
Neighbor ID     Pri   State           Dead Time   Address         Interface
2.2.2.2           1   FULL/DR         00:00:35    172.16.10.2     GigabitEthernet0/1.100
```

> 📸 Las capturas de pantalla de la simulación en Packet Tracer se encuentran en el archivo `Explicacion_y_pantallazos.docx` adjunto al proyecto.

---

## 👥 Integrantes

| Nombre | Rol |
|--------|-----|
| **Brayan Iván Ribón Quintero** | Desarrollo Backend / Lógica de protocolos |
| **Edwar Roberto Martínez Pinto** | Desarrollo Frontend / Diseño de interfaz |
| **Jeimy Lorena Niño Correa** | Documentación técnica / Pruebas en Packet Tracer |

**Universidad:** Universidad Compensar — UCompensar  
**Materia:** Enrutamiento IP Avanzado / Redes WAN  
**Fecha de entrega:** 26 de mayo de 2026

---

## 📄 Licencia

Este proyecto es de uso **educativo y académico**, desarrollado en el marco del programa de Ingeniería de Sistemas y Telecomunicaciones de la Universidad Compensar.

---
