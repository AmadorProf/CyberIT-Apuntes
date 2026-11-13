---
title: "13 - Threat Intelligence y Red-Blue-Purple Teaming"
---

# 13. Threat Intelligence y Red/Blue/Purple Teaming

## 13.1 Inteligencia de amenazas: de datos en bruto a decisiones

**Threat Intelligence (inteligencia de amenazas)** no es, pese a la confusión frecuente con el término, una lista de indicadores técnicos sin más: es el proceso de **convertir información sobre adversarios en algo que permite tomar una decisión mejor**. Se organiza habitualmente en tres niveles, cada uno dirigido a una audiencia distinta:

- **Estratégica:** dirigida a dirección y a quien fija presupuesto y prioridades. Responde a preguntas como "¿qué sectores y qué tipo de actor representan hoy el mayor riesgo para nuestro negocio concreto?", sin entrar en detalle técnico.
- **Operativa:** dirigida a responsables de seguridad que planifican campañas de defensa a medio plazo. Describe campañas, capacidades y objetivos de grupos de amenaza concretos.
- **Táctica:** dirigida a analistas de SOC (tema 11) en el día a día. Son los indicadores técnicos concretos y accionables de inmediato: direcciones IP maliciosas, hashes de malware, dominios de C2.

## 13.2 La Pirámide del Dolor: por qué no todos los indicadores valen lo mismo

El analista David Bianco formalizó una idea que estructura buena parte de la inteligencia de amenazas táctica moderna: la **Pirámide del Dolor** (*Pyramid of Pain*), que ordena los tipos de indicador según cuánto esfuerzo le cuesta al atacante cambiarlos cuando se da cuenta de que han sido detectados:

- **Valores hash:** triviales de cambiar (recompilar el mismo malware genera un hash distinto). Casi ningún coste para el atacante.
- **Direcciones IP:** algo más de fricción, pero sigue siendo barato rotar de infraestructura.
- **Nombres de dominio:** algo más costoso (requiere registrar dominios nuevos), pero sigue siendo relativamente accesible.
- **Artefactos de red y de host:** patrones más estructurales del malware o de su comportamiento en el sistema, más costosos de rediseñar por completo.
- **Herramientas:** si se detecta y se bloquea la herramienta concreta que usa el atacante, tiene que desarrollar o adquirir una alternativa, un coste de tiempo y recursos ya significativo.
- **Tácticas, Técnicas y Procedimientos (TTP):** la cúspide de la pirámide. Detectar y defender contra el **comportamiento** de fondo de un adversario (exactamente lo que cataloga MITRE ATT&CK, tema 4) obliga al atacante a cambiar su forma de operar de manera fundamental, no solo una herramienta o una dirección IP — es, con diferencia, lo más caro de evadir, y por tanto donde la inteligencia de amenazas aporta más valor defensivo sostenido en el tiempo.

El mensaje práctico de la pirámide es claro: bloquear hashes e IP tiene valor inmediato pero bajo, porque el atacante los rota sin esfuerzo; construir detección basada en TTP cuesta más trabajo analítico por adelantado, pero sigue siendo eficaz mucho después de que el atacante haya cambiado toda su infraestructura técnica superficial.

## 13.3 Estandarización: STIX/TAXII

Para que la inteligencia de amenazas se pueda compartir entre organizaciones de forma automatizada —no solo leída por humanos en un informe PDF—, el sector adoptó estándares abiertos: **STIX (Structured Threat Information eXpression)** define un lenguaje estructurado para describir indicadores, TTP, campañas y relaciones entre ellos; **TAXII (Trusted Automated Exchange of Intelligence Information)** define el protocolo de transporte para compartir esa información STIX entre plataformas de forma automatizada, por ejemplo entre un proveedor de inteligencia de amenazas y el SIEM de una organización cliente.

## 13.4 Red Team, Blue Team y Purple Team

- **Red Team:** simula a un adversario real, con objetivos concretos (por ejemplo, "exfiltrar un documento de una carpeta específica sin ser detectado") y sin restricción artificial a un alcance técnico estrecho, a diferencia de un test de penetración clásico centrado en encontrar vulnerabilidades técnicas puntuales. Su valor no es solo encontrar fallos: es **poner a prueba la capacidad de detección y respuesta real** de la organización frente a un comportamiento adversario sostenido en el tiempo.
- **Blue Team:** el equipo defensivo —en la práctica, el SOC del tema 11— cuya misión es detectar y responder a esa simulación (y, por supuesto, a los ataques reales) sin que se le informe necesariamente del momento ni del vector exacto que el Red Team va a usar, para que el ejercicio mida la capacidad de detección real y no una respuesta preparada de antemano.
- **Purple Team:** no es un tercer equipo separado, sino una **función de colaboración activa** entre Red y Blue: en vez de que el Red Team actúe de forma completamente aislada y entregue un informe al final, Purple Team comparte en tiempo cuasi-real qué técnica concreta se está ejecutando, para que el Blue Team pueda verificar de inmediato si su instrumentación la detectó o no, y ajustar sus reglas de detección sobre la marcha. Es, en esencia, acelerar el ciclo de aprendizaje que de otro modo tardaría semanas en materializarse tras la entrega de un informe final.

## 13.5 Adversary emulation: llevar ATT&CK del catálogo a la simulación

La práctica más madura de ejercicio Red/Purple Team actual no simula "un ataque genérico", sino que realiza **emulación de adversario**: reproduce, técnica por técnica, el comportamiento documentado de un grupo de amenaza real y relevante para el sector de la organización, usando directamente la matriz de MITRE ATT&CK Enterprise (tema 4) como guion. Esto cierra el círculo completo del temario: la inteligencia de amenazas (este tema) identifica qué actor y qué TTP son relevantes; ATT&CK (tema 4) cataloga esas TTP de forma estructurada; la emulación de adversario las reproduce de forma controlada; y el SOC (tema 11), con sus reglas de SIEM diseñadas sobre la misma matriz, debería —si está bien construido— detectarlas. Cuando no las detecta, ese hueco concreto, verificado con evidencia real y no con una suposición, es exactamente el dato que informa en qué invertir primero en el ciclo de mejora continua de la función **Gobernar** con la que se abrió este temario.

## Bibliografía

- Bianco, D. (2013). *The Pyramid of Pain*. http://detect-respond.blogspot.com/2013/03/the-pyramid-of-pain.html
- OASIS Open. *STIX 2.1 / TAXII 2.1 Specifications*. https://oasis-open.github.io/cti-documentation/
- MITRE. *ATT&CK Enterprise*, base habitual de los ejercicios de *adversary emulation* (p. ej. MITRE ATT&CK Evaluations).
- NIST (2008, con guías relacionadas posteriores). *SP 800-115 — Technical Guide to Information Security Testing and Assessment*.
- SANS Institute. Metodologías de Red Team, Blue Team y Purple Team en programas de seguridad ofensiva y defensiva.
