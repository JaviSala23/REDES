# 🔌 ¿Qué es un Socket?

## 📖 Introducción

Hasta ahora comprendimos cómo los **servicios**, los **protocolos** y las **direcciones IP y puertos** permiten que los dispositivos se comuniquen.  
El siguiente paso es entender **cómo los programas utilizan esos mecanismos** para enviar y recibir datos.  

Ahí entra en juego el concepto fundamental de las redes modernas: **el socket**.

Un **socket** es la herramienta que conecta al software con la red.  
Gracias a los sockets, una aplicación puede comunicarse con otra, incluso si están en diferentes computadoras o continentes.

> 📬 Si los protocolos definen el idioma, los sockets son los teléfonos que permiten mantener la conversación.

---

## ⚙️ Definición técnica

Un **socket** es un **punto final de comunicación** entre dos procesos (programas en ejecución) que intercambian datos a través de una red.  
En la práctica, es una **interfaz de programación** (API) proporcionada por el sistema operativo que permite a las aplicaciones conectarse y transferir información utilizando protocolos como **TCP** o **UDP**.

Cada socket se identifica de forma única mediante tres elementos:

```
Socket = (Protocolo, Dirección IP, Puerto)
```

Por ejemplo:

```
(TCP, 192.168.1.100, 8080)
```

Esto representa un punto de comunicación TCP que escucha en la IP 192.168.1.100, puerto 8080.

---

## 🧠 Breve historia

El concepto de *socket* nació en la Universidad de California, Berkeley, en la década de 1980, dentro del sistema operativo **BSD Unix**.  
De allí surgieron las **Berkeley Sockets**, una de las primeras API de red modernas.  
Con el tiempo, fueron adoptadas y adaptadas por la mayoría de los sistemas operativos, incluyendo **Linux**, **macOS** y **Windows**, lo que permitió la interoperabilidad global de las aplicaciones de red.

Hoy en día, prácticamente todos los lenguajes de programación (Python, Java, C, Go, etc.) implementan su propia biblioteca basada en este modelo.

> 🔍 Gracias a las Berkeley Sockets, nació la posibilidad de programar servicios como navegadores, servidores web, chats, y juegos en red.

---

## 🧩 Tipos de Sockets

Los sockets pueden clasificarse según el tipo de protocolo o el modo de transmisión que utilizan:

| Tipo de socket | Protocolo | Características | Ejemplo de uso |
|----------------|------------|------------------|----------------|
| **Stream (Flujo)** | TCP | Orientado a conexión, confiable, entrega en orden, control de errores y congestión. | Navegadores, servidores web, correo electrónico, APIs. |
| **Datagram (Datagrama)** | UDP | No orientado a conexión, sin confirmación, menor latencia pero sin garantía de entrega. | Streaming, videollamadas, videojuegos online. |
| **Raw (Crudo)** | IP directo | Permite construir y analizar paquetes manualmente, sin procesamiento del sistema. | Sniffers, herramientas de diagnóstico (Wireshark, Nmap). |

> 💡 En la práctica, la mayoría de las aplicaciones cotidianas usan **sockets TCP o UDP**, dependiendo de si priorizan confiabilidad o velocidad.

---

## 🧭 Roles de Cliente y Servidor

Toda comunicación mediante sockets sigue un esquema **cliente-servidor**:

- **Servidor:**  
  - Crea un socket.  
  - Lo asocia a una dirección IP y puerto (`bind()`).  
  - Espera solicitudes (`listen()`).  
  - Acepta conexiones (`accept()`).  

- **Cliente:**  
  - Crea un socket.  
  - Se conecta al servidor (`connect()`).  
  - Envía y recibe datos (`send()` / `recv()`).  
  - Cierra la conexión (`close()`).  

> 🧩 Ambos lados utilizan la misma estructura de socket, pero con funciones y responsabilidades diferentes.

---

## 🔄 Ciclo de vida de un Socket TCP

```
Servidor                              Cliente
─────────                              ─────────
socket()       ← crear socket →        socket()
bind()         ← asociar IP:puerto
listen()       ← esperar conexiones
accept()       ← aceptar cliente
                → connect()  ← conectarse
send() / recv()  ↔  intercambio de datos
close()         ← cierre de conexión →   close()
```

