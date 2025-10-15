# 🔢 Puertos y Direcciones en Redes

## 📍 Introducción

En las redes de computadoras, **toda comunicación necesita dos identificadores fundamentales** para que los datos lleguen correctamente a su destino:

1. **La dirección IP**, que indica **qué dispositivo** en la red es el destinatario.
2. **El número de puerto**, que indica **qué aplicación o servicio** dentro de ese dispositivo debe recibir los datos.

Podemos pensar en esto como enviar una carta:  
- La **dirección IP** sería la dirección del edificio.  
- El **puerto** sería el número del departamento o la oficina dentro del edificio.  

Sin ambos datos, el mensaje se pierde o no llega a donde corresponde.

---

## 🌐 Direcciones IP

Una **dirección IP (Internet Protocol)** es un número que identifica de forma única a cada dispositivo conectado a una red.  
El protocolo IP forma parte del modelo **TCP/IP**, en la capa de red, y es el encargado de **enrutar** los paquetes de datos desde el origen hasta el destino.

### Tipos de direcciones IP

| Versión | Tamaño | Ejemplo | Comentario |
|----------|--------|----------|-------------|
| **IPv4** | 32 bits | `192.168.1.10` | Es la versión más usada actualmente. Representa hasta 4.294.967.296 direcciones posibles. |
| **IPv6** | 128 bits | `2001:0db8:85a3::8a2e:0370:7334` | Creada para reemplazar a IPv4 y permitir una cantidad casi ilimitada de direcciones. |

Cada dispositivo puede tener una o más IPs, y se diferencian en dos grandes categorías:

- **IP pública:** Identifica un dispositivo de forma única en Internet. Es asignada por el proveedor de servicios (ISP).
- **IP privada:** Se usa dentro de redes locales (LAN) y no es visible desde Internet.

> 💡 Ejemplo:  
> Tu computadora puede tener la IP privada `192.168.0.5`, pero al acceder a Internet lo hace a través del router, que tiene una IP pública, como `181.28.15.102`.  
> Este mecanismo se conoce como **NAT (Network Address Translation)**.

---

## 🚪 ¿Qué es un puerto?

Un **puerto** es un número lógico que permite distinguir múltiples procesos o servicios que se ejecutan simultáneamente en una misma máquina.

El puerto funciona como un **canal de comunicación** dentro del sistema operativo. Cada vez que un programa necesita enviar o recibir información en la red, **solicita un puerto al sistema** para poder operar.

> 📦 En cada paquete de red, además de las direcciones IP, también se incluyen los **puertos de origen y destino**, junto con el **protocolo de transporte** (TCP o UDP).

Ejemplo de una conexión:
```
(IP origen : Puerto origen)  →  (IP destino : Puerto destino)
```
Por ejemplo:
```
192.168.1.10:55000  →  142.250.190.78:443
```
En este caso:
- Tu PC (cliente) utiliza el puerto **55000** de manera temporal.  
- Se conecta al servidor de Google en el puerto **443**, reservado para HTTPS.

---

## ⚙️ Rangos de puertos

Los números de puerto van desde **0 hasta 65535**, y se dividen en tres grandes categorías:

| Rango | Nombre | Uso típico | Ejemplo |
|--------|---------|------------|----------|
| **0 – 1023** | Puertos bien conocidos (*Well-known ports*) | Reservados para servicios estándar definidos por la IANA. | HTTP (80), FTP (21), SSH (22), DNS (53) |
| **1024 – 49151** | Puertos registrados (*Registered ports*) | Asignados a aplicaciones específicas o servicios de terceros. | MySQL (3306), PostgreSQL (5432) |
| **49152 – 65535** | Puertos dinámicos o efímeros (*Ephemeral ports*) | Usados temporalmente por clientes para iniciar conexiones. | Navegadores, apps de chat, juegos en línea. |

> 🧠 Cada vez que abrís una página web, tu navegador elige un puerto efímero (por ejemplo, 52048) para comunicarse con el servidor web en el puerto 80 o 443.

---

## 🧩 Asociación IP + Puerto = Socket

El **socket** es la combinación única entre una **dirección IP** y un **número de puerto**.  
Cada socket representa un punto de conexión en la comunicación.

| Elemento | Descripción |
|-----------|-------------|
| **IP** | Identifica el dispositivo en la red. |
| **Puerto** | Identifica el servicio o aplicación dentro del dispositivo. |
| **Socket** | Representa el par `(IP, Puerto)`, que forma el punto final de la conexión. |

Ejemplo:
```
Cliente: 192.168.0.5:53000  →  Servidor: 142.250.190.78:443
```
Aquí:
- El cliente usa un puerto temporal (53000).  
- El servidor escucha en el puerto fijo (443) correspondiente a HTTPS.

> 📡 El socket permite al sistema operativo enrutar los datos correctamente hacia la aplicación correspondiente.

---

## 🔄 Comunicación en la práctica

### 1. El cliente inicia la conexión
El sistema operativo le asigna un puerto temporal y prepara el socket de salida.

### 2. El servidor escucha
El servidor tiene un socket abierto y “en escucha” (`listen`) en un puerto específico.

### 3. Se establece la conexión
Cuando el cliente se conecta al puerto del servidor, se crea un canal de comunicación bidireccional.

### 4. Intercambio de datos
Los mensajes fluyen en ambos sentidos hasta que uno de los extremos cierra la conexión (`close`).

---

## 🧱 Ejemplo visual del flujo

```
Cliente                        Servidor
192.168.0.5:53000  ─────────►  142.250.190.78:443
          ▲                              │
          │<──────────────Respuesta──────┘
```

El cliente inicia la solicitud, el servidor responde, y ambos intercambian información siguiendo las reglas del protocolo de transporte (TCP o UDP).

---

## 🧠 Analogía cotidiana

Imaginá un **edificio de oficinas**:

- La **dirección IP** es la dirección del edificio (ejemplo: “Av. Internet 192.168”).  
- Cada **puerto** es una oficina distinta dentro del edificio (ejemplo: “Oficina 80: Web”, “Oficina 25: Correo”).  
- Si enviás una carta sin indicar el número de oficina, el mensaje no llega al destinatario correcto.

Del mismo modo, si una aplicación intenta comunicarse con una IP sin especificar el puerto adecuado, el sistema no sabrá a qué servicio destinar la información.

---

## 📚 Conclusión

Los **puertos** y las **direcciones IP** son la base sobre la cual se construye toda comunicación en red.  
Gracias a ellos, los sistemas operativos pueden distinguir y gestionar cientos de conexiones simultáneas sin interferencias.

> 🧩 En resumen:  
> - La **dirección IP** identifica el equipo.  
> - El **puerto** identifica el servicio dentro del equipo.  
> - Juntos forman un **socket**, que será el corazón de la comunicación entre cliente y servidor.

Comprender esta relación es esencial antes de adentrarse en el estudio de los **sockets**, que son las herramientas que nos permiten **crear y programar nuestros propios servicios de red**.
