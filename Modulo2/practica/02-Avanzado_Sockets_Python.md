# 🚀 Sockets Avanzados en Python (MD): Multicliente, Threading, Selectors y TLS

> Este apunte asume que ya viste el **tutorial práctico básico** y querés subir de nivel: múltiples clientes, protocolos simples, escalabilidad con `selectors`, cierre ordenado y un toque de **TLS** para cifrar conexiones.

---

## 🧭 Mapa del contenido

1. [Patrones avanzados y buenas prácticas](#patrones-avanzados-y-buenas-prácticas)
2. [Servidor **multicliente** con `threading`](#servidor-multicliente-con-threading)
3. [Cliente de chat con lectura concurrente](#cliente-de-chat-con-lectura-concurrente)
4. [**Protocolos de mensajes**: delimitadores y longitud-prefijo](#protocolos-de-mensajes-delimitadores-y-longitud-prefijo)
5. [Servidor **escalable** con `selectors` (I/O no bloqueante)](#servidor-escalable-con-selectors-io-no-bloqueante)
6. [Cierre limpio, señales y timeouts](#cierre-limpio-señales-y-timeouts)
7. [Optimización y opciones de socket (SO_REUSEADDR, TCP_NODELAY, Keep-Alive)](#optimización-y-opciones-de-socket-so_reuseaddr-tcp_nodelay-keep-alive)
8. [Capa segura: **TLS** con `ssl`](#capa-segura-tls-con-ssl)
9. [Desafíos y ejercicios](#desafíos-y-ejercicios)

---

## Patrones avanzados y buenas prácticas

- **Evitar bloqueos**: usá hilos o I/O no bloqueante si esperás múltiples clientes.
- **Definí un protocolo**: newline (`\n`) o longitud-prefijo (4 bytes) para separar mensajes.
- **Manejo de errores**: capturá `ConnectionResetError`, `BrokenPipeError`, `TimeoutError`.
- **Cierre ordenado**: primero `shutdown(socket.SHUT_WR)`, luego `close()`.
- **Logs y depuración**: incluí prints/logs con cliente/puerto para trazar eventos.
- **Seguridad**: validá inputs; si exponés a Internet, considerá **TLS** y autenticación.

---

## Servidor multicliente con `threading`

### 🧩 Características
- Acepta múltiples clientes simultáneamente.
- **Broadcast** a todos los conectados.
- Comandos `/name` y `/quit`.
- Lock para acceso seguro a la lista de clientes.
- Protocolo **línea por línea** (`\n` terminado, UTF-8).

### `chat_server_threaded.py`

```python
import socket
import threading
import time

HOST = "0.0.0.0"   # Escucha en todas las interfaces
PORT = 9001

clients = {}           # sock -> {"addr": (ip,port), "name": str}
clients_lock = threading.Lock()

def broadcast(msg: str, origin=None):
    data = (msg + "\n").encode("utf-8")
    with clients_lock:
        dead = []
        for sock, meta in clients.items():
            if sock is origin:
                continue
            try:
                sock.sendall(data)
            except Exception as e:
                print(f"[WARN] No se pudo enviar a {meta['addr']}: {e}")
                dead.append(sock)
        for d in dead:
            remove_client(d)

def remove_client(sock):
    with clients_lock:
        meta = clients.pop(sock, None)
    if meta:
        try:
            sock.shutdown(socket.SHUT_RDWR)
        except Exception:
            pass
        try:
            sock.close()
        except Exception:
            pass
        print(f"[INFO] Cliente desconectado: {meta['name']} {meta['addr']}")
        broadcast(f"🟡 {meta['name']} salió del chat.")

def handle_client(sock: socket.socket, addr):
    sock.settimeout(120)  # Inactividad máxima
    name = f"{addr[0]}:{addr[1]}"
    with clients_lock:
        clients[sock] = {"addr": addr, "name": name}
    sock.sendall(f"Bienvenido! Tu nombre inicial es {name}. Cambialo con /name <nuevo>\n".encode("utf-8"))
    broadcast(f"🟢 {name} se unió al chat.", origin=None)
    print(f"[INFO] Nuevo cliente: {name}")

    buffer = b""
    try:
        while True:
            chunk = sock.recv(4096)
            if not chunk:
                break
            buffer += chunk
            while b"\n" in buffer:
                line, buffer = buffer.split(b"\n", 1)
                text = line.decode("utf-8", errors="replace").strip()
                if not text:
                    continue
                if text.lower() == "/quit":
                    sock.sendall(b"Adios!\n")
                    raise SystemExit
                if text.startswith("/name "):
                    new_name = text[6:].strip() or name
                    with clients_lock:
                        old = clients[sock]["name"]
                        clients[sock]["name"] = new_name
                    broadcast(f"✏️ {old} ahora es {new_name}")
                    continue
                # Mensaje normal
                with clients_lock:
                    sender = clients[sock]["name"]
                broadcast(f"{sender}: {text}", origin=sock)
    except (ConnectionResetError, TimeoutError):
        pass
    except SystemExit:
        pass
    except Exception as e:
        print(f"[ERROR] Cliente {addr}: {e}")
    finally:
        remove_client(sock)

def main():
    server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    server.bind((HOST, PORT))
    server.listen(100)
    print(f"[READY] Chat server en {HOST}:{PORT} (CTRL+C para salir)")

    try:
        while True:
            sock, addr = server.accept()
            t = threading.Thread(target=handle_client, args=(sock, addr), daemon=True)
            t.start()
    except KeyboardInterrupt:
        print("\n[SIGINT] Cerrando servidor...")
    finally:
        with clients_lock:
            for s in list(clients.keys()):
                remove_client(s)
        server.close()

if __name__ == "__main__":
    main()
```

---

## Cliente de chat con lectura concurrente

- Un hilo lee del servidor y muestra mensajes.
- El hilo principal toma input del usuario y envía.
- Comando `/quit` para salir.

### `chat_client.py`
```python
import socket
import threading
import sys

HOST = "127.0.0.1"
PORT = 9001

def reader(sock: socket.socket):
    try:
        while True:
            data = sock.recv(4096)
            if not data:
                print("[INFO] Conexión cerrada por el servidor.")
                break
            print(data.decode("utf-8"), end="")
    except Exception as e:
        print(f"[ERROR] Reader: {e}")
    finally:
        try:
            sock.close()
        except Exception:
            pass

def main():
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.connect((HOST, PORT))
    t = threading.Thread(target=reader, args=(sock,), daemon=True)
    t.start()
    try:
        for line in sys.stdin:
            msg = line.rstrip("\n")
            sock.sendall((msg + "\n").encode("utf-8"))
            if msg.strip().lower() == "/quit":
                break
    except KeyboardInterrupt:
        pass
    finally:
        try:
            sock.shutdown(socket.SHUT_WR)
        except Exception:
            pass
        sock.close()

if __name__ == "__main__":
    main()
```

> 💡 Probalo: ejecutá primero `chat_server_threaded.py`, luego abrí **varias** terminales y corré `chat_client.py` en cada una.

---

## Protocolos de mensajes: delimitadores y longitud-prefijo

Cuando enviás bytes por un socket **no hay “límites de mensaje”**; llega un **stream**. Tenés que **definir tu propio protocolo**:

### 1) **Delimitador** (p. ej., `\n`)
- Simple y apto para texto.
- Leés hasta encontrar `\n`.

### 2) **Longitud-prefijo** (4 bytes big-endian)
- Enviás primero `len(payload)` en 4 bytes y luego el mensaje.
- Útil para binario o mensajes largos.

#### Ejemplo de empaquetado/desempaquetado

```python
import struct

def pack_message(payload: bytes) -> bytes:
    return struct.pack("!I", len(payload)) + payload  # !I = uint32 big-endian

def recv_all(sock, n):
    data = b""
    while len(data) < n:
        chunk = sock.recv(n - len(data))
        if not chunk:
            raise ConnectionError("Socket cerrado durante recv_all")
        data += chunk
    return data

def recv_message(sock) -> bytes:
    raw_len = recv_all(sock, 4)
    (length,) = struct.unpack("!I", raw_len)
    return recv_all(sock, length)
```

---

## Servidor escalable con `selectors` (I/O no bloqueante)

Cuando el número de clientes crece, **thread-per-client** puede escalar mal. Alternativa: **I/O multiplexada** con `selectors`.

### `echo_selectors.py` (multi-cliente, no bloqueante)

```python
import selectors
import socket

HOST, PORT = "0.0.0.0", 9002
sel = selectors.DefaultSelector()

def accept(sock):
    conn, addr = sock.accept()
    conn.setblocking(False)
    print(f"[ACCEPT] {addr}")
    sel.register(conn, selectors.EVENT_READ, read)

def read(conn):
    try:
        data = conn.recv(4096)
    except ConnectionResetError:
        data = b""
    if data:
        # Eco
        conn.sendall(b"Echo: " + data)
    else:
        print("[CLOSE]", conn.getpeername())
        sel.unregister(conn)
        conn.close()

def main():
    server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    server.bind((HOST, PORT))
    server.listen(100)
    server.setblocking(False)
    sel.register(server, selectors.EVENT_READ, accept)
    print(f"[READY] echo_selectors en {HOST}:{PORT}")
    try:
        while True:
            events = sel.select(timeout=1)  # 1s para poder reaccionar a señales
            for key, mask in events:
                callback = key.data
                callback(key.fileobj)
    except KeyboardInterrupt:
        print("\n[SIGINT] Saliendo...")
    finally:
        sel.close()
        server.close()

if __name__ == "__main__":
    main()
```

> 🔬 `selectors` evita crear un hilo por cliente y permite manejar miles de conexiones con un solo loop.

---

## Cierre limpio, señales y timeouts

- **Timeouts**: `sock.settimeout(segundos)` para evitar bloqueos indefinidos.
- **Señales (Ctrl+C)**: rodeá el loop principal con `try/except KeyboardInterrupt`.
- **Cierre ordenado**:
  1. `sock.shutdown(socket.SHUT_WR)` para avisar que no enviarás más.
  2. Leé lo pendiente (si aplica).
  3. `sock.close()`.

---

## Optimización y opciones de socket (SO_REUSEADDR, TCP_NODELAY, Keep-Alive)

```python
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Reusar puerto rápidamente tras reiniciar
sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

# Deshabilitar Nagle (latencia menor para mensajes pequeños)
sock.setsockopt(socket.IPPROTO_TCP, socket.TCP_NODELAY, 1)

# Keep-Alive (detecta caídas de conexión inactivas a nivel TCP)
sock.setsockopt(socket.SOL_SOCKET, socket.SO_KEEPALIVE, 1)
```

> ⚠️ Los parámetros de Keep-Alive (intervalos) son del sistema operativo y pueden ajustarse a nivel de OS.

---

## Capa segura: TLS con `ssl`

Para cifrar el tráfico (y autenticación del servidor), podés envolver un socket con **TLS**.

### Servidor TLS (esqueleto)

```python
import socket, ssl

HOST, PORT = "0.0.0.0", 9443

context = ssl.SSLContext(ssl.PROTOCOL_TLS_SERVER)
context.load_cert_chain(certfile="server.crt", keyfile="server.key")

bindsock = socket.socket()
bindsock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
bindsock.bind((HOST, PORT))
bindsock.listen(5)
print(f"[READY] TLS server en {HOST}:{PORT}")

while True:
    newsock, addr = bindsock.accept()
    try:
        ssock = context.wrap_socket(newsock, server_side=True)
        data = ssock.recv(1024)
        ssock.sendall(b"Hola con TLS\n")
        ssock.close()
    except ssl.SSLError as e:
        print("[SSL ERROR]", e)
```

### Cliente TLS (esqueleto)

```python
import socket, ssl

HOST, PORT = "127.0.0.1", 9443

context = ssl.create_default_context()
# Para pruebas con cert auto-firmado (no recomendado en producción):
context.check_hostname = False
context.verify_mode = ssl.CERT_NONE

with socket.create_connection((HOST, PORT)) as sock:
    with context.wrap_socket(sock, server_hostname=HOST) as ssock:
        ssock.sendall(b"Ping\n")
        print(ssock.recv(1024).decode())
```

> 🔐 En producción, **no desactives** la verificación; usá certificados válidos y `verify_mode=CERT_REQUIRED` con `cafile`/`capath`.

---

## Desafíos y ejercicios

1. **Chat escalable con `selectors`**: reimplementá el chat multicliente usando `selectors` (broadcast sin hilos).
2. **Mensajería robusta**: migrá el chat a **longitud-prefijo** para permitir binario y mensajes grandes.
3. **Historial y comandos**: agregá `/who`, `/whisper <user> <msg>`, `/rooms` y salas.
4. **TLS opcional**: permití `--tls` al ejecutar el servidor y cliente.
5. **Reconexión automática**: el cliente intenta reconectar con backoff exponencial.
6. **Observabilidad**: agregá logging con `logging` y métricas simples (clientes activos, mensajes por minuto).
7. **IPv6**: hacé que el servidor acepte IPv4/IPv6 (`AF_INET6`) y probalo.

---

## Apéndice: IPv6 rápido

Para escuchar con IPv6:
```python
srv6 = socket.socket(socket.AF_INET6, socket.SOCK_STREAM)
srv6.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
srv6.bind(("::", 9003, 0, 0))  # (addr, port, flowinfo, scopeid)
srv6.listen(100)
```

---

### ✅ Conclusión

- Aprendiste a construir **chat multicliente** con `threading`, y una versión **escalable** con `selectors`.  
- Viste **protocolos de framing**, **cierre ordenado**, **opciones de socket** y **TLS**.  
- Con estas bases, podés implementar desde **APIs personalizadas** hasta **servicios de tiempo real** con seguridad.

> “Primero que funcione, luego que escale, y después que sea seguro.” – Orden recomendado para tus iteraciones 🚧🔧🔐
