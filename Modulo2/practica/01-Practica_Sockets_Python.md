# 🧠 Tutorial Práctico: Programación con Sockets en Python

## 📘 Introducción

La librería `socket` de Python permite crear aplicaciones que se comunican entre sí a través de la red.  
Con ella podemos construir **clientes y servidores** que envían y reciben datos, simulando el funcionamiento de servicios reales como un chat, un servidor web o una API.

Python incluye `socket` en su biblioteca estándar, por lo que **no es necesario instalar nada adicional**.

```bash
# No requiere instalación
import socket
```

---

## 🧩 Conceptos básicos

Recordemos:

- Un **socket** conecta dos programas (cliente y servidor) a través de una **dirección IP** y un **puerto**.
- El **servidor** espera conexiones entrantes.
- El **cliente** inicia la comunicación.

El flujo básico de uso es el siguiente:

### En el Servidor:
1. Crear un socket (`socket()`).
2. Asociarlo a una IP y puerto (`bind()`).
3. Esperar conexiones (`listen()`).
4. Aceptar una conexión (`accept()`).
5. Enviar o recibir datos (`send()` / `recv()`).
6. Cerrar la conexión (`close()`).

### En el Cliente:
1. Crear un socket (`socket()`).
2. Conectarse al servidor (`connect()`).
3. Enviar o recibir datos (`send()` / `recv()`).
4. Cerrar la conexión (`close()`).

---

## 🧱 Ejemplo 1: Servidor TCP básico

Este servidor acepta una conexión y responde con un mensaje de bienvenida.

```python
import socket

# 1. Crear el socket TCP
servidor = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# 2. Asociarlo a una IP y puerto
servidor.bind(("localhost", 5000))

# 3. Escuchar conexiones entrantes
servidor.listen(1)
print("Servidor en espera de conexiones...")

# 4. Aceptar la conexión
conexion, direccion = servidor.accept()
print(f"Conectado con {direccion}")

# 5. Enviar un mensaje
mensaje = "Hola, cliente. Bienvenido al servidor Python!"
conexion.send(mensaje.encode('utf-8'))

# 6. Cerrar la conexión
conexion.close()
servidor.close()
```

### 🧠 Explicación
- `socket.AF_INET`: indica que se usará IPv4.  
- `socket.SOCK_STREAM`: define el uso de TCP (flujo de bytes confiable).  
- `bind(("localhost", 5000))`: vincula el servidor a la IP local y al puerto 5000.  
- `listen(1)`: permite que el servidor espere hasta una conexión.  
- `accept()`: bloquea el programa hasta que un cliente se conecte.  

---

## 💻 Ejemplo 2: Cliente TCP

Este cliente se conecta al servidor y recibe el mensaje.

```python
import socket

# 1. Crear el socket
cliente = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# 2. Conectarse al servidor
cliente.connect(("localhost", 5000))

# 3. Recibir el mensaje
mensaje = cliente.recv(1024).decode('utf-8')
print(f"Mensaje recibido: {mensaje}")

# 4. Cerrar la conexión
cliente.close()
```

### 📡 Ejecución

1. Ejecutá primero el **servidor** (esperará conexiones).
2. Luego ejecutá el **cliente**.
3. Verás en la terminal del servidor:  
   ```
   Servidor en espera de conexiones...
   Conectado con ('127.0.0.1', 12345)
   ```
4. Y en el cliente:  
   ```
   Mensaje recibido: Hola, cliente. Bienvenido al servidor Python!
   ```

---

## 🔁 Ejemplo 3: Intercambio bidireccional (eco)

En este ejemplo, el servidor repetirá cualquier mensaje que reciba del cliente, hasta que este escriba `salir`.

### 🖥️ Servidor (eco_server.py)

```python
import socket

servidor = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
servidor.bind(("localhost", 6000))
servidor.listen(1)
print("Servidor de eco esperando conexión...")

conexion, direccion = servidor.accept()
print(f"Conectado con {direccion}")

while True:
    datos = conexion.recv(1024).decode('utf-8')
    if not datos or datos.lower() == "salir":
        print("Cerrando conexión...")
        break
    print(f"Cliente: {datos}")
    conexion.send(f"Eco: {datos}".encode('utf-8'))

conexion.close()
servidor.close()
```

### 💬 Cliente (eco_client.py)

```python
import socket

cliente = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
cliente.connect(("localhost", 6000))

while True:
    mensaje = input("Tú: ")
    cliente.send(mensaje.encode('utf-8'))
    if mensaje.lower() == "salir":
        break
    respuesta = cliente.recv(1024).decode('utf-8')
    print(respuesta)

cliente.close()
```

### 🧩 Resultado
```
Servidor de eco esperando conexión...
Conectado con ('127.0.0.1', 52344)
Cliente: Hola servidor
Cliente: ¿Cómo estás?
Cliente: salir
```

El cliente verá:
```
Tú: Hola servidor
Eco: Hola servidor
Tú: ¿Cómo estás?
Eco: ¿Cómo estás?
Tú: salir
```

---

## 🚀 Ejemplo 4: Servidor UDP

UDP no requiere establecer una conexión. Es más rápido, pero no garantiza la entrega.

### Servidor

```python
import socket

servidor = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
servidor.bind(("localhost", 7000))
print("Servidor UDP listo...")

while True:
    datos, direccion = servidor.recvfrom(1024)
    print(f"Mensaje de {direccion}: {datos.decode()}")
    if datos.decode().lower() == "salir":
        break
    servidor.sendto(f"Eco UDP: {datos.decode()}".encode(), direccion)

servidor.close()
```

### Cliente

```python
import socket

cliente = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

while True:
    mensaje = input("Tú: ")
    cliente.sendto(mensaje.encode(), ("localhost", 7000))
    if mensaje.lower() == "salir":
        break
    datos, _ = cliente.recvfrom(1024)
    print(datos.decode())

cliente.close()
```

> 🧠 Diferencia clave: UDP usa `sendto()` y `recvfrom()` en lugar de `send()` y `recv()`, ya que no establece una conexión persistente.

---

## ⚡ Ejercicio propuesto

🔹 **Desafío 1:** Modificá el servidor TCP para aceptar múltiples clientes usando *threads*.  
🔹 **Desafío 2:** Implementá un pequeño chat grupal entre varios clientes conectados.  
🔹 **Desafío 3:** Añadí manejo de errores (por ejemplo, capturar `ConnectionResetError`).

> 💡 Consejo: probá ejecutar el servidor en otra computadora de tu red local.  
> Solo tenés que reemplazar `"localhost"` por la IP real del servidor, como `"192.168.1.10"`.

---

## 📚 Conclusión

En esta práctica aprendiste a:

✅ Crear sockets en Python con `socket.socket()`  
✅ Programar clientes y servidores TCP y UDP  
✅ Enviar y recibir datos entre aplicaciones  
✅ Comprender el ciclo básico de comunicación en red

> Los sockets son la base de todo: navegadores, videojuegos, APIs, chats y servidores web los usan constantemente.  
> Dominar esta herramienta te permitirá **entender Internet desde adentro**.
