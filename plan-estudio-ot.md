# Plan de Estudio OT/ICS desde Cero — CTNP / Michelzon

> Ruta completa para aprender seguridad de Tecnología Operativa (OT) e Sistemas de Control Industrial (ICS), de cero a competente, con teoría y práctica usando herramientas gratuitas. Pensado para recorrerse en meses, paso a paso. Conecta con tu experiencia en Metro de Chile, tu conexión en pesca (Perú), y la posibilidad de ampliar CTNP hacia OT.

---

## Cómo usar este plan

Cada fase tiene: **objetivo**, **conceptos a aprender**, **práctica**, y **cómo conecta con tu realidad**. No saltes fases. Dedica el tiempo que necesites a cada una. La práctica es tan importante como la teoría: en OT, entender haciendo es clave.

**Regla de oro:** todo lo que practiques, hazlo en TU laboratorio (simuladores, máquinas virtuales). Nunca en sistemas reales sin autorización formal — ni siquiera en la planta de tu tío sin permiso escrito de la empresa.

---

## FASE 0 — Preparación del entorno (antes de empezar)

**Objetivo:** tener un laboratorio donde practicar sin riesgo.

### Herramientas a instalar (todas gratuitas)
- **VirtualBox o VMware Player** — para máquinas virtuales.
- **GNS3** — emulador de redes (el estándar). Permite construir topologías de red con switches, firewalls, y conectar dispositivos virtuales. Soporta imágenes reales de Cisco y muchos fabricantes.
- **Cisco Packet Tracer** — simulador de redes de Cisco, gratis con registro en Cisco Networking Academy. Más simple que GNS3, ideal para empezar con redes y tiene componentes IoT/industriales básicos.
- **Wireshark** — analizador de tráfico de red (esencial para ver protocolos industriales).

### Por qué estas herramientas
GNS3 y Packet Tracer te permiten modelar la topología de una red industrial (firewalls, switches, segmentación) y conectar dispositivos ICS virtuales para construir un laboratorio que refleje una planta real, sin ningún riesgo.

---

## FASE 1 — Fundamentos: ¿Qué es OT? (2-3 semanas)

**Objetivo:** entender qué es OT, cómo se diferencia de IT, y sus componentes.

### Conceptos clave
1. **OT vs IT:**
   - IT = maneja datos (computadores, redes de oficina, servidores).
   - OT = controla procesos físicos (máquinas, sensores, producción).
   - En IT priorizas Confidencialidad > Integridad > Disponibilidad.
   - En OT es al revés: **Disponibilidad > Integridad > Confidencialidad** (lo más importante es que la planta siga funcionando).

2. **Componentes de un ICS (Sistema de Control Industrial):**
   - **PLC (Controlador Lógico Programable):** el "cerebro" que controla máquinas. Recibe señales de sensores y acciona motores, válvulas, etc.
   - **RTU (Unidad Terminal Remota):** similar al PLC, para ubicaciones remotas.
   - **HMI (Interfaz Humano-Máquina):** las pantallas que ven los operadores para monitorear y controlar.
   - **SCADA (Supervisión, Control y Adquisición de Datos):** el sistema que supervisa y controla todo el proceso desde un centro de control, recogiendo datos de PLCs/RTUs.
   - **Historian:** base de datos que guarda el histórico del proceso (temperaturas, presiones, etc.).
   - **Sensores y actuadores:** los "sentidos" (medir) y "músculos" (accionar) del sistema.

3. **Diferencia SCADA vs ICS:** ICS automatiza y controla los procesos; SCADA provee la supervisión y control en tiempo real. SCADA es parte del mundo ICS.

### Práctica
- Dibuja (en papel o digital) cómo crees que funciona una planta: sensores → PLC → actuadores, y SCADA supervisando.
- Ve documentales/videos sobre cómo funciona una fábrica automatizada.

### Conexión con tu realidad
En Metro: los sistemas de señalización, control de trenes, TETRA — todo eso es OT. Identifica qué componentes (PLC, SCADA, HMI) reconoces en tu trabajo.
En la pesca (tu tío): las plantas de harina de pescado usan PLCs y SCADA para controlar cocinas, secadores, prensas. Pregúntale qué pantallas y sistemas usan.

---

## FASE 2 — Redes industriales (3-4 semanas)

**Objetivo:** entender cómo se conectan los sistemas OT y por qué son vulnerables.

### Conceptos clave
1. **El Modelo Purdue (arquitectura de referencia OT):** organiza la red industrial en niveles:
   - **Nivel 0:** dispositivos de campo (sensores, actuadores).
   - **Nivel 1:** controladores (PLCs, RTUs).
   - **Nivel 2:** supervisión (SCADA, HMI).
   - **Nivel 3:** operaciones de la planta (historians, gestión de producción).
   - **Nivel 3.5 (DMZ industrial):** zona de separación entre OT e IT.
   - **Niveles 4-5:** IT corporativo (oficinas, internet).
   - La seguridad se basa en **segmentar** estos niveles: el nivel 0 nunca debería hablar directo con internet.

