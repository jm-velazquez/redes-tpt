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

**Link al RFC:** https://datatracker.ietf.org/doc/html/rfc4303

**Link a TP:** https://github.com/jm-velazquez/redes-tpt

---

# Servicios que ofrece 

* `Confidencialidad`
* `Autenticación de origen`
* `Integridad` 
* `Anti-replay`
* `Confidencialidad de flujo de datos`

---

# IPSec

 - Capa de red
 - Puerto 500 

## Protocolos

 - Security Associations (SA) <-- "Conexión", Simplex

 - Authentication Header (AH) <-- Integridad, Anti-replay [1]

 - Encapsulating Security Payload (ESP) <-- más utilizado entre ambos [2]

[1] RFC 4302 - https://datatracker.ietf.org/doc/html/rfc4302


[2] RFC 4301, Sección 3.2 - https://datatracker.ietf.org/doc/html/rfc4301#section-3.2

---

# Modos

## Modo Transporte
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

## Modo Túnel
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

# Algoritmos

##### Integridad

##### Encriptación

##### Modo combinado

---

# Formato del paquete ESP

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

**Fuente:** RFC 4303, Sección 2 - https://datatracker.ietf.org/doc/html/rfc4303#section-2

---

```text

░█▄█░█░█░█▀▀░█░█░█▀█░█▀▀░░░█▀▀░█▀▄░█▀█░█▀▀░▀█▀░█▀█░█▀▀░
░█░█░█░█░█░░░█▀█░█▀█░▀▀█░░░█░█░█▀▄░█▀█░█░░░░█░░█▀█░▀▀█░
░▀░▀░▀▀▀░▀▀▀░▀░▀░▀░▀░▀▀▀░░░▀▀▀░▀░▀░▀░▀░▀▀▀░▀▀▀░▀░▀░▀▀▀░
```


