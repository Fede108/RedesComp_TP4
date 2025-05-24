# Trabajo práctico 4:Ruteo externo dinámico (Border Gateway Protocol (BGP)) y sistemas autónomos (Autonomous Systems (AS))

*Facultad de Ciencias Exactas, Fisicas y Naturales de la U.N.C**

**Redes de Computadoras**

**Profesores**
- Santiago M. Henn
- Facundo N. Oliva C.
  
**Nombre del grupo : Kiritoro** 

**Integrantes**
- Federico Cechich
- Juan Manuel Ferrero
- Luciano Trachta


**23 de Mayo de 2025**


**Información de contacto**:  _juan.manuel.ferrero@mi.unc.edu.ar_, _federico.cechich@mi.unc.edu.ar_, _ltrachta@mi.unc.edu.ar_ 

---

### ¿Qué es un Sistema Autónomo (AS)?

Un **Sistema Autónomo (AS)** es un conjunto de redes IP y routers bajo el control de una misma entidad administrativa que presenta una política de enrutamiento común. Es un componente esencial en la arquitectura de Internet, permitiendo la comunicación entre diferentes redes. Cada AS se identifica por un número único llamado **ASN (Autonomous System Number)**.

#### Características principales:

* **Control administrativo único**: administrado por un ISP, universidad, empresa, etc.
* **Política de enrutamiento común**: todos los routers siguen las mismas reglas.
* **Identificador único (ASN)**: permite identificar al AS en Internet.

#### Tipos de AS:

* **Stub**: conectado a un solo AS; no permite tránsito.
* **Multihomed**: conectado a varios AS; no permite tránsito.
* **Transit**: conectado a varios AS y permite tránsito.

---

### ¿Qué es un Autonomous System Number (ASN) y cómo está conformado?

El **ASN** es un identificador único asignado a un Sistema Autónomo. Es esencial para el enrutamiento mediante BGP.

#### Formatos de ASN:

* **16 bits**: de 1 a 65535 (original)
* **32 bits**: de 0 a 4,294,967,295 (actual); notación "X.Y"

#### Tipos de ASN:

* **Públicos**: asignados por registros regionales (RIR) como LACNIC; visibles en Internet.
* **Privados**: para uso interno; no deben anunciarse globalmente.

  * Rango 16 bits: 64512 a 65534
  * Rango 32 bits: 4200000000 a 4294967294

#### Importancia:

* Identificación de la red
* Intercambio de rutas BGP
* Implementación de políticas de enrutamiento

---

### Ejemplos de ASN de organizaciones

#### 1. **Google – AS15169**

* **Organización**: Google LLC
* **Tipo**: Empresa tecnológica global
* **Uso**: Para servicios como YouTube, Gmail, Google Search

#### 2. **AT\&T – AS7018**

* **Organización**: AT\&T Services, Inc.
* **Tipo**: Proveedor de servicios de Internet (ISP)
* **Uso**: Enrutamiento de tráfico para usuarios en EE.UU.

#### 3. **CERN – AS513**

* **Organización**: Organización Europea para la Investigación Nuclear (CERN)
* **Tipo**: Institución de investigación
* **Uso**: Enrutamiento de datos científicos globales

---

### **ASN de mi conexion actual**

El ASN de la conexion actual es AS7303 (Telecom Argentina S.A.)
  - **Tasa de Trafico**: 10-20 Tbps
  - **Alcance Geográfico**: Regional
  - **Protocolos compatibles**: IPv4 Unicast, IPv6 
---

### ¿Qué es el Border Gateway Protocol (BGP)?

El protocolo de puerta de enlace fronteriza (BGP) es un conjunto de reglas que determinan las mejores rutas de red para la transmisión de datos en Internet. Internet se compone de miles de redes privadas, públicas, corporativas y gubernamentales vinculadas entre sí mediante protocolos, dispositivos y tecnologías de comunicación estandarizados. Cuando navega por Internet, los datos viajan a través de varias redes antes de llegar a su destino. La responsabilidad del BGP es analizar todas las rutas disponibles por las que podrían viajar los datos y seleccionar la mejor ruta. Por ejemplo, cuando un usuario de los Estados Unidos carga una aplicación con servidores de origen en Europa, el BGP hace que la comunicación sea rápida y eficiente.

### 
