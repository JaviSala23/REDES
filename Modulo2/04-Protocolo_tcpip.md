# 📘 Apunte de Clase: Protocolo TCP/IP

## 1. ¿Qué es TCP/IP?
**TCP/IP** significa **Transmission Control Protocol / Internet Protocol**.  
Es el **conjunto de protocolos de comunicación estandarizados** que permiten que los dispositivos de una red, sin importar su tipo, sistema operativo o ubicación, puedan **enviar y recibir datos entre sí**.

- Funciona tanto en **redes locales (LAN)** como en **Internet**.  
- No depende del fabricante del dispositivo ni del sistema operativo: lo usan **Windows, Linux, Android, iOS, routers, impresoras de red, televisores inteligentes, etc.**  
- Gracias a TCP/IP, es posible que un mensaje enviado desde una computadora en Argentina llegue en segundos a otra en Japón, pasando por miles de dispositivos intermedios.  

📌 **Definición clave:** TCP/IP no es un protocolo único, sino una **familia completa de protocolos** que cooperan entre sí para garantizar que la información viaje desde un dispositivo hasta otro.

---

## 2. Breve historia de TCP/IP
### ARPANET: el origen
En los años **60 y 70**, el Departamento de Defensa de los Estados Unidos desarrolló una red experimental llamada **ARPANET**, que buscaba interconectar universidades y centros de investigación.  
El objetivo era que la información pudiera seguir circulando incluso si parte de la red fallaba.

### Los creadores
En 1974, **Vinton Cerf** y **Robert Kahn** presentaron el diseño de TCP/IP. Su idea principal fue crear un protocolo que:
- Permitiera conectar **redes diferentes** entre sí (inter-networking).  
- No dependiera de un único fabricante o tecnología.  
- Soportara fallas: si un nodo se caía, los datos podían tomar otra ruta.  

Por eso, TCP/IP se convirtió en la base de lo que hoy conocemos como **Internet**.

---

## 3. ¿Por qué se habla de una "familia de protocolos"?
Un protocolo es como un **conjunto de reglas** que define cómo se transmiten los datos.  
Ejemplo: cuando dos personas hablan, siguen reglas de un idioma (gramática, palabras, orden). Si uno habla en español y el otro en japonés, no se entienden. Con los dispositivos ocurre lo mismo.

👉 TCP/IP es una **colección de protocolos especializados**, cada uno con una función distinta, que trabajan juntos como un engranaje perfecto.

Algunos de los más importantes son:
- **IP (Internet Protocol):** Identifica cada dispositivo y encamina los paquetes de datos.  
- **TCP (Transmission Control Protocol):** Garantiza que los datos lleguen completos y en orden.  
- **UDP (User Datagram Protocol):** Envía datos rápidamente, sin control de errores.  
- **HTTP/HTTPS:** Permite navegar en la web.  
- **FTP:** Transfiere archivos.  
- **SMTP/POP3/IMAP:** Manejan el correo electrónico.  
- **DNS:** Traduce nombres de dominio (como `google.com`) en direcciones IP.  
- **DHCP:** Asigna direcciones IP automáticamente en una red.  
- **ICMP:** Se utiliza para diagnóstico de red (por ejemplo, el comando `ping`).  

---

## 4. ¿Qué resuelve TCP/IP?
Antes de TCP/IP, cada fabricante tenía sus propios protocolos (IBM, DEC, etc.), lo que hacía que las redes fueran **incompatibles entre sí**.  
Con TCP/IP se resolvieron varios problemas:

1. **Compatibilidad universal:** Todos los dispositivos pueden entenderse.  
2. **Escalabilidad:** Permite conectar desde 2 hasta miles de millones de equipos.  
3. **Flexibilidad:** Funciona sobre diferentes medios físicos (cable de cobre, fibra óptica, Wi-Fi, satélite).  
4. **Robustez:** Si un camino falla, los datos pueden viajar por otro.  

---

## 5. La metáfora del idioma universal
Podemos pensar en TCP/IP como el **idioma universal de las computadoras**:

- Un teléfono Android y una computadora con Windows se entienden gracias a TCP/IP.  
- Así como los humanos necesitan compartir un idioma para comunicarse, las máquinas necesitan usar los mismos protocolos.  
- Si un dispositivo habla "TCP/IP", puede conectarse a Internet.  

---

## 6. ¿Cómo funciona en la práctica?
Cuando un usuario escribe `www.google.com` en el navegador, ocurren muchos pasos invisibles:

1. **DNS** traduce el nombre de dominio a una dirección IP (por ejemplo, `142.250.184.14`).  
2. El navegador utiliza **HTTP/HTTPS**, que a su vez usa **TCP** para organizar la transmisión.  
3. **TCP** divide la solicitud en pequeños segmentos.  
4. **IP** encapsula esos segmentos en paquetes y decide la ruta más eficiente para enviarlos.  
5. Los paquetes viajan a través de routers, switches y servidores.  
6. El servidor de Google recibe los datos, responde y la información regresa al usuario.  

👉 Todo esto ocurre en **milésimas de segundo**.

---

## 7. Ejemplo comparativo
Podemos comparar el envío de datos con mandar una carta por correo:

- **IP:** Escribe la dirección del destinatario en el sobre (dirección de la casa / dirección IP).  
- **TCP:** Asegura que la carta llegue completa, y si falta una hoja, pide que se reenvíe.  
- **UDP:** Sería mandar una postal sin preocuparse si llegó o no.  
- **DNS:** Es como la guía telefónica que traduce nombres en direcciones.  
- **HTTP:** Especifica cómo está escrita la carta (formato de la web).  

---

## 8. Ejercicio práctico con comandos
Para que los alumnos puedan experimentar:  

- `ping google.com` → Verifica si hay conexión con Google (usa **ICMP**).  
- `tracert google.com` → Muestra la ruta que siguen los paquetes hasta Google.  
- `netstat -n` → Lista las conexiones activas TCP y UDP en la computadora.  

Estos simples ejercicios permiten **visualizar el trabajo real de TCP/IP**.


---

## 2. ¿Qué es una Dirección IP?

## 1. Definición básica
Una **dirección IP (Internet Protocol Address)** es un **número único que identifica a un dispositivo en una red**.  
Así como cada persona tiene un número de documento y cada casa una dirección postal, cada computadora, celular o dispositivo conectado a Internet tiene su propia **dirección IP**.

👉 Sin direcciones IP, no habría forma de saber a qué computadora enviarle la información en una red.

---

## 2. Funciones principales de una dirección IP
1. **Identificación:**  
   - Cada dispositivo conectado a la red debe tener una dirección IP única para ser reconocido.  

2. **Localización:**  
   - Una IP no solo identifica, también indica **dónde está** el dispositivo dentro de la red.  
   - Gracias a esto, los routers pueden decidir la mejor ruta para enviar los datos.  

📌 **Ejemplo cotidiano:**  
Imaginemos una ciudad: cada casa tiene un número de calle (dirección). Si querés enviar una carta, el correo necesita saber la dirección exacta. En Internet, la IP cumple el mismo papel.

---

## 3. Versiones de direcciones IP
### 🔹 IPv4
- Formato: cuatro números decimales separados por puntos.  
- Ejemplo: `192.168.0.1`.  
- Cada número va de **0 a 255**.  
- Espacio de direcciones: **2³² ≈ 4.3 mil millones** posibles direcciones.  
- Problema: con tantos dispositivos conectados hoy en día, **se están agotando las direcciones IPv4**.  

### 🔹 IPv6
- Formato: ocho grupos de números hexadecimales separados por `:`.  
- Ejemplo: `2001:0db8:85a3::8a2e:0370:7334`.  
- Espacio de direcciones: **2¹²⁸** (casi infinito).  
- Soluciona el agotamiento de IPv4.  
- Está en uso actualmente, aunque IPv4 sigue siendo mayoritario.  

---

## 4. Tipos de direcciones IP
### a) Según visibilidad
- **IP pública:**  
  - Es la dirección visible en Internet.  
  - Asignada por el proveedor de Internet (ISP).  
  - Ejemplo: `181.45.67.123`.  
- **IP privada:**  
  - Usada dentro de redes locales (hogar, oficina).  
  - No son accesibles directamente desde Internet.  
  - Rango típico: `192.168.x.x`, `10.x.x.x`, `172.16.x.x`.

### b) Según asignación
- **IP estática:**  
  - Fija, no cambia.  
  - Usada en servidores o servicios que necesitan siempre la misma dirección.  
- **IP dinámica:**  
  - Cambia cada vez que el dispositivo se conecta.  
  - Asignada automáticamente por **DHCP**.  
  - Es la más común para usuarios hogareños.

---

## 5. Clases de direcciones IPv4 (históricas)
Aunque hoy se usan más subredes (CIDR), es útil conocer las **clases clásicas** de IPv4:

