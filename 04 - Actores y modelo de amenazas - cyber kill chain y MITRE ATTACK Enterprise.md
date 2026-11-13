---
title: "04 - Actores y modelo de amenazas - cyber kill chain y MITRE ATTACK Enterprise"
---

# 4. Actores y modelo de amenazas: cyber kill chain y MITRE ATT&CK Enterprise

## 4.1 Quién ataca y por qué: una taxonomía mínima de actores

No todos los adversarios tienen las mismas capacidades ni los mismos objetivos, y diseñar defensas sin tener claro contra quién se defiende es, en la práctica, diseñar a ciegas. Una clasificación útil, aunque simplificada:

- **Ciberdelincuencia con ánimo de lucro:** el actor más numeroso hoy. Operan con modelos de negocio reconocibles —**Ransomware-as-a-Service (RaaS)**, mercados de credenciales robadas, redes de afiliados— y su motivación es económica, no ideológica. DarkSide, el grupo detrás de Colonial Pipeline, encaja en esta categoría.
- **Actores patrocinados por estados (APT):** con recursos, tiempo y objetivos sostenidos en el tiempo (espionaje, sabotaje, posicionamiento estratégico). NotPetya, atribuido por varios gobiernos a la inteligencia militar rusa (GRU), es el ejemplo de referencia de un ataque con apariencia de ransomware cuyo objetivo real no era el lucro sino la disrupción.
- **Hacktivistas:** motivación ideológica o política, con capacidad técnica muy variable y un interés documentado, desde 2022, en infraestructura crítica como objetivo simbólico (fenómeno ya mencionado en el curso paralelo de ciberseguridad OT).
- **Insiders:** empleados o contratistas con acceso legítimo que lo usan de forma maliciosa (o, con más frecuencia de lo que se reconoce, de forma negligente). No necesitan "entrar": ya están dentro.

## 4.2 La cyber kill chain: un modelo lineal y sus límites

Propuesto por Lockheed Martin, el modelo de la **cyber kill chain** describe un ataque como una secuencia de siete fases: *Reconnaissance* (reconocimiento), *Weaponization* (preparación del arma), *Delivery* (entrega), *Exploitation* (explotación), *Installation* (instalación), *Command & Control* (C2) y *Actions on Objectives* (acciones sobre el objetivo). Su valor pedagógico es real: comunica con claridad la idea de que **romper la cadena en cualquier eslabón detiene el ataque completo**, lo que justifica invertir en defensa en profundidad en vez de depender de un único control.

Su limitación, reconocida ampliamente en el sector, es que modela un ataque como una secuencia lineal de intrusión externa, lo que encaja mal con escenarios como el *insider* que ya tiene acceso (no hay "entrega" que bloquear) o con el movimiento lateral moderno, mucho más iterativo y no lineal que una cadena de siete pasos. Es precisamente ese hueco el que MITRE ATT&CK Enterprise vino a llenar con un modelo más granular y no estrictamente secuencial.

## 4.3 MITRE ATT&CK Enterprise

Si la kill chain responde "en qué fase general está el ataque", **MITRE ATT&CK** responde "con qué técnica concreta, documentada en incidentes reales, se está ejecutando esa fase". La matriz Enterprise organiza el comportamiento adversario en **catálogos de tácticas** (el "qué" quiere lograr el atacante) y **técnicas** (el "cómo" lo logra), con el listado clásico de catorce tácticas que va de *Reconnaissance* a *Impact*:

*Reconnaissance* → *Resource Development* → *Initial Access* → *Execution* → *Persistence* → *Privilege Escalation* → *Defense Evasion* → *Credential Access* → *Discovery* → *Lateral Movement* → *Collection* → *Command and Control* → *Exfiltration* → *Impact*

A diferencia de la kill chain, estas tácticas **no son estrictamente secuenciales**: un adversario puede ejecutar técnicas de *Discovery* y de *Defense Evasion* de forma intercalada y repetida durante toda la intrusión, no una sola vez en un orden fijo. Cada táctica se descompone a su vez en decenas de técnicas y sub-técnicas concretas (por ejemplo, dentro de *Credential Access*, técnicas específicas como el volcado de credenciales de memoria o el *phishing* dirigido a credenciales), todas documentadas con referencias a grupos de amenaza reales e incidentes públicos que las usaron.

Conviene señalar, con la cautela que merece una matriz en evolución activa, que versiones recientes del framework (en torno a 2026 según fuentes del sector, sin confirmación en fuente primaria al redactar este tema) habrían dividido la táctica *Defense Evasion* en dos tácticas más específicas — es el tipo de cambio de versión que conviene siempre verificar directamente contra attack.mitre.org antes de citarlo con precisión en un informe o una presentación.

## 4.4 Para qué sirve en la práctica, más allá de ser un catálogo

Los usos reales de ATT&CK Enterprise en un programa de seguridad maduro son, en esencia, los mismos tres que ya se describieron para su variante ICS en el curso de ciberseguridad OT: evaluar huecos de cobertura de detección técnica por técnica, estructurar ejercicios de simulación de adversario (tema 13), y dar una base estructurada y comparable a los informes de inteligencia de amenazas en vez de depender de descripciones en prosa no normalizadas entre proveedores distintos.

## Bibliografía

- MITRE. *ATT&CK Enterprise Matrix*. https://attack.mitre.org/matrices/enterprise/
- Lockheed Martin. *Cyber Kill Chain®*. https://www.lockheedmartin.com/en-us/capabilities/cyber/cyber-kill-chain.html
- CISA. Advisorios públicos de atribución de amenazas que referencian tácticas y técnicas ATT&CK (p. ej., Volt Typhoon).
- Cobertura pública sobre la atribución de NotPetya a actores estatales rusos (GRU), incluyendo declaraciones gubernamentales conjuntas de EE. UU. y Reino Unido (2018).
