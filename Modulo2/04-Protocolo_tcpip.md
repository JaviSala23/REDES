# 📘 Apunte de Clase: Protocolo TCP/IP

## 1. ¿Qué es TCP/IP?
**TCP/IP** (Transmission Control Protocol / Internet Protocol) es el **conjunto de protocolos** que permite que los dispositivos en redes (como Internet o redes locales) se comuniquen entre sí.  

- Fue desarrollado en los años 70 en el proyecto ARPANET (precursor de Internet).  
- No es un único protocolo, sino una **familia de protocolos** que trabajan en conjunto.  

👉 **En otras palabras:** TCP/IP es el idioma común que hablan las computadoras y dispositivos conectados a una red.

---

## 2. ¿Qué es una dirección IP?
Una **IP (Internet Protocol Address)** es como el **DNI o dirección postal** de un dispositivo en la red.  

- Sirve para **identificar y ubicar** a cada equipo en una red.  
- Tipos:
  - **IPv4:** 32 bits → formato `192.168.0.1` (≈ 4.3 mil millones de direcciones).  
  - **IPv6:** 128 bits → formato `2001:0db8:85a3::8a2e:0370:7334` (cantidad casi infinita).  

🔹 **Ejemplo:**  
Si querés enviar una carta, necesitás la dirección de la casa. En Internet, la IP cumple ese rol.

---

## 3. Capas del Modelo TCP/IP
El modelo TCP/IP tiene **4 capas** (similar al modelo OSI de 7 capas):

1. **Capa de Aplicación:**  
   Protocolos: HTTP, FTP, SMTP, DNS.  
2. **Capa de Transporte:**  
   Protocolos: **TCP** y **UDP**.  
3. **Capa de Internet:**  
   Protocolo principal: **IP**.  
4. **Capa de Acceso a Red (o Enlace):**  
   Protocolos: Ethernet, Wi-Fi, etc.  

---

## 4. Protocolo TCP (Transmission Control Protocol)
- Garantiza que los datos lleguen **completos y en orden**.  
- Características:
  - Confiable (usa acuses de recibo).  
  - Orientado a conexión.  
  - Divide la información en **segmentos**.  
  - Reensambla los datos en el destino.  
- Usos: Web, correo, transferencia de archivos.  

🔹 **Ejemplo cotidiano:** TCP es como hablar por teléfono: esperás respuesta antes de seguir.

---

## 5. Protocolo UDP (User Datagram Protocol)
- Envía datos **rápidamente**, sin confirmar si llegaron.  
- Características:
  - No confiable.  
  - No orientado a conexión.  
  - Más rápido que TCP.  
- Usos: Streaming, juegos online, VoIP, DNS.  

🔹 **Ejemplo cotidiano:** UDP es como gritar algo en la calle: el mensaje sale rápido, pero no sabés si lo escucharon.

---

## 6. Diferencias entre TCP y UDP

| Característica     | TCP 📞 (Fiable)         | UDP 📢 (Rápido)       |
|---------------------|-------------------------|-----------------------|
| Conexión           | Orientado a conexión    | No orientado a conexión |
| Fiabilidad         | Alta (confirma entrega) | Baja (sin confirmación) |
| Velocidad          | Más lento               | Más rápido             |
| Uso típico         | Web, correo, archivos   | Streaming, juegos, VoIP |

---

## 7. Otros Protocolos Importantes en TCP/IP
- **HTTP/HTTPS:** Navegación web.  
- **FTP:** Transferencia de archivos.  
- **SMTP/IMAP/POP3:** Correo electrónico.  
- **DNS:** Traducción de nombres de dominio en IPs.  
- **DHCP:** Asigna IP automáticamente.  
- **ICMP:** Diagnóstico (ejemplo: `ping`).  

---

## 8. Funcionamiento básico del envío de datos en TCP/IP
1. El usuario escribe `www.google.com`.  
2. **DNS** traduce el nombre a IP.  
3. **TCP** segmenta la información.  
4. **IP** envía los paquetes.  
5. El dispositivo destino recibe y responde.  

👉 Todo en **milisegundos**.

---

## 9. Ejemplo práctico para el alumno
Abrir consola (CMD/PowerShell) y probar:  

- `ping google.com` → Test de conectividad.  
- `tracert google.com` → Ruta de los paquetes.  
- `netstat -n` → Conexiones activas TCP/UDP.  

---

## 10. Conclusión
- TCP/IP es el **lenguaje universal de Internet**.  
- La dirección IP identifica dispositivos.  
- TCP y UDP son los dos métodos principales de transporte.  
- Es la base de **redes, Internet y ciberseguridad**.  

---

## 📌 Ejercicios para alumnos
1. Explicar con sus palabras la diferencia entre TCP y UDP.  
2. Buscar su propia dirección IP pública (ejemplo: https://whatismyipaddress.com).  
3. Hacer un `ping` a un sitio web y explicar qué significa el resultado.  