| Clase | Rango de IPs            | Uso común                     |
|-------|--------------------------|-------------------------------|
| A     | 0.0.0.0 – 127.255.255.255 | Redes muy grandes             |
| B     | 128.0.0.0 – 191.255.255.255 | Redes medianas              |
| C     | 192.0.0.0 – 223.255.255.255 | Redes pequeñas              |
| D     | 224.0.0.0 – 239.255.255.255 | Multicast (especiales)      |
| E     | 240.0.0.0 – 255.255.255.255 | Investigación (reservadas)  |

---

## 6. Máscara de subred
La **máscara de subred** (subnet mask) indica qué parte de la dirección IP corresponde a la **red** y qué parte al **host (equipo específico)**.

- Ejemplo:  
  - Dirección IP: `192.168.1.10`  
  - Máscara: `255.255.255.0`  
  - Significa que la red es `192.168.1.0` y los hosts van del `1` al `254`.  

📌 Esto permite dividir grandes redes en **subredes más pequeñas**, optimizando la administración.

---

## 7. Ejemplo práctico
Supongamos que en una casa hay:
- Una computadora (`192.168.1.2`)  
- Un celular (`192.168.1.3`)  
- Una impresora Wi-Fi (`192.168.1.4`)  

Todos comparten la misma **red privada** (`192.168.1.x`) pero cada uno tiene su **IP única** para que el router sepa a dónde enviar los datos.

---

## 8. Herramientas para trabajar con direcciones IP
- `ipconfig` (Windows) → Muestra IPs del equipo.  
- `ifconfig` o `ip addr` (Linux) → Similar en Linux.  
- `ping <dirección>` → Verifica conexión.  
- `tracert` (Windows) o `traceroute` (Linux) → Muestra la ruta que sigue un paquete.  

---

