---
theme: theme.json
author: Joaquín Velazquez
paging: Página %d / %d
---
# Trabajo Práctico Teórico

```text


██████╗ ███████╗ ██████╗    ██╗  ██╗██████╗  ██████╗ ██████╗ 
██╔══██╗██╔════╝██╔════╝    ██║  ██║╚════██╗██╔═████╗╚════██╗
██████╔╝█████╗  ██║         ███████║ █████╔╝██║██╔██║ █████╔╝
██╔══██╗██╔══╝  ██║         ╚════██║ ╚═══██╗████╔╝██║ ╚═══██╗
██║  ██║██║     ╚██████╗         ██║██████╔╝╚██████╔╝██████╔╝
╚═╝  ╚═╝╚═╝      ╚═════╝         ╚═╝╚═════╝  ╚═════╝ ╚═════╝ 
                                                             

┏━╸┏┓╻┏━╸┏━┓┏━┓┏━┓╻ ╻╻  ┏━┓╺┳╸╻┏┓╻┏━╸
┣╸ ┃┗┫┃  ┣━┫┣━┛┗━┓┃ ┃┃  ┣━┫ ┃ ┃┃┗┫┃╺┓
┗━╸╹ ╹┗━╸╹ ╹╹  ┗━┛┗━┛┗━╸╹ ╹ ╹ ╹╹ ╹┗━┛
┏━┓┏━╸┏━╸╻ ╻┏━┓╻╺┳╸╻ ╻
┗━┓┣╸ ┃  ┃ ┃┣┳┛┃ ┃ ┗┳┛
┗━┛┗━╸┗━╸┗━┛╹┗╸╹ ╹  ╹ 
┏━┓┏━┓╻ ╻╻  ┏━┓┏━┓╺┳┓
┣━┛┣━┫┗┳┛┃  ┃ ┃┣━┫ ┃┃
╹  ╹ ╹ ╹ ┗━╸┗━┛╹ ╹╺┻┛



```

**Materia:** Redes

**Carrera:** Ingeniería en Informática - FIUBA

**Cuatrimestre:** 2C2024

---

# Índice

- 🔌 IPSec
- 🔐 Servicios
- 🚚 Modos
- 🤝 Security Associations
- 🤖 Algoritmos
- 📦 Formato del paquete
- 🖊️ Ventajas y desventajas
- 🔧 Casos de uso comunes
- 🔄 Alternativas

---

# 🔌 IPSec

Conjunto de protocolos de seguridad para la capa de red.

Usa UDP como protocolo de transporte, y normalmente usa el puerto `500`.

## Protocolos

 - Authentication Header (AH)

 - Encapsulating Security Payload (ESP)

 - Security Associations (SA)

---

# 🔐 Servicios que ofrece 

 * `Confidencialidad` - encripta el *payload* para que la información del paquete no pueda ser leida por otro individuo.
 * `Autenticación de origen` - asegura que el paquete proviene del origen indicado.
 * `Integridad` - asegura que el contenido del paquete no va a ser modificado.
 * `Anti-replay` - evita que el reenvío de paquetes interceptados por un agente externo afecte el flujo de datos.
 * `Confidencialidad de flujo de datos (TCP)` - disimula (de manera limitada) la cantidad de paquetes enviados de origen a destino.

---

# 🚏 Modos

## 🚚 Modo Transporte
```text
┌Paquete─ESP─────────────────────────────┐
│         ┌Paquete─ESP──────────────────┐│
│         │        ┌Datos──────┐        ││
│ IP      │ ESP    │           │ ESP    ││
│ Header  │ Header │           │ Trailer││
│         │        │           │        ││
│         │        └───────────┘        ││
│         └─────────────────────────────┘│
└────────────────────────────────────────┘
```

## 🚆 Modo Túnel
```text
┌Datagrama─IP─Nuevo────────────────────────────────┐
│        ┌Paquete─ESP─────────────────────────────┐│
│        │         ┌Datagrama─IP─Original┐        ││
│        │         │        ┌Datos──────┐│        ││
│ IP     │ ESP     │ IP     │           ││ESP     ││
│ Header │ Header  │ Header │           ││Trailer ││
│        │         │        │           ││        ││
│        │         │        └───────────┘│        ││
│        │         └─────────────────────┘        ││
│        └────────────────────────────────────────┘│
└──────────────────────────────────────────────────┘
```

---

# 🤝 Security Associations

Protocolo para establecer los parámetros de seguridad entre dos hosts.


---

# 🤖 Algoritmos

---

# 📦 Formato del paquete

Dependiendo de los servicios y algoritmos que se utilice, ESP puede agregar
hasta 60 bytes de headers y trailers, sin contar el header estándar de IP.

```text
     0                   1                   2                   3
     0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |               Security Parameters Index (SPI)                 |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                      Sequence Number                          |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+---
   |                    IV (optional)                              | ^ p
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+ | a
   |                    Rest of Payload Data  (variable)           | | y
   ~                                                               ~ | l
   |                                                               | | o
   +               +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+ | a
   |               |         TFC Padding * (optional, variable)    | v d
   +-+-+-+-+-+-+-+-+         +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+---
   |                         |        Padding (0-255 bytes)        |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+     +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                               |  Pad Length   | Next Header   |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |         Integrity Check Value-ICV   (variable)                |
   ~                                                               ~
   |                                                               |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```


- Una vez definidos los algoritmos y servicios, la estructura del paquete se
  mantiene fija.

---

## Definiciones

##### Security Parameters Index
Número que permite que el receiver identifique a que SA pertenece el paquete.
Lo define el receiver y arranca desde 256 (0-255 están reservados).

##### Número de secuencia
Es parte del servicio Anti Replay y actúa como el número de secuencia en 
conexiones TCP. Siempre empieza desde 0.

Puede extenderse a 64 bits, con los 32 bits superiores siendo implícitos (tanto
el receiver como el sender mantienen la cuenta).

##### Initialization Vector (IV)
Campo para los algoritmos de encriptación que necesiten información adicional.
Se considera parte del payload.

##### Padding
Se utiliza en casos en donde los datos del payload no terminan en un número de
bytes divisible por 4 (necesario para la estructura del paquete ESP). Pero
además, algunos algoritmos de encriptado pueden necesitar que la información
sea de un múltiplo específico. El padding puede ser de 0 a 255 bytes, y su
contenido es una secuencia del estilo 1,2,3, .... El tamaño del padding
aplicado se puede ver en el siguiente campo de 1 byte, llamado Pad Length.

##### Next Header
El código del siguiente protocolo presente en el payload. Es similar al campo
presente en el header IP. El código 59 se utiliza para indicar que no hay
siguiente protocolo. Este tipo de paquete se denomina dummy.

Los paquetes dummy se pueden utilizar para enmascarar la ausencia de tráfico.
Para esto, se puede modelar el tráfico para que siga una distribución
específica, agregando dummies en momentos en donde no hay información que
envíar.

##### TFC Padding
Padding adicional para cumplir con los requisitos del servicio de Traffic Flow
Confidentiality (TFC). Este padding, a diferencia del anteriormente mencionado,
no tiene límite, y permite esconder el verdadero tamaño del paquete. Para
agregarlo, es necesario que dentro del payload se especifique el verdadero
tamaño del datagrama.

##### Integrity Check Value (ICV)
Es parte del servicio de integridad. Tiene tamaño variado y su valor depende
del algoritmo de integridad (o de modo combinado) utilizado.

---

# 👎 Ventajas y desventajas

---

# 🔄 Alternativas

 - Authentication Header (AH)
 - TLS
