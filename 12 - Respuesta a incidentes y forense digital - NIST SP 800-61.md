---
title: "12 - Respuesta a incidentes y forense digital - NIST SP 800-61"
---

# 12. Respuesta a incidentes y forense digital (NIST SP 800-61)

## 12.1 De la detección a la acción

El tema 11 describió cómo un SOC detecta que algo anómalo está ocurriendo. Este tema cubre qué pasa **después** de esa detección: el proceso formal de **respuesta a incidentes**, cuya referencia más citada a nivel internacional es la guía **NIST SP 800-61**, cuya revisión más reciente —**Rev. 3**, finalizada en abril de 2025— marca un cambio de enfoque significativo frente a ediciones anteriores.

## 12.2 El modelo clásico (Rev. 2) frente al modelo actual (Rev. 3)

La **Revisión 2** (2012), durante más de una década el documento de referencia del sector, organizaba la respuesta a incidentes en un ciclo de vida propio de cuatro fases: **Preparación**, **Detección y Análisis**, **Contención, Erradicación y Recuperación**, y **Actividad Post-Incidente**. Es un modelo todavía ampliamente enseñado y citado, y conviene conocerlo porque buena parte de la literatura y de las certificaciones profesionales del sector siguen usando ese vocabulario.

La **Revisión 3** (2025) toma una decisión de fondo distinta: en vez de mantener un ciclo de vida de respuesta a incidentes independiente, **integra sus recomendaciones directamente dentro de las seis funciones del NIST CSF 2.0** (tema 1) — Gobernar, Identificar, Proteger, Detectar, Responder, Recuperar —, presentándose explícitamente como un **perfil de comunidad de CSF 2.0** centrado en la gestión de riesgo de ciberseguridad, no como una guía de respuesta aislada. La razón declarada de este cambio es dejar claro que la respuesta a incidentes no es una actividad separada del resto de la gestión de riesgo de la organización, sino una instancia concreta —activada cuando algo falla— de las mismas capacidades de gobernanza, identificación y protección que ya deberían existir de forma continua.

## 12.3 Las fases, en la práctica, independientemente del modelo que se use para nombrarlas

Con independencia de si una organización sigue formalmente el modelo de cuatro fases de la Rev. 2 o el enfoque integrado de la Rev. 3, las actividades reales de una respuesta a incidentes competente son, en esencia, las mismas:

1. **Preparación:** disponer, antes de que ocurra cualquier incidente, de un plan documentado, roles y responsabilidades claros (incluyendo quién tiene autoridad para decisiones críticas como desconectar un sistema de producción), herramientas forenses listas, y canales de comunicación —tanto técnicos como legales y de relaciones públicas— ya acordados. La preparación es, con diferencia, la fase que más determina si los días siguientes al descubrimiento de un incidente grave son una ejecución ordenada o un caos improvisado.
2. **Detección y análisis:** confirmar que lo observado es, en efecto, un incidente de seguridad (no un falso positivo ni un problema puramente operativo), determinar su alcance real —qué sistemas, qué datos, desde cuándo— y clasificar su severidad para priorizar la respuesta.
3. **Contención:** limitar el daño sin destruir evidencia necesaria para el análisis forense posterior. La contención a corto plazo (aislar un sistema comprometido de la red inmediatamente) y la contención a largo plazo (aplicar medidas más permanentes mientras se prepara la erradicación completa) suelen requerir decisiones distintas y, a veces, en tensión entre sí: desconectar de inmediato puede alertar al atacante de que ha sido detectado, perdiendo la oportunidad de observar su comportamiento para entender mejor el alcance.
4. **Erradicación:** eliminar la causa raíz del incidente —no solo sus síntomas visibles—, lo que exige haber identificado correctamente el vector de entrada; erradicar sin haberlo identificado casi garantiza una reinfección.
5. **Recuperación:** restaurar los sistemas afectados a una operación normal y verificada, con monitorización reforzada temporal para detectar cualquier señal de que el atacante conserva acceso residual.
6. **Actividad post-incidente (lecciones aprendidas):** la fase más frecuentemente omitida bajo presión operativa, y la que convierte un incidente costoso en, al menos, una mejora real del programa de seguridad: qué falló, qué funcionó, qué cambia en el plan de preparación para la próxima vez.

## 12.4 Forense digital: preservar la evidencia sin contaminarla

El análisis forense digital aplica, al mundo técnico, los mismos principios que el forense tradicional exige para que la evidencia sea útil —y, en casos que terminan en proceso legal, admisible—:

- **Cadena de custodia:** documentar de forma rigurosa quién ha tenido acceso a cada pieza de evidencia, cuándo y con qué propósito, desde el momento de su captura.
- **Orden de volatilidad:** capturar primero lo que desaparece antes — memoria RAM y conexiones de red activas, después discos y almacenamiento persistente, por último copias de seguridad y registros históricos — porque reiniciar un sistema comprometido para "limpiarlo" destruye irremediablemente evidencia en memoria que podría haber revelado al atacante.
- **Copias forenses verificadas (bit a bit)** de cualquier medio de almacenamiento, con verificación de integridad mediante hash (tema 2) antes y después de la copia, para poder demostrar que el análisis se realizó sobre una réplica exacta y no sobre el original, que permanece preservado sin alterar.

## 12.5 La tensión central de toda respuesta a incidentes

Contener rápido reduce el daño operativo pero puede destruir evidencia valiosa o alertar prematuramente al atacante; investigar a fondo antes de actuar preserva mejor la evidencia pero prolonga la exposición. No existe una respuesta universalmente correcta a esa tensión: la resuelve, en cada caso, el criterio del equipo de respuesta, informado por la severidad real del incidente y por la tolerancia al riesgo que la propia organización ya debería haber fijado de antemano, en la fase de preparación — el ciclo vuelve, una vez más, a la función **Gobernar** que abría este temario en el tema 1.

## Bibliografía

- NIST (2025). *SP 800-61 Rev. 3 — Incident Response Recommendations and Considerations for Cybersecurity Risk Management: A CSF 2.0 Community Profile*. https://csrc.nist.gov/pubs/sp/800/61/r3/final
- NIST (2012). *SP 800-61 Rev. 2 — Computer Security Incident Handling Guide* (modelo de cuatro fases, de referencia histórica).
- SANS Institute. *Digital Forensics and Incident Response (DFIR)* — metodología de cadena de custodia y orden de volatilidad.
- ENISA. Guías de buenas prácticas de gestión de incidentes para equipos de respuesta (CSIRT) europeos.
