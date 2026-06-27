# Mi Laboratorio de Seguridad OT/ICS — Bitácora de Aprendizaje

> Documentación de mi proceso de aprendizaje en seguridad de Tecnología Operativa (OT) y Sistemas de Control Industrial (ICS). Voy montando un laboratorio práctico desde cero y registrando lo que aprendo, incluyendo los problemas reales y cómo los resolví. Estudiante de Ingeniería en Conectividad y Redes, con experiencia en infraestructura técnica.

---

## ¿Por qué OT?

La seguridad de sistemas industriales (minería, energía, manufactura, pesca, transporte) es un campo crítico y en crecimiento. Los ataques a estos sistemas no solo afectan datos: pueden detener producción, dañar equipos o poner en riesgo la seguridad física. Quiero especializarme en proteger estos entornos.

---

## Mi laboratorio (en construcción)

| Componente | Herramienta | Estado |
|------------|-------------|--------|
| Controlador (PLC) | OpenPLC Runtime | Instalado y funcionando |
| Editor de programas | OpenPLC Editor (Beremiz) | Instalado (Linux) |
| Pantallas (HMI/SCADA) | ScadaBR | Pendiente |
| Análisis de tráfico | Wireshark | Pendiente |
| Red industrial | Cisco Packet Tracer / GNS3 | Pendiente |
| Fábrica simulada | ICSSIM | Pendiente |

---

## Bitácora

### Entrada 1 — Montando OpenPLC y primer acercamiento a la programación de PLCs

**Fecha:** 23 de junio de 2026

**Objetivo:** instalar OpenPLC (runtime + editor) como primer componente de mi laboratorio OT, y crear un primer programa de control en lenguaje Ladder.

#### Qué es un PLC
Un PLC (Controlador Lógico Programable) es la computadora industrial que controla máquinas y procesos físicos en una planta (motores, válvulas, bombas). Funciona en un ciclo constante llamado *scan*:

1. **Leer entradas** (sensores del mundo físico)
2. **Ejecutar la lógica** (las reglas programadas)
3. **Escribir salidas** (accionar motores, válvulas, etc.)

Repite este ciclo miles de veces por segundo, 24/7. Por eso, en OT, la **disponibilidad** es la prioridad #1 (al revés que en IT, donde lo es la confidencialidad).

#### Qué hice
- Instalé **OpenPLC Runtime** en Ubuntu (el "cerebro" del PLC). Compiló correctamente, incluyendo soporte para el protocolo **Siemens S7** (el mismo que fue atacado por Stuxnet).
- Accedí al panel web del runtime en el panel web local del runtime.
- Instalé el **OpenPLC Editor** (Beremiz), resolviendo varios problemas de dependencias (ver sección de problemas).
- Aprendí a crear un **POU** (Program Organization Unit) en lenguaje **Ladder (LD)**.
- Declaré variables de control:
  - `sensor_nivel` — entrada (BOOL): indica si el nivel de un estanque está bajo.
  - `bomba` — salida (BOOL): enciende o apaga la bomba.
- Dibujé la lógica Ladder: un contacto (sensor) conectado a una bobina (bomba), representando *"si el nivel está bajo, enciende la bomba"*.

#### Problemas que resolví (lo más valioso)
Montar el OpenPLC Editor en Linux requirió resolver varias dependencias de Python una por una:

| Problema | Solución |
|----------|----------|
| `No module named 'wx'` | Instalé `python3-wxgtk4.0` desde apt |
| El editor usaba un venv aislado que no veía wxPython | Recreé el venv con `--system-site-packages` |
| `No module named 'numpy'` | `pip install numpy` en el venv |
| `No module named 'matplotlib'` | `pip install matplotlib` |
| `No module named 'lxml'` | `pip install lxml` (y otras dependencias) |
| Error de compilación: contacto sin conectar / RecursionError | Identifiqué problemas en las conexiones gráficas del Ladder (pendiente de resolver) |

#### Qué aprendí
- Cómo funciona internamente un PLC (ciclo de scan).
- Los fundamentos del lenguaje Ladder (contactos, bobinas, rieles de energía).
- La diferencia entre variables de entrada y salida, y el tipo BOOL.
- **Gestión de entornos virtuales de Python** y resolución de dependencias en Linux (una habilidad transferible a cualquier trabajo de TI/seguridad).
- Un detalle de seguridad importante: los servicios OT pueden quedar expuestos en la red local por configuración por defecto, y hay que asegurarlos (segmentación, no exponer paneles de control).

#### Pendiente para la próxima sesión
- Resolver la generación de código del programa Ladder (el editor en Linux dio problemas con las conexiones gráficas; evaluaré usar el OpenPLC Editor en Windows, que es más estable).
- Cargar el programa en el runtime y verlo ejecutarse.
- Conectar una interfaz SCADA/HMI para supervisar el proceso.

---

## Conceptos OT que voy aprendiendo

- **PLC (Controlador Lógico Programable):** computadora industrial que controla procesos físicos mediante un ciclo de leer entradas, ejecutar lógica y escribir salidas.
- **HMI (Interfaz Humano-Máquina):** las pantallas que usan los operadores para supervisar y controlar la planta.
- **SCADA:** sistema de supervisión y control que recoge datos de los PLCs y permite operar el proceso a distancia.
- **Ladder (LD):** lenguaje de programación de PLCs con forma de diagrama eléctrico (contactos y bobinas).
- **Ciclo de scan:** el ciclo constante leer-decidir-accionar que ejecuta un PLC.
- **Modelo Purdue:** *(pendiente de profundizar)* arquitectura de referencia que organiza la red industrial en niveles para segmentar IT de OT.

---

## Casos de estudio analizados

- **Stuxnet (2010):** malware que saboteó centrifugadoras nucleares en Irán manipulando PLCs Siemens. Enseña que: el daño en OT es físico, el aislamiento (air-gap) no es suficiente, y los atacantes pueden engañar a los operadores mostrando datos falsos.

---

## Recursos que estoy usando

- **OpenPLC** (runtime + editor) — controlador de código abierto.
- **Cisco Packet Tracer / GNS3** — redes industriales (pendiente).
- **Wireshark** — análisis de tráfico (pendiente).
- Material de **CISA** y **SANS** sobre seguridad ICS.
- Casos de estudio: Stuxnet, Industroyer, Triton.

---

## Reflexión

El primer día de laboratorio OT me enseñó tanto de los conceptos (qué es un PLC, cómo se programa) como de la realidad técnica de montar herramientas industriales: muchas son de nicho, con dependencias complejas, y requieren paciencia y capacidad de diagnóstico. Resolver esos problemas es parte del trabajo real en ciberseguridad OT.

---

*Bitácora de aprendizaje en seguridad OT/ICS · En construcción · 2026*
