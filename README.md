# HERRAMIENTADEAPOYO.NETUCOMPENSAR

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
11. Prom

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

<img width="207" height="537" alt="image" src="https://github.com/user-attachments/assets/8ce76b25-7d54-4fb2-8163-185481af1772" />

> El sistema calculará automáticamente la dirección de red, la máscara tradicional y el wildcard.

### Paso 2 — Seleccionar el vendor
En el cuerpo principal, elige el sistema operativo del equipo:
- **CISCO (IOS / IOS-XE)**
- **HUAWEI (VRP)**

<img width="517" height="217" alt="image" src="https://github.com/user-attachments/assets/37ec67db-1882-45c4-a4bf-2687d7b0e283" />

Ejemplo si escogemos Cisco:

<img width="510" height="301" alt="image" src="https://github.com/user-attachments/assets/051a1172-6ee6-4948-a112-fec6a5d518e1" />

Ejemplo si escogemos Huawei:

<img width="518" height="288" alt="image" src="https://github.com/user-attachments/assets/54bb7a95-bd00-440a-8889-71d122c848db" />

El catálogo de modelos de hardware se actualizará dinámicamente.

### Paso 3 — Explorar los protocolos
Haz clic en cada pestaña para ver la configuración del protocolo correspondiente:

- **📡 OSPF** → Copia el script generado en la interfaz CLI de tu router Cisco o Huawei en Packet Tracer.

<img width="512" height="153" alt="image" src="https://github.com/user-attachments/assets/5f679355-5821-4cca-be83-b2aa4ef7fa2a" />

- **🌐 BGP** → Revisa la IP del vecino generada automáticamente y aplica la configuración.

<img width="516" height="126" alt="image" src="https://github.com/user-attachments/assets/898af7ca-71a2-4871-9833-ede344ad65fe" />

- **🌌 IS-IS** → Selecciona si los equipos son Routers de Núcleo o Switches Multicapa; el script y la topología se adaptan en tiempo real.

<img width="514" height="190" alt="image" src="https://github.com/user-attachments/assets/4a7ba92c-c7b1-4977-bb8c-cd83d262db4f" />


### Paso 4 — Observar la topología SVG

<img width="331" height="153" alt="image" src="https://github.com/user-attachments/assets/b5e480a4-d5a5-4780-b992-6dd18852c976" />

Al desplazarte hacia abajo verás el mapa de topología física con:
- Los nodos representando tus equipos configurados.
- Animación de flujo de datos (fibra óptica láser).
- Colores que indican el protocolo activo.

<img width="515" height="112" alt="image" src="https://github.com/user-attachments/assets/080252f1-864e-4f19-be15-d4fcc5131abf" />


### Paso 5 — Aplicar en Packet Tracer
Copia el script de la caja de terminal oscura y pégalo en la CLI del equipo configurado en tu simulación de Cisco Packet Tracer.

---

## 🔬 Prueba con OSPF en Cisco Packet Tracer (Router 2811)

Como caso de prueba, se realizó la configuración del protocolo **OSPF** en un equipo **Cisco Router 2811** dentro de la plataforma **Cisco Packet Tracer**.

### Topología de prueba

| Parámetro | Valor |
|-----------|-------|
| Dirección IP Gateway | `10.10.120.1` |
| Máscara CIDR | `/24` |
| Wildcard | `0.0.0.255` |
| VLAN ID | `100` |
| Nombre VLAN | `VLAN_PRODUCCION` |
| Protocolo | OSPF — Área 0 |
| Equipo | Cisco Router 2811 |

### Script de configuración generado por la herramienta

<img width="514" height="323" alt="image" src="https://github.com/user-attachments/assets/c75ab6e8-5599-45a1-aa75-ce2ae1b0cfd0" />

Configuración realizada en cisco packet tracer:

<img width="384" height="130" alt="image" src="https://github.com/user-attachments/assets/22c2ea7c-e63b-482b-b7fb-1c650742000b" />

<img width="519" height="238" alt="image" src="https://github.com/user-attachments/assets/df3cc1ec-3561-410f-9ed4-452acb9add8b" />

<img width="519" height="238" alt="image" src="https://github.com/user-attachments/assets/09aa36d8-1b37-4324-a1c6-e2e91f3de446" />


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

# PROMPT
Contexto del Proyecto: Actúa como un Ingeniero de DevOps y Telecomunicaciones experto en desarrollo de software con Python. Debes construir una aplicación web corporativa/académica utilizando Streamlit orientada a la simulación y generación dinámica de scripts de configuración de red para estudiantes de la UCompensar. El diseño visual de la interfaz debe ser moderno y limpio, utilizando estilos avanzados de CSS embebido con una estética de modo oscuro (Glassmorphism y tonos azulados/púrpuras).

Requerimientos de Arquitectura de Red y Lógica de Datos:

Cálculo de Direccionamiento Automático: Utiliza la librería integrada ipaddress de Python para que, a partir de una dirección IP de Gateway (por defecto 172.16.10.1) y una máscara en formato CIDR seleccionadas por el usuario, el sistema calcule matemáticamente la dirección de red base y su respectiva máscara tradicional en formato decimal, así como su wildcard.

