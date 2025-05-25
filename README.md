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

### Ejemplos de ASN de organizaciones.

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

### **ASN de mi conexion actual.**

El ASN de la conexion actual es AS7303 (Telecom Argentina S.A.)
  - **Tasa de Trafico**: 10-20 Tbps
  - **Alcance Geográfico**: Regional
  - **Protocolos compatibles**: IPv4 Unicast, IPv6 
---

### ¿Qué es el Border Gateway Protocol (BGP)?

El protocolo de puerta de enlace fronteriza (BGP) es un conjunto de reglas que determinan las mejores rutas de red para la transmisión de datos en Internet. Internet se compone de miles de redes privadas, públicas, corporativas y gubernamentales vinculadas entre sí mediante protocolos, dispositivos y tecnologías de comunicación estandarizados. Cuando navega por Internet, los datos viajan a través de varias redes antes de llegar a su destino. La responsabilidad del BGP es analizar todas las rutas disponibles por las que podrían viajar los datos y seleccionar la mejor ruta. Por ejemplo, cuando un usuario de los Estados Unidos carga una aplicación con servidores de origen en Europa, el BGP hace que la comunicación sea rápida y eficiente.

---

### Funcionamiento del BGP.

**1. Adquisición de vecino (Neighbor Acquisition).**

Los pares de BGP intercambian información de enrutamiento con los pares de BGP cercanos a través de la información de accesibilidad de la capa de red (NLRI) y los atributos de ruta. Después de intercambiar información, cada par de BGP puede generar un gráfico de las conexiones de red a su alrededor.
* Los routers BGP establecen una conexión TCP para iniciar una sesión de enrutamiento.
* Intercambian mensajes OPEN para negociar versión, número de AS y otros parámetros.

**2. Detección de vecino alcanzable (Neighbor Reachability Detection).**

Una vez establecida la sesión, se envían mensajes KEEPALIVE para confirmar la conectividad. Si no se recibe un KEEPALIVE en el tiempo acordado, se termina la sesión.

**3. Detección de red alcanzable (Reachable Network Detection).**

Los enrutadores de BGP utilizan la información almacenada para dirigir el tráfico de manera óptima. El factor principal en la selección de rutas es la ruta más corta, según lo determinado por los gráficos de rutas almacenados.
* Se utilizan mensajes UPDATE para anunciar o retirar rutas.
* Estos mensajes contienen atributos de ruta y NLRI (Network Layer Reachability Information).

---

### Tipos de mensajes en BGP.

* **OPEN**: Inicia una sesión BGP. Incluye versión, AS, identificador del router y opciones.
* **KEEPALIVE**: Mantiene activa la sesión y verifica que el vecino siga disponible.
* **UPDATE**: Anuncia nuevas rutas o retira rutas previamente anunciadas.
* **NOTIFICATION**: Informa errores y finaliza la sesión de forma inmediata.

**Formato de los paquetes BGP.**

* Cabecera de 19 bytes:

  * **Marker**: 16 bytes para autenticación.
  * **Length**: 2 bytes con la longitud total del mensaje.
  * **Type**: 1 byte para el tipo de mensaje (1 a 4).

---

### Diferencias entre eBGP e iBGP.
El protocolo de puerta de enlace fronteriza (BGP) se clasifica como interno o externo en función del lugar al que se dirijan los datos. Los enrutadores de BGP externos conectan un sistema autónomo a Internet a nivel mundial. Sin embargo, los sistemas autónomos grandes se componen a su vez de sistemas autónomos más pequeños. El BGP interno dirige los datos en un sistema.
La principal diferencia entre la interconexión de BGP interna y externa es la forma en la que la ruta de BGP recibida de un par se propaga de forma predeterminada a otros pares. 
 - Las nuevas rutas aprendidas de un par de BGP externo se vuelven a anunciar a todos los pares.
 - Las nuevas rutas aprendidas de un par de BGP interno se vuelven a anunciar solo a todos los pares externos.


| Característica            | eBGP (Externo)                    | iBGP (Interno)                                    |
| ------------------------- | --------------------------------- | ------------------------------------------------- |
| Ámbito de operación       | Entre diferentes AS               | Dentro del mismo AS                               |
| Propagación de rutas      | Se reanuncian a pares eBGP e iBGP | No se reanuncian a otros pares iBGP               |
| Requisito de conectividad | Conexión directa (TTL=1)          | Puede usar loopback; no requiere conexión directa |


La funcion que se envia dentro de un AS tiene sus diferencias:

**eBGP (entre AS distintos)**

 - AS_PATH: Cada vez que enviamos un prefijo a un vecino eBGP, el router añade su propio ASN al atributo AS_PATH. Por tanto, si vemos que un prefijo lleva “…, AS1” al comienzo, sabemos que viene directamente de un vecino eBGP que pertenece al AS 1.

 - NEXT_HOP: El router eBGP suele reescribir el next-hop al IP de su propia interfaz hacia el vecino eBGP.

**iBGP (dentro del mismo AS)**

 - AS_PATH: No se modifica. El atributo AS_PATH de las rutas que llegan por iBGP permanece tal cual se las enviaron al vecino iBGP.

 - NEXT_HOP:Tampoco se cambia, conserva el next-hop que llegó por eBGP originalmente.

![image](https://github.com/user-attachments/assets/598e6a06-d813-46f6-8601-1b2f692d9ebd)

Un Sistema Autónomo (AS) de tránsito es aquel que permite que el tráfico de datos pase a través de su red, aunque ni el origen ni el destino final de ese tráfico pertenezcan a su propia infraestructura. En este caso el AS de transito es el AS2. 

---

El AS de mi conexion actual, AS7303, cuenta con 16 conexiones. Algunas de ellas son: 

![image](https://github.com/user-attachments/assets/52fea907-33fe-4d7a-80cb-5ce712ed9e95)

Usando la red 4g el AS es AS22927 Telefonica de Argentina. Se puede observar que existe una conexion entre las dos redes. Ademas, las conexiones se ven mejor distribuidas o ordenadas. 

![image](https://github.com/user-attachments/assets/53dfc26e-0304-4665-b504-97931758c245)

---

### Problemas de enrutamiento BGP.

Uno de los incidentes más significativos en la historia del enrutamiento BGP fue el Incidente del AS 7007, ocurrido el 25 de abril de 1997. 

 **Causas del incidente.**
 
El problema se originó cuando un enrutador del sistema autónomo AS7007, filtró accidentalmente una parte de su tabla de rutas al resto de Internet. Debido a un error de software, estas rutas fueron desagregadas en prefijos /24, más específicos que las rutas existentes, y el atributo AS_PATH fue reescrito para indicar que el origen de todas esas rutas era el propio AS7007. Como resultado, los routers de otros sistemas autónomos comenzaron a preferir estas rutas más específicas, redirigiendo una gran cantidad de tráfico hacia AS7007. Este tráfico sobrecargó su infraestructura, convirtiéndolo en un "agujero negro" donde los datos eran descartados.

**Consecuencias.**
- Interrupción global: El tráfico de una parte significativa de Internet fue redirigido incorrectamente, afectando a numerosos proveedores de servicios y usuarios finales.

- Pérdida de conectividad: Muchas redes experimentaron pérdida de conectividad, ya que el tráfico era enviado a AS7007 y luego descartado debido a la sobrecarga.
Kentik

- Revisión de prácticas: El incidente llevó a una revisión de las prácticas de enrutamiento BGP, destacando la necesidad de implementar filtros de rutas y mecanismos de validación más robustos para prevenir la propagación de información incorrecta.
