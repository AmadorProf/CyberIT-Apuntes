---
title: "01 - Fundamentos de seguridad de la informacion - CIA riesgo y NIST CSF 2.0"
---

# 1. Fundamentos de seguridad de la información: CIA, riesgo y NIST CSF 2.0

## 1.1 La tríada CIA

Toda la disciplina de seguridad de la información se puede resumir, de forma simplificada pero útil, en proteger tres propiedades de la información y los sistemas que la procesan:

- **Confidencialidad:** que solo acceda a la información quien está autorizado a hacerlo.
- **Integridad:** que la información no se altere de forma no autorizada ni indetectada.
- **Disponibilidad:** que la información y los sistemas estén accesibles cuando se necesitan.

A diferencia del mundo OT visto en el curso paralelo de ciberseguridad industrial —donde, como se explicaba allí, el orden práctico se invierte hacia disponibilidad e integridad—, en el grueso de la seguridad de la información **IT convencional** la confidencialidad suele encabezar la lista de prioridades, simplemente porque el daño más frecuente y mejor cuantificado (una filtración de datos personales o de propiedad intelectual) es un daño de confidencialidad. Esto no es una regla fija: para un proveedor de comercio electrónico en plena campaña de rebajas, la disponibilidad puede pesar tanto o más que la confidencialidad.

## 1.2 Seguridad no es ausencia de riesgo, es gestión de riesgo

El error conceptual más común entre quien empieza en la disciplina es tratar la seguridad como un estado binario ("seguro" / "inseguro"). La aproximación profesional es la **gestión de riesgo**: el riesgo es función de la probabilidad de que ocurra un evento adverso y el impacto que tendría si ocurre, y la organización decide, de forma consciente, cuánto riesgo está dispuesta a **aceptar**, cuánto va a **mitigar** con controles, cuánto va a **transferir** (seguros, contratos con terceros) y cuánto va a **evitar** (no desplegar un servicio determinado). Ningún control reduce el riesgo a cero: reduce la probabilidad o el impacto, a un coste. Decidir dónde se detiene ese gasto frente al riesgo residual aceptado es, en última instancia, una decisión de negocio, no una decisión puramente técnica — aunque sea el equipo técnico quien aporte los datos para tomarla con criterio.

## 1.3 El marco de referencia: NIST Cybersecurity Framework 2.0

El **NIST Cybersecurity Framework (CSF)** es, con diferencia, el marco de gestión de riesgo de ciberseguridad más citado a nivel internacional, pese a ser de origen estadounidense y de adopción voluntaria. Su versión **2.0**, finalizada en febrero de 2024 tras un borrador público en 2023, reorganiza la disciplina en **seis funciones** que se ejecutan en paralelo, no de forma estrictamente secuencial (NIST, 2024; LogicGate, s.f.; SecureControlsFramework, s.f.):

| Función | Qué responde | Ejemplos de actividad |
|---|---|---|
| **Gobernar (Govern)** — nueva en la 2.0 | ¿Quién decide, con qué estrategia y qué tolerancia al riesgo? | Políticas, roles y responsabilidades, gestión de riesgo de la cadena de suministro |
| **Identificar (Identify)** | ¿Qué tengo que proteger y qué riesgo corre? | Inventario de activos, evaluación de riesgos, contexto de negocio |
| **Proteger (Protect)** | ¿Qué controles reducen la probabilidad o el impacto? | Control de acceso, formación, cifrado, configuración segura |
| **Detectar (Detect)** | ¿Cómo sé que algo está pasando ahora mismo? | Monitorización continua, análisis de anomalías |
| **Responder (Respond)** | ¿Qué hago cuando ya ha pasado? | Triaje, contención, erradicación, comunicación |
| **Recuperar (Recover)** | ¿Cómo vuelvo a la normalidad? | Restauración de sistemas y datos, lecciones aprendidas |

La incorporación de **Gobernar** como función explícita —y no como un apéndice de Identificar, como ocurría en la versión 1.1— es el cambio más comentado de la 2.0: refleja el consenso de que buena parte de los fallos de seguridad de los últimos años no fueron fallos técnicos sino fallos de **gobernanza**: nadie tenía claro quién era responsable de decidir, nadie había fijado una tolerancia al riesgo explícita, y la gestión de riesgo de proveedores terceros —que en la 2.0 se mueve precisamente a Gobernar— quedaba en tierra de nadie (industrialcyber.co, 2024). El marco completo, en su detalle operativo, se despliega en **22 categorías y 106 subcategorías** de referencia (LogicGate, s.f.).

## 1.4 Por qué este marco aparece una y otra vez en este temario

NIST CSF 2.0 no es solo el tema de apertura: es el esqueleto sobre el que se apoyan explícitamente otros documentos de referencia que se verán más adelante. La revisión más reciente de la guía de respuesta a incidentes de NIST, **SP 800-61 Rev. 3** (tema 12), abandona su antiguo ciclo de vida propio de cuatro fases y pasa a organizar sus recomendaciones directamente sobre las funciones de CSF 2.0 (NIST, 2025) — una señal clara de hacia dónde está convergiendo el vocabulario de referencia de todo el sector.

## Bibliografía

- NIST (2024). *The NIST Cybersecurity Framework (CSF) 2.0*. National Institute of Standards and Technology. https://www.nist.gov/cyberframework
- Industrial Cyber (2024). *NIST releases CSF 2.0 focused on governance and supply chain security across sectors*. https://industrialcyber.co/nist/nist-releases-csf-2-0-focused-on-governance-and-supply-chain-security-across-sectors/
- LogicGate (s.f.). *Understanding NIST CSF 2.0 Cybersecurity Framework*. https://logicgate.com/blog/understanding-nist-csf-2-0-cybersecurity-framework
- Secure Controls Framework (s.f.). *NIST CSF 2.0*. https://securecontrolsframework.com/grc-fundamentals/common-cybersecurity-frameworks/nist-csf-2-0
- NIST (2025). *SP 800-61 Rev. 3 — Incident Response Recommendations and Considerations for Cybersecurity Risk Management*.