Parámetros Globales e Inputs Dinámicos: El panel lateral (st.sidebar) debe capturar las variables clave de la infraestructura: Dirección IP, Selector de Máscara CIDR (rango 24 a 30), ID de la VLAN (numérico), Nombre de la VLAN (texto) y un Deslizador (st.slider) interactivo para definir la cantidad de equipos en la topología física en un rango de 1 a 10 equipos.

Catálogo de Referencias de Hardware: Añade un menú desplegable (st.selectbox) que ofrezca al usuario una lista de modelos y referencias reales del mercado de telecomunicaciones. Esta lista debe cambiar de manera dinámica dependiendo del Vendor tecnológico seleccionado por el usuario en la interfaz:

Si el Vendor es CISCO: Mostrar modelos como ISR 4331, Catalyst 9300, ASR 1001-X, Nexus 9300.

Si el Vendor es HUAWEI: Mostrar modelos como NetEngine AR6140, CloudEngine S5735-L, NetEngine 8000 M1A, CloudEngine S6730-H.

Estructura de la Interfaz Central (Pestañas de Protocolos): El cuerpo principal de la página debe llevar el título institucional destacado: "CONFIGURACION DE PROTOCOLOS BGP,ISIS O OSPF UCOMPENSAR". Debe dividirse obligatoriamente en tres pestañas independientes (st.tabs) para los protocolos principales de enrutamiento:

Pestaña OSPF (Link-State): Debe mostrar una tarjeta teórica explicativa del protocolo y una caja de terminal oscura con comandos reales de sintaxis IOS para Cisco o VRP para Huawei (dependiendo del radio button activo), inyectando dinámicamente las variables calculadas (VLAN, IP, Máscara, Wildcard, etc.).

Pestaña BGP (Path-Vector): Debe simular la configuración de vecindades BGP calculando lógicamente una IP adyacente para el comando neighbor o peer.

Pestaña IS-IS (ISO CLNS): Debe incluir una característica exclusiva: un selector de radio (st.radio) que permita elegir la naturaleza de los "Sistemas Intermedios", alternando entre Routers de Núcleo (Core) o Switches Multicapa (Capa 3).

Requerimiento Gráfico Avanzado (Topología Dinámica en SVG): Al final de la página, embebe un componente HTML que renderice un mapa de topología física autogenerado en formato SVG que responda en tiempo real a las interacciones del usuario:

Debe graficar exactamente la cantidad de nodos configurados en el slider (hasta 10).

Los enlaces deben simular cables de fibra óptica con un haz láser animado que fluye continuamente a lo largo de la red (stroke-dashoffset).

Lógica de color adaptativa: El color de los cables láser y los bordes de los equipos debe cambiar dinámicamente según la pestaña del protocolo que se esté visualizando: Celeste (#38bdf8) para OSPF, Amarillo/Ámbar (#fbbf24) para BGP y Púrpura (#a855f7) para IS-IS.

Lógica de figuras adaptativa: Si en la pestaña IS-IS el usuario selecciona "Switches Multicapa", los círculos clásicos de los routers deben transformarse en cajas cuadradas con el icono trad
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
## Uso de Inteligencia Artificial como Apoyo
Durante el desarrollo de este proyecto se utilizaron herramientas de inteligencia artificial generativa como apoyo en la elaboración del código, la documentación y la estructura del repositorio. Los asistentes de IA utilizados fueron:
Herramienta	Uso principal
Google Gemini	Apoyo en la generación del prompt de arquitectura, sugerencias de diseño de interfaz y revisión de lógica de configuración de protocolos
Claude (Anthropic)	Apoyo en la redacción de la documentación técnica, estructuración del README y generación del código base de la aplicación Streamlit
> **Nota importante:** Todo el contenido técnico generado por las herramientas de IA fue revisado, validado y comprobado en la práctica por los integrantes del equipo. Las configuraciones de los protocolos OSPF, BGP e IS-IS fueron verificadas directamente en **Cisco Packet Tracer** usando equipos Router 2811, confirmando que los scripts generados por la aplicación producen resultados correctos y reproducibles en un entorno real de simulación.
>
> El uso de IA se enmarca en un modelo de **colaboración humano-máquina**: la IA actuó como asistente de productividad, pero la comprensión técnica, las decisiones de diseño, las pruebas y la validación final fueron responsabilidad exclusiva del equipo estudiantil.
---
📄 Licencia
Este proyecto es de uso educativo y académico, desarrollado en el marco del programa de Ingeniería de Sistemas y Telecomunicaciones de la Universidad Compensar.
---
<div align="center">
  <sub>Construido con ❤️ para los estudiantes de UCompensar · 2026</sub>
</div>
Este proyecto es de uso **educativo y académico**, desarrollado en el marco del programa de Ingeniería de Sistemas y Telecomunicaciones de la Universidad Compensar.