## 9. Ejercicio para alumnos
1. Abrir la consola en su computadora y usar `ipconfig` o `ifconfig`.  
2. Identificar su dirección IP privada y su máscara de subred.  
3. Buscar su dirección IP pública en [https://whatismyipaddress.com](https://whatismyipaddress.com).  
4. Investigar: ¿Su IP es dinámica o estática?  
---

## 3. Capas del Modelo TCP/IP

## 1. Introducción
El modelo **TCP/IP** describe cómo se transmiten los datos a través de una red.  
Está compuesto por **4 capas**, cada una con funciones específicas, que trabajan juntas para que la comunicación sea posible.

👉 Podés imaginarlo como una **cadena de correo**: cada capa tiene un rol, como el remitente, el transportista, la oficina de correos y el destinatario.  

---

## 2. Relación con el Modelo OSI
El modelo OSI (7 capas) es más teórico, mientras que **TCP/IP es el modelo práctico** que realmente se usa en Internet.  

- OSI tiene 7 capas.  
- TCP/IP las simplifica en 4 capas.  

| Modelo OSI (7 capas)     | Modelo TCP/IP (4 capas) |
|--------------------------|--------------------------|
| Aplicación               | Aplicación              |
| Presentación             |                         |
| Sesión                   |                         |
| Transporte               | Transporte              |
| Red                      | Internet                |
| Enlace de datos          | Acceso a red            |
| Física                   |                         |

---

## 3. Capas del Modelo TCP/IP

### 🔹 1. Capa de Aplicación
- Es la capa más cercana al **usuario final**.  
- Aquí funcionan los **programas y servicios** que usamos a diario.  
- Se encarga de definir cómo las aplicaciones interactúan con la red.  

**Protocolos principales:**
- **HTTP/HTTPS:** Navegación web.  
- **FTP:** Transferencia de archivos.  
- **SMTP, POP3, IMAP:** Correo electrónico.  
- **DNS:** Traducción de nombres de dominio a direcciones IP.  

📌 **Ejemplo cotidiano:** Cuando entrás a `www.youtube.com`, tu navegador usa **HTTP/HTTPS** para comunicarse con los servidores de YouTube.

---

### 🔹 2. Capa de Transporte
- Asegura que los datos lleguen correctamente al destino.  
- Divide la información en **segmentos** y controla la comunicación entre los equipos.  

**Protocolos principales:**
- **TCP (Transmission Control Protocol):**  
  - Confiable, asegura la entrega de datos completos y en orden.  
  - Ejemplo: Navegar en la web, enviar un correo.  
- **UDP (User Datagram Protocol):**  
  - Más rápido pero sin control de errores.  
  - Ejemplo: Juegos online, streaming de video, llamadas por VoIP.  

📌 **Ejemplo cotidiano:**  
TCP es como una conversación telefónica (esperás respuesta).  
UDP es como mandar un mensaje por altavoz (rápido, pero sin confirmación).

---

### 🔹 3. Capa de Internet
- Se encarga de **direccionar** los paquetes y decidir la **ruta** que seguirán en la red.  
- Aquí aparece el concepto de **dirección IP**.  

**Protocolos principales:**
- **IP (Internet Protocol):** Identificación y direccionamiento.  
- **ICMP (Internet Control Message Protocol):** Diagnóstico y control (ejemplo: `ping`).  
- **ARP (Address Resolution Protocol):** Traduce direcciones IP en direcciones MAC.  

📌 **Ejemplo cotidiano:** Es como un GPS que decide la ruta que debe seguir un auto para llegar a destino.

---

### 🔹 4. Capa de Acceso a Red (o Enlace)
- Es la más cercana al **hardware**.  
- Define cómo los datos se transmiten físicamente en la red.  
- Incluye tarjetas de red, cables, switches, routers y señales eléctricas/ópticas.  

**Protocolos y tecnologías:**
- **Ethernet:** Redes cableadas.  
- **Wi-Fi:** Redes inalámbricas.  
- **Bluetooth, 4G/5G:** Otros medios de acceso.  

📌 **Ejemplo cotidiano:** Es como la carretera o autopista por la que circulan los autos (los datos).

---

## 4. Flujo de datos en las capas
Cuando un mensaje viaja por la red:
1. **Aplicación:** El usuario escribe un correo (SMTP).  
2. **Transporte:** TCP divide el correo en segmentos.  
3. **Internet:** IP asigna direcciones y decide la ruta.  
4. **Acceso a Red:** La tarjeta de red envía los paquetes físicamente.  

En el destino, el proceso se invierte hasta que el usuario ve el mensaje.

---

## 5. Analogía: Enviar una carta
- **Aplicación:** Escribís la carta.  
- **Transporte:** Dividís la carta en páginas y las numerás.  
- **Internet:** Escribís la dirección del destinatario.  
- **Acceso a Red:** Entregás la carta al correo para que la lleve.  

---

## 6. Ejercicio práctico
1. Hacer un `ping google.com` para ver cómo responde la **capa de Internet**.  
2. Abrir un navegador y entrar a un sitio web → **capa de Aplicación (HTTP)**.  
3. Usar `netstat -n` para ver conexiones activas → **capa de Transporte (TCP/UDP)**.  
4. Identificar si están conectados por Wi-Fi o Ethernet → **capa de Acceso a Red**.  


---

## 4. Protocolo TCP (Transmission Control Protocol)

El **Protocolo de Control de Transmisión (TCP, Transmission Control Protocol)** es uno de los protocolos más importantes del conjunto **TCP/IP**.  
Su función principal es **garantizar una comunicación fiable entre dos dispositivos en una red**.

👉 Se utiliza en la mayoría de las aplicaciones críticas de Internet: navegación web, correo electrónico, transferencia de archivos, mensajería, etc.

---

## 4.1. Características principales de TCP
1. **Orientado a conexión:**  
   - Antes de transmitir datos, establece una “conversación” entre emisor y receptor.  
   - Asegura que ambos estén listos para comunicarse.

2. **Fiabilidad:**  
   - Verifica que los datos lleguen sin errores, completos y en el mismo orden en que fueron enviados.  
   - Si algo falla, vuelve a enviar los datos.

3. **Segmentación y reensamblado:**  
   - TCP divide la información en **segmentos**.  
   - En el destino, vuelve a unirlos en el mensaje original.

4. **Control de flujo:**  
   - Regula la cantidad de datos que se envían para no saturar al receptor.  

5. **Detección de errores:**  
   - Cada segmento incluye un **checksum** para detectar si hubo alteraciones en los datos.  

---

## 4.2. El proceso de comunicación en TCP
Podemos compararlo con **una llamada telefónica**:

1. Una persona llama a otra (se establece la conexión).  
2. Se hablan y verifican que ambos se escuchan bien.  
3. Comienzan la conversación real (transferencia de datos).  
4. Al terminar, se despiden y cuelgan (cierre de conexión).  

---

## 4.3. El famoso *Three-Way Handshake*
El **three-way handshake** (apretón de manos en tres pasos) es el proceso que TCP utiliza para **establecer una conexión fiable** antes de transmitir datos.

### 🔹 Paso 1: SYN
- El cliente (emisor) envía un segmento con la bandera **SYN (synchronize)**.  
- Indica que quiere iniciar una conexión y propone un número de secuencia inicial.

### 🔹 Paso 2: SYN-ACK
- El servidor (receptor) responde con un segmento **SYN-ACK**.  
- Reconoce la solicitud (ACK) y a su vez envía su propio número de secuencia (SYN).

### 🔹 Paso 3: ACK
- El cliente envía un último segmento con la bandera **ACK**, confirmando la recepción del SYN del servidor.  
- La conexión queda establecida.

📌 **Después de este proceso, ambos pueden intercambiar datos con seguridad.**

---

## 4.4. Representación gráfica del Handshake

Cliente Servidor
| --- SYN (quiero conectar) -------> |
| <--- SYN + ACK (ok, yo también) -- |
| --- ACK (confirmado) ------------> |
| CONEXIÓN LISTA |


---

## 4.5. Flujo de datos con TCP
1. Se establece la conexión (*three-way handshake*).  
2. El emisor comienza a enviar datos en segmentos numerados.  
3. El receptor responde con **ACKs** confirmando qué segmentos recibió.  
4. Si un segmento se pierde, el emisor lo retransmite.  
5. Al finalizar, se realiza un proceso de **cierre de conexión** (four-way handshake).

---

## 4.6. Ejemplos de aplicaciones que usan TCP
- **HTTP/HTTPS** → Navegación web.  
- **SMTP, IMAP, POP3** → Correo electrónico.  
- **FTP** → Transferencia de archivos.  
- **SSH** → Conexión remota segura.  
- **Telnet** → Acceso remoto (no seguro).  

---

## 4.7. Ejercicio práctico
1. Abrir un navegador y entrar a cualquier página → se inicia automáticamente un **handshake TCP** con el servidor.  
2. Usar la herramienta **Wireshark** para capturar tráfico y observar el intercambio **SYN, SYN-ACK, ACK**.  
3. Pregunta: ¿Por qué es importante que TCP confirme los datos en aplicaciones como correo electrónico o banca online?


---

## 4.8. Protocolo UDP (User Datagram Protocol)
- Envía datos **rápidamente**, sin confirmar si llegaron.  
- Características:
  - No confiable.  
  - No orientado a conexión.  
  - Más rápido que TCP.  
- Usos: Streaming, juegos online, VoIP, DNS.  

🔹 **Ejemplo cotidiano:** UDP es como gritar algo en la calle: el mensaje sale rápido, pero no sabés si lo escucharon.

---

## 4.9. Diferencias entre TCP y UDP

| Característica     | TCP 📞 (Fiable)         | UDP 📢 (Rápido)       |
|---------------------|-------------------------|-----------------------|
| Conexión           | Orientado a conexión    | No orientado a conexión |
| Fiabilidad         | Alta (confirma entrega) | Baja (sin confirmación) |
| Velocidad          | Más lento               | Más rápido             |
| Uso típico         | Web, correo, archivos   | Streaming, juegos, VoIP |

---

## 4.10. Otros Protocolos Importantes en TCP/IP
- **HTTP/HTTPS:** Navegación web.  
- **FTP:** Transferencia de archivos.  
- **SMTP/IMAP/POP3:** Correo electrónico.  
- **DNS:** Traducción de nombres de dominio en IPs.  
- **DHCP:** Asigna IP automáticamente.  
- **ICMP:** Diagnóstico (ejemplo: `ping`).  

---

## 4.11. Funcionamiento básico del envío de datos en TCP/IP
1. El usuario escribe `www.google.com`.  
2. **DNS** traduce el nombre a IP.  
3. **TCP** segmenta la información.  
4. **IP** envía los paquetes.  
5. El dispositivo destino recibe y responde.  

👉 Todo en **milisegundos**.

---

## 4.12. Ejemplo práctico para el alumno
Abrir consola (CMD/PowerShell) y probar:  

- `ping google.com` → Test de conectividad.  
- `tracert google.com` → Ruta de los paquetes.  
- `netstat -n` → Conexiones activas TCP/UDP.  

---

## 4.13. Conclusión
- TCP/IP es el **lenguaje universal de Internet**.  
- La dirección IP identifica dispositivos.  
- TCP y UDP son los dos métodos principales de transporte.  
- Es la base de **redes, Internet y ciberseguridad**.  

---

## 📌 Ejercicios para alumnos
1. Explicar con sus palabras la diferencia entre TCP y UDP.  
2. Buscar su propia dirección IP pública (ejemplo: https://whatismyipaddress.com).  
3. Hacer un `ping` a un sitio web y explicar qué significa el resultado.  