Cada paso se ejecuta mediante funciones específicas definidas por la API de sockets del sistema operativo.

---

## 🤝 Establecimiento de Conexión (Three-Way Handshake – TCP)

El protocolo **TCP** utiliza un proceso de tres pasos llamado *three-way handshake* para establecer una conexión confiable:

1. **SYN:** El cliente solicita conexión al servidor.  
2. **SYN-ACK:** El servidor acepta y responde afirmativamente.  
3. **ACK:** El cliente confirma y la conexión queda establecida.

```
Cliente                        Servidor
   SYN      ─────────────────►
            ◄────────────────   SYN + ACK
   ACK      ─────────────────►
```

A partir de aquí, ambos pueden intercambiar datos de manera confiable y ordenada.

> 🔐 Este proceso garantiza que ambos extremos estén listos antes de transmitir información, evitando pérdidas de paquetes.

---

## 🧰 Estados de un Socket TCP

Durante su vida, un socket puede pasar por varios **estados**, controlados por el sistema operativo:

| Estado | Descripción |
|---------|--------------|
| **LISTEN** | El servidor está esperando conexiones entrantes. |
| **SYN_SENT** | El cliente envió una solicitud de conexión. |
| **SYN_RECEIVED** | El servidor respondió con una confirmación. |
| **ESTABLISHED** | La conexión está activa y se intercambian datos. |
| **FIN_WAIT / CLOSE_WAIT** | Uno de los extremos está cerrando la conexión. |
| **TIME_WAIT** | TCP espera para asegurar que los últimos paquetes se recibieron. |
| **CLOSED** | La conexión ha finalizado completamente. |

> 🧩 Estos estados forman parte del **control de flujo y confiabilidad** que distingue a TCP de otros protocolos.

---

## ⚡ TCP vs UDP

| Característica | TCP | UDP |
|----------------|-----|-----|
| Orientación a conexión | ✅ Sí | ❌ No |
| Confirmación de entrega | ✅ Sí | ❌ No |
| Orden garantizado | ✅ Sí | ❌ No |
| Velocidad | 🐢 Más lento | ⚡ Más rápido |
| Uso típico | Web, correo, transferencia de archivos | Streaming, juegos, DNS |

> 🧠 En resumen: TCP prioriza la **confiabilidad**, mientras que UDP prioriza la **rapidez**.

---

## 🧠 Diferencia entre Puerto y Socket

| Concepto | Descripción | Ejemplo |
|-----------|--------------|----------|
| **Puerto** | Identifica un servicio dentro de un dispositivo. | Puerto 80 → HTTP |
| **Socket** | Representa una conexión entre dos puntos: IP + Puerto + Protocolo. | (TCP, 192.168.1.5:80) |

> 🎯 El **puerto** es la puerta de entrada; el **socket** es la conversación que ocurre a través de esa puerta.

---

## 💬 Analogía cotidiana

Imaginá una **oficina de atención al público**:

- El **puerto** es la ventanilla o mostrador.  
- El **socket** es el diálogo entre el cliente y el empleado.  
- El **protocolo** son las reglas de atención (cómo saludar, en qué orden hablar, etc.).

Sin sockets, el cliente no podría establecer una conversación estructurada, y los datos viajarían sin control ni contexto.

---

## 🧩 Ejemplo conceptual de comunicación

```
Cliente                        Servidor
192.168.0.10:51000 ────────►   192.168.0.1:80
  │                                 │
  │────────────── Mensaje ─────────▶│
  │◄────────────── Respuesta ───────│
```

El cliente abre un socket temporal (51000) para conectarse al servidor web (puerto 80).  
Ambos intercambian datos mientras la sesión esté activa, y luego cierran la conexión limpiamente.

---

## 📚 Conclusión

Los **sockets** son el punto de unión entre la teoría de redes y la programación práctica.  
Permiten que las aplicaciones se comuniquen entre sí, intercambien información, y creen sistemas distribuidos que operan en tiempo real.

> 💡 Comprender los sockets es entender el corazón de Internet: cada página web, correo electrónico, videollamada o mensaje de chat viaja gracias a ellos.

En la siguiente sección, aprenderemos a **crear y usar sockets en Python**, implementando nuestros propios clientes y servidores con ejemplos reales.
