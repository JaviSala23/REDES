# Capítulo 2: Modelo OSI – Capa Física y Topología Bus



---

## 🎯 Objetivo del capítulo

Estudiar la comunicación en red a través del **Modelo de Referencia OSI**, comprendiendo sus capas, su funcionamiento modular y profundizando en la **Capa 1 - Física** y la **topología Bus**.

---

## 📺 Recursos audiovisuales complementarios

- [Modelo OSI explicado](https://youtu.be/I6qlFo2WyTE)
- [Topología Bus y problemas comunes](https://youtu.be/CnNRdJgeMo8)

---

## 🧩 ¿Qué es el modelo OSI?

El **Modelo OSI (Open Systems Interconnection)** es un modelo de referencia creado por la ISO que divide el proceso de comunicación en redes en **7 capas**, facilitando el diseño, compatibilidad y evolución de redes.

---

## 🧱 Las 7 capas del modelo OSI

| Nº | Capa           | Función principal |
|----|----------------|-------------------|
| 7  | Aplicación     | Interacción con el usuario final |
| 6  | Presentación   | Formato y codificación de datos |
| 5  | Sesión         | Administración de sesiones |
| 4  | Transporte     | Entrega confiable de datos |
| 3  | Red            | Direccionamiento y ruteo |
| 2  | Enlace de Datos| Control de acceso y errores |
| 1  | Física         | Transmisión eléctrica o física |

---

## 🔌 Capa 1 - Física

- Define especificaciones **eléctricas, mecánicas y funcionales**.
- Ej: niveles de tensión, tasa de transferencia, conectores, distancia máxima.
- Utiliza **NICs** (placas de red) para transmitir los datos en forma de señales (eléctricas o luminosas).

---

## 🔄 Comunicación modular

Cada capa se comunica con:
- La **capa superior**
- La **capa inferior**
- Su **equivalente en otro dispositivo**

Esto permite **reemplazar tecnologías** sin reescribir todas las aplicaciones (modelo modular ≠ monolítico).

---

## 🧪 Protocolos de red

Un **protocolo** es un conjunto de reglas para permitir la comunicación.

### Tipos de protocolos según OSI:
- **LAN** (Ethernet, Wi-Fi)
- **WAN** (Frame Relay, ATM)
- **Ruteo** (IP, RIP, OSPF)
- **Red** (HTTP, FTP, SMTP)

---

## 🧵 Medios de transmisión de datos

### 🔹 Medios guiados
- **Cobre (coaxial, par trenzado)**: económicos, fácil instalación.
- **Fibra óptica**: alta velocidad y largo alcance.

### 🔸 Medios no guiados
- **Radiofrecuencia, infrarrojos, microondas**: brindan movilidad, pero menor velocidad y confiabilidad.

---

## 🧷 Cables coaxiales (RG58)

- Núcleo de cobre + aislante + malla metálica + vaina.
- Impedancia: **50 ohms**.
- Conectores: **BNC crimpeables**.
- Segmento máximo: **185 metros**.

> 📏 Permite velocidades de hasta 10 Mbps (10Base2).

---

## ⚠️ Fallos comunes en redes Bus (10Base2)

| Problema                | Efecto                                               |
|-------------------------|------------------------------------------------------|
| Cortocircuito           | Detiene la propagación de datos                     |
| Cable cortado           | Rebote de señales; división del segmento            |
| Falsos contactos        | Ruido eléctrico e interferencias                    |
| Terminadores incorrectos| Rechazo o colisiones de tramas                      |

---

## 🔍 Diagnóstico con Multímetro

Mediciones básicas para cables coaxiales:

1. **Malla** → continuidad ≈ 0Ω
2. **Conductor central** → continuidad ≈ 0Ω
3. **Aislamiento** → resistencia **infinita** (sin contacto entre malla y núcleo)

> ✅ Una red funcional 10Base2 marca **25Ω** en una “T” (por dos terminadores de 50Ω en paralelo).

---

## 🧮 Conceptos de electricidad aplicada

- **Conductores**: permiten el paso de corriente (cobre, aluminio)
- **Aislantes**: impiden el paso (plástico, goma)
- **Resistencia eléctrica (R)**: oposición al paso de la corriente (se mide en **Ohmios - Ω**)
- **Ley de resistencias**:
  - Serie: `Rt = R1 + R2 + ...`
  - Paralelo: `1/Rt = 1/R1 + 1/R2 + ...`

---

## 🧰 Multímetro (Tester)

- Mide **resistencia**, **tensión**, **continuidad**, etc.
- El símbolo `1` en pantalla representa **infinito** (sin continuidad).
- En modo continuidad: **zumbador = conexión**.

---

## ⚙️ Paso a paso: detección de fallos

1. Medir en el centro del segmento.
2. Dividir en mitades sucesivas (método binario).
3. Confirmar con medición:  
   - **25Ω = OK**
   - **0Ω = Cortocircuito**
   - **50Ω = Faltante de un terminador**
   - **∞ = Cable cortado o sin terminadores**

---

## ❓ Cuestionario de repaso

1. ¿Cuál es el objetivo del Modelo OSI?
2. ¿Cuál es la funcionalidad de la Capa Física? ¿Qué componentes de red incluiría en ella?
3. ¿Cómo definiría a la expresión “Segmento Troncal”?
4. ¿Por qué puedo determinar el correcto funcionamiento de un segmento troncal 10Base2 si un multímetro mide 25Ω desde una “T”?
5. ¿Es lo mismo medir resistencia que medir continuidad? ¿Por qué?

---


