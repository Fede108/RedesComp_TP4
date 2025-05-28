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

## Parte II: Simulaciones y Análisis

### 1) Análisis de la configuración BGP y evidencia de que el protocolo está activo
Para verificar que BGP está funcionando correctamente, primero abrimos sesión en cada router y ejecutamos `show ip bgp summary`.

En Router 0:

![image](https://github.com/user-attachments/assets/f5698255-35e5-4cd4-a5b9-fe724fd90d72)

![image](https://github.com/user-attachments/assets/69885c8a-abf0-4aac-967f-a59bdd066a28)

En Router 1:

![image](https://github.com/user-attachments/assets/a624653d-2d1f-40b7-a396-829f80678856)

![image](https://github.com/user-attachments/assets/b84e9ff9-faa5-49b4-8a15-8359c233c552)

En la salida de R1 pude ver una línea como:
```shell
10.0.0.1  …  Up/Down 00:00:53  State/PfxRcd 4
```
Salida la cual indica que la sesión eBGP con su vecino en AS100 ha llegado a estado Established y ha recibido 4 prefijos (PfxRcd 4). 
A continuación usamos show ip bgp para listar todos los prefijos conocidos por BGP y sus atributos (Next Hop, AS-PATH, origen). Por ejemplo, en R1 aparecían tanto su red local 192.168.2.0/24 con Next Hop 0.0.0.0 (prefijo propio) como la red 192.168.1.0/24 aprendida de R0 con Next Hop 10.0.0.1. Finalmente, con show ip route bgp confirmamos que esas rutas se habían instalado en la RIB, pues R1 mostraba:
```
B 192.168.1.0/24 [20/0] via 10.0.0.1
```
donde la “B” indica que proviene de BGP y, por tanto, el router usará esa ruta para reenviar tráfico hacia AS100.

---

### 2) Comprobación de la conectividad entre hosts de cada AS
Desde h0 a h2:

![image](https://github.com/user-attachments/assets/bbdec887-4387-4ea8-b750-70d4c13ae3c3)

Desde h0 a h3:

![image](https://github.com/user-attachments/assets/7ed9d1d1-0546-4139-aa9b-01c8360eb462)

Desde h1 a h2:

![image](https://github.com/user-attachments/assets/86169c3d-07b9-49fc-aefa-2d8e1c83d7cf)

Desde h1 a h3:

![image](https://github.com/user-attachments/assets/8b8ef529-46b0-4ed4-9538-92128a3cdf54)


Con la vecindad BGP ya establecida y las rutas correctamente instaladas, hicimos pings cruzados desde los hosts de AS100 (h0: `192.168.1.2` y h1: `192.168.1.3`) hacia los hosts de AS200 (h2: `192.168.2.2` y h3: `192.168.2.3`) y viceversa. En los primeros intentos (h0 → h2/h3) obtuvimos 25% de _packet loss_ porque las rutas aún se estaban propagando; al repetir el ping obtuvimos 0% de pérdida, lo que confirma que cada router reenvía los paquetes a través de BGP de forma correcta.

---

### 3) Simulación de tráfico y caída/recuperación de un router
Para entender cómo reacciona BGP ante una interrupción, apagué intencionadamente Router 1 (AS200) y observé en R0 los mensajes capturados por Wireshark dentro de Packet Tracer (Foto 3). Primero apareció un **TCP FIN** de la conexión BGP y, tras reiniciar R1, un **three-way handshake** sobre el puerto 179. A continuación vi los mensajes **OPEN** desde ambos extremos y varios **KEEPALIVE** hasta que la sesión volvió a _Established_, y finalmente los **UPDATE** con los prefijos intercambiados. Esta secuencia demuestra cómo BGP restablece la vecindad y anuncia las rutas al recuperarse.

![image](https://github.com/user-attachments/assets/b96332bb-7de3-47d8-a5cb-1aa542e885eb)

R0 ( 10.0.0.1) tambien envia el mensaje de OPEN a R1 (10.0.0.2)

![image](https://github.com/user-attachments/assets/50000d31-a855-4f66-ab94-1de4f9960fe0)

Cada uno recibe el OPEN correspondiente y envía el mensaje KEEPALIVE para establecer la conexión.

![image](https://github.com/user-attachments/assets/21d03a5d-936c-4946-8e2a-7041b02f37ef)

![image](https://github.com/user-attachments/assets/fd96f488-ae58-42a0-a53c-1d49618fb57b)

Una vez que se establece la conexión empieza el intercambiar rutas (UPDATEs)

![image](https://github.com/user-attachments/assets/e4b4da81-95b3-4844-9ddc-4a0ce9091c9b)

![image](https://github.com/user-attachments/assets/a693d3f0-e4f2-45f2-a232-b978832b5bf8)

---

### 4) Configuración IPv6 y verificación de conectividad entre ambos AS
En esta etapa añadimos direcciones IPv6 a todas las interfaces de R0, R1 y los PCs, y habilitamos `ipv6 unicast-routing` en cada router. Asignamos a R0 las direcciones `2001:DB8:1::1/64` (backbone) y `2001:DB8:2::1/64` (AS100 interna), y a R1 `2001:DB8:1::2/64` (backbone) y `2001:DB8:3::1/64` (AS200 interna). Sin embargo, el soporte de Packet Tracer no permite mandar paquetes IPv6 mediante el protocolo BGP, por lo que este ejercicio no pudo ser resuelto.

---

### 5) Documentación del diseño de la red en tabla
Para dejar constancia clara de la topología, armamos la siguiente tabla con cada dispositivo, interfaz, las direcciones IPv4 y IPv6, las máscaras y un comentario sobre su función:

| Equipo   | Interfaz         | Red IPv4     | IP IPv4      | Máscara | IPv6              | Comments       |
|----------|------------------|--------------|--------------|---------|-------------------|----------------|
| Router0  | FastEthernet0/0  | 10.0.0.0     | 10.0.0.1     | /24     | 2001:DB8:1::1/64  | Backbone eBGP  |
| Router0  | FastEthernet0/1  | 192.168.1.0  | 192.168.1.1  | /24     | 2001:DB8:2::1/64  | AS100 interna  |
| Router1  | FastEthernet0/0  | 10.0.0.0     | 10.0.0.2     | /24     | 2001:DB8:1::2/64  | Backbone eBGP  |
| Router1  | FastEthernet0/1  | 192.168.2.0  | 192.168.2.1  | /24     | 2001:DB8:3::1/64  | AS200 interna  |

---

### 6) Incorporación de R2, switch y quinto host (h4) en AS100
Se añadió un tercer router (R2) al AS100, conectando su FastEthernet0/0 a R0 (`192.168.10.2/24` ↔ `192.168.10.1/24`) y conectando un switch a su FastEthernet0/1. Al switch se conectó el host h4 configurado con `192.168.3.2/24` y puerta de enlace `192.168.3.1`. Con ello quedó una nueva LAN interna (`192.168.3.0/24`) dentro de AS100.

![image](https://github.com/user-attachments/assets/d6d27e74-2649-493a-bd3c-7dafb1f35026)

---

### 7) Configuración de rutas estáticas en AS100
Para que R0 y R2 conozcan mutuamente todas las subredes de AS100 se usaron estos comandos:

**En R0:**
```
ip route 192.168.3.0 255.255.255.0 192.168.10.2
ip route 192.168.2.0 255.255.255.0 10.0.0.2
```
**En R2:**
```
ip route 192.168.1.0 255.255.255.0 192.168.10.1
ip route 192.168.2.0 255.255.255.0 192.168.10.1
```
**En R1:**
```
ip route 192.168.3.0 255.255.255.0 10.0.0.1
```

Estas rutas permiten que cualquier host de AS100 y AS200 se comunique con h4.

---

### 8) Redistribución de OSPF en BGP y análisis de configuraciones
Para integrar dinámicamente la red de AS100 en BGP, primero habilitamos OSPF en AS100:

**En R0:**
```
router ospf 1
network 10.0.0.0 0.0.0.255 area 0
network 192.168.1.0 0.0.0.255 area 0
network 192.168.10.0 0.0.0.255 area 0
```

**En R2:**
```
router ospf 1
network 192.168.10.0 0.0.0.255 area 0
network 192.168.3.0 0.0.0.255 area 0
```

Verificamos que ambos routers formaran adyacencia OSPF y tuvieran las rutas con marcador “O” en la RIB. Luego, en R0 dentro del proceso BGP (AS100) configuramos:
```
address-family ipv4 unicast
redistribute ospf 1
```
Así, BGP comenzó a anunciar automáticamente todas las rutas OSPF de AS100 a R1. En R1 comprobamos con `show ip bgp` que aparecían `192.168.3.0/24` y `192.168.10.0/24` junto con las redes originales de AS100, y `show ip route bgp` confirmaba que las instalaba marcadas como “B”.

Rutas ospf en router 2 dentro de AS100:

![image](https://github.com/user-attachments/assets/2860def5-9fe1-4122-8d73-64c714da05a4)

Tabla BGP router 0:

![image](https://github.com/user-attachments/assets/3362e0cd-4ec8-4001-8eac-4390d9802cc5)

Tabla BGP router 1:

![image](https://github.com/user-attachments/assets/32c629b3-6232-419d-9564-f72beb89a377)

Redistribuir OSPF en el router 2 a través del router 1 por BGP:

![image](https://github.com/user-attachments/assets/65ad8606-dae4-4469-8143-102b9f16ff1f)

Tabla de rutas router 0:

![image](https://github.com/user-attachments/assets/754e5ff4-10d1-4f38-a08c-e48ea89044ee)

Tabla de rutas router 1 con las rutas aprendidas por OSPF en AS100:

![image](https://github.com/user-attachments/assets/78aef518-013f-43ea-a7f9-dba309f8084e)

Tabla de rutas router 2:

![image](https://github.com/user-attachments/assets/6f1a1b4b-31e0-438f-b37d-2b525b2b8b16)

---

### 9) Verificación final de conectividad con h4 en ambos AS
Por último, realizamos los siguientes pings:
- Desde h4 (`192.168.3.2`) hacia h2 (`192.168.2.2`) y h0 (`192.168.1.2`).
- Desde h2 y h3 hacia h4.
- Desde h0 y h1 también hacia h4.

Todos los pings fueron exitosos, demostrando que la redistribución OSPF→BGP y las rutas estáticas estaban correctamente configuradas, garantizando conectividad total entre los cinco hosts en los dos AS.

ping de pc1 a pc4:

![image](https://github.com/user-attachments/assets/86c3d20b-22cc-4d6b-932a-7c530bbe69ec)

ping de pc4 a pc2:

![image](https://github.com/user-attachments/assets/e47f3899-5097-46c3-a098-b8698be09de4)