2. **Protocolos industriales** (el "idioma" de las máquinas):
   - **Modbus:** muy común, simple, VIEJO y sin seguridad (sin cifrado ni autenticación).
   - **DNP3:** usado en energía/agua.
   - **PROFINET, EtherNet/IP:** industriales sobre Ethernet.
   - **OPC-UA:** más moderno, con seguridad.
   - Clave: muchos protocolos OT se diseñaron hace décadas SIN seguridad, porque se asumía que la red estaba aislada.

3. **Por qué OT es vulnerable:**
   - Sistemas viejos que no se pueden parchear fácil (parar la planta cuesta millones).
   - Protocolos sin cifrado/autenticación.
   - Convergencia IT/OT: al conectar OT a redes IT/internet, se expone.

### Práctica
- **En Packet Tracer o GNS3:** construye una red simple con segmentación: una zona "IT" (oficina) y una zona "OT" (planta), separadas por un firewall. Practica el concepto de DMZ industrial.
- **Con Wireshark:** aprende a capturar y leer tráfico. Más adelante, capturarás tráfico Modbus simulado.

### Conexión con tu realidad
Tu carrera (Conectividad y Redes) te da ventaja aquí: ya entiendes switches, VLANs, firewalls. El Modelo Purdue es aplicar esos conceptos a un entorno industrial. Tu experiencia en Metro con switches encaja directo.

---

## FASE 3 — Laboratorio ICS práctico (4-6 semanas)

**Objetivo:** montar un laboratorio ICS virtual y experimentar de forma segura.

### Herramientas de simulación ICS (gratuitas)
- **ICSSIM:** framework para simular un testbed de ICS para experimentos de ciberseguridad. Incluye un ejemplo de "fábrica de llenado de botellas" con PLCs, corre sobre Docker y se integra con GNS3. Permite simular amenazas y ataques de forma segura.
- **OpenPLC:** un PLC de código abierto que puedes correr en tu PC. Ideal para aprender cómo programar y cómo funciona un PLC real.
- **ScadaBR o Ignition (demo):** software SCADA gratuito/demo para crear pantallas HMI y supervisar procesos simulados.
- **Conpot:** un honeypot que simula dispositivos ICS (habla Modbus, IEC-104) de forma segura — útil para ver cómo se ve un dispositivo industrial en la red.
- **ModbusPal / pymodbus:** para simular dispositivos Modbus y practicar el protocolo.

### Práctica progresiva
1. Instala **OpenPLC** y crea un programa simple (ej: encender una "bomba" si un "sensor de nivel" baja).
2. Conecta una HMI (ScadaBR) a tu OpenPLC y visualiza el proceso.
3. Usa **Wireshark** para capturar el tráfico Modbus entre la HMI y el PLC. Observa que va SIN cifrar (puedes leer los comandos).
4. Con **ICSSIM**, levanta la fábrica de llenado de botellas y estudia cómo interactúan los dos PLCs.

### Conexión con tu realidad
Este laboratorio es tu "campo de pruebas" seguro. Aquí entiendes, sin riesgo, lo que pasa en una planta real como la de tu tío. Cuando hables con él, sabrás de qué hablas.

---

## FASE 4 — Amenazas y ataques OT (estudio, 3-4 semanas)

**Objetivo:** entender cómo se atacan los sistemas OT (para defenderlos).

### Casos reales a estudiar (ya empezaste con Stuxnet)
- **Stuxnet (Irán, 2010):** sabotaje de centrifugadoras. Enseña: daño físico, air-gap no basta, engaño a operadores.
- **Industroyer/CrashOverride (Ucrania, 2016):** causó un apagón. Atacó protocolos de red eléctrica.
- **Triton/Trisis (2017):** atacó sistemas instrumentados de seguridad (SIS) de una planta petroquímica — los sistemas que evitan explosiones. El más peligroso (apuntaba a vidas).
- **Colonial Pipeline (2021):** ransomware que paró un oleoducto en EE.UU. (fue IT, pero paró OT por precaución).

### Conceptos
- **Lo que aprendes de cada caso:** vectores de entrada (USB, phishing, IT→OT), objetivos (sabotaje, parada, daño), y cómo se pudo prevenir.
- **MITRE ATT&CK for ICS:** una versión del framework MITRE específica para OT. Cataloga las técnicas que usan los atacantes en entornos industriales. (Tú ya usas MITRE ATT&CK en CTNP — esta es su versión OT.)

### Práctica (en TU laboratorio, nunca en sistemas reales)
- En tu lab ICS, simula un "ataque" simple: modifica un valor Modbus con pymodbus y observa cómo afecta el proceso simulado. Esto te enseña por qué Modbus sin seguridad es peligroso.

### Conexión con tu realidad
Estos casos son los que convencen a un cliente (como una pesquera) de que necesita seguridad OT. Entenderlos te da los argumentos.

---

## FASE 5 — Defensa y monitoreo OT (4-6 semanas)

**Objetivo:** aprender a proteger y monitorear entornos OT.

### Conceptos clave
1. **Segmentación IT/OT:** separar las redes (Modelo Purdue), firewalls industriales, DMZ.
2. **Monitoreo pasivo:** en OT NO puedes escanear agresivamente (puedes tumbar un PLC viejo). Se monitorea de forma pasiva, observando el tráfico sin interferir.
3. **Detección de anomalías:** como el tráfico OT es predecible (las máquinas hacen lo mismo siempre), se detecta lo anómalo.
4. **IEC 62443:** el estándar internacional de seguridad para sistemas de automatización industrial. La "biblia" de la seguridad OT. Empieza por entender su estructura.
5. **Control de accesos, gestión de dispositivos USB, hardening de HMIs.**

### Herramientas de monitoreo OT
- **Wazuh (¡el que ya usas!):** puede monitorear hosts en entornos OT (HMIs, historians basados en Windows/Linux).
- **Security Onion:** plataforma de monitoreo de red (puede analizar tráfico OT).
- **Zeek:** analizador de tráfico que entiende algunos protocolos industriales.
- Herramientas comerciales OT (a futuro): Nozomi, Claroty, Dragos (las líderes; para conocer el mercado).

### Práctica
- Conecta Wazuh a una VM que simule un HMI (Windows/Linux) en tu lab y monitoréala.
- Con Zeek/Wireshark, analiza el tráfico de tu lab ICS y detecta un cambio "anómalo".

### Conexión con tu realidad / CTNP
**Aquí es donde OT amplía tu SOC.** Wazuh, que ya usas en CTNP, sirve para monitorear parte del entorno OT. Podrías evolucionar CTNP para ofrecer monitoreo OT básico a PyMEs industriales. Es el puente natural entre lo que ya tienes y OT.

---

## FASE 6 — Especialización y certificación (largo plazo)

**Objetivo:** profesionalizar tu conocimiento OT.

### Formación
- **Recursos gratuitos:** material de CISA (gobierno EE.UU.) sobre ICS security, posters y webinars de SANS ICS, cursos introductorios de Fortinet/OPSWAT.
- **Certificaciones (a futuro, cuando tengas base):**
  - **GICSP (Global Industrial Cyber Security Professional):** la más reconocida en OT.
  - **SANS ICS410:** el curso de referencia (caro, pero el mejor).
  - **ISA/IEC 62443:** certificaciones sobre el estándar.

### Conexión con tu realidad
Una certificación OT + tu experiencia en Metro + tu laboratorio + tu conexión en Perú = un perfil OT serio, que pocos tienen en la región.

---

## Cómo esto amplía CTNP y tu futuro

1. **CTNP + OT:** tu SOC ya tiene la base (Wazuh, monitoreo, MITRE). Sumar OT sería ofrecer monitoreo de seguridad a PyMEs industriales (un nicho con menos competencia y que paga más).
2. **Mercado peruano:** pesca, minería, energía — todos con OT. Tu conexión (tío en pesca) + ser de Perú = puerta de entrada para aprender el sector.
3. **Con Marco:** si ambos se preparan en OT, es una línea de negocio compartida a futuro.
4. **Empleo:** saber de OT te abre puertas a roles de "Analista SOC OT" o "Seguridad Industrial", muy demandados y mejor pagados.

---

## Resumen de la ruta

| Fase | Tema | Duración aprox. | Práctica principal |
|------|------|-----------------|--------------------|
| 0 | Preparar laboratorio | 1 semana | Instalar GNS3, Packet Tracer, VirtualBox |
| 1 | Fundamentos OT | 2-3 semanas | Identificar componentes ICS |
| 2 | Redes industriales | 3-4 semanas | Segmentación en Packet Tracer/GNS3 |
| 3 | Laboratorio ICS | 4-6 semanas | OpenPLC + SCADA + Wireshark |
| 4 | Amenazas OT | 3-4 semanas | Estudiar casos + simular en lab |
| 5 | Defensa y monitoreo | 4-6 semanas | Wazuh + Zeek en lab OT |
| 6 | Certificación | Largo plazo | GICSP / SANS / IEC 62443 |

---

## Principios para tu aprendizaje OT

- **Seguridad y legalidad primero:** practica solo en tu laboratorio. Nunca en sistemas reales sin autorización formal de la empresa dueña.
- **Teoría + práctica:** en OT, entender haciendo es esencial. Monta el laboratorio.
- **Aprovecha lo que tienes:** Metro (entorno OT real), tu carrera (redes), CTNP (monitoreo), tu tío (sector pesca).
- **Paso a paso:** OT es un maratón, no un sprint. Domina cada fase antes de avanzar.
- **Conecta con el negocio:** cada cosa que aprendas, piensa cómo aplica a CTNP o a un cliente real.

---

*CTNP Security · Plan de estudio OT/ICS · 2026 · Documento de aprendizaje*
