---
title: "10 - GRC - ISO-IEC 27001 y cumplimiento RGPD"
---

# 10. GRC: ISO/IEC 27001 y cumplimiento (RGPD)

## 10.1 Qué es GRC y por qué no es "la parte aburrida" de la seguridad

**GRC** son las siglas de **Gobernanza, Riesgo y Cumplimiento** (*Governance, Risk, Compliance*). Es habitual que, en equipos técnicos, se perciba como la parte burocrática de la seguridad frente a la parte "de verdad" (pentesting, detección, respuesta a incidentes). Esa percepción no resiste el escrutinio: sin gobernanza no hay quien decida la tolerancia al riesgo de la que depende cada control técnico (tema 1), y sin cumplimiento normativo muchas organizaciones simplemente no tienen permitido operar en según qué sectores o jurisdicciones. GRC no sustituye a la seguridad técnica: le da el marco de prioridad y de obligación legal dentro del cual esa seguridad técnica opera.

## 10.2 ISO/IEC 27001: el sistema de gestión, no solo la lista de controles

**ISO/IEC 27001** es la norma internacional de referencia para un **Sistema de Gestión de Seguridad de la Información (SGSI)**. Es importante remarcar, porque es una confusión frecuente, que la norma **no es, en esencia, una lista de controles técnicos**: es un marco de gestión —basado, como casi toda la familia de normas ISO de sistemas de gestión, en el ciclo Planificar-Hacer-Verificar-Actuar— que exige a la organización certificada definir el alcance de su SGSI, evaluar sistemáticamente sus riesgos, seleccionar los controles proporcionados a esos riesgos, y someterse a auditoría externa periódica para mantener la certificación.

Los controles concretos que la organización selecciona, tras su propia evaluación de riesgo, se listan en el **Anexo A**. La revisión de **2022** reorganizó ese anexo de forma sustancial frente a la edición de 2013: pasó de **114 controles en 14 dominios** a **93 controles agrupados en cuatro temas**:

| Tema | Rango | Número de controles | Qué cubre |
|---|---|---|---|
| Organizacionales | A.5.1 – A.5.37 | 37 | Políticas, roles, gestión de proveedores, gestión de incidentes a nivel organizativo |
| Personas | A.6.1 – A.6.8 | 8 | Formación, acuerdos de confidencialidad, proceso disciplinario |
| Físicos | A.7.1 – A.7.14 | 14 | Perímetro físico, equipos, eliminación segura de medios |
| Tecnológicos | A.8.1 – A.8.34 | 34 | Control de acceso técnico, criptografía, prevención de fuga de datos, gestión de vulnerabilidades |

La reducción del número total de controles no refleja una norma menos exigente: refleja, sobre todo, la **consolidación** de controles que en 2013 estaban fragmentados en varios puntos y que la revisión de 2022 unificó, además de incorporar categorías nuevas que no existían en 2013 (como controles específicos sobre seguridad en la nube o sobre prevención de fuga de datos) para reflejar el panorama tecnológico actual.

## 10.3 Un matiz importante: certificación no es garantía de seguridad perfecta

Estar certificado en ISO/IEC 27001 demuestra que una organización **tiene un proceso sistemático** de gestión de riesgo de seguridad de la información, auditado por un tercero independiente — no demuestra que esa organización sea invulnerable, ni siquiera que haya seleccionado el nivel de control óptimo para cada riesgo. Una organización puede estar perfectamente certificada y aun así sufrir un incidente grave, si su propia evaluación de riesgo —documentada, auditada y conforme a la norma— simplemente subestimó una amenaza concreta. La certificación es una garantía de **proceso**, no de **resultado**.

## 10.4 RGPD: cuando el cumplimiento normativo es, además, ley

El **Reglamento General de Protección de Datos (RGPD)**, vigente en la Unión Europea desde mayo de 2018, es el ejemplo de referencia de marco de cumplimiento que no es voluntario: su incumplimiento conlleva sanciones económicas que pueden alcanzar, en los casos más graves, hasta el 4% de la facturación anual global de la organización infractora o 20 millones de euros, lo que sea mayor. A diferencia de ISO/IEC 27001 (un marco de gestión, adoptado voluntariamente para demostrar rigor), el RGPD impone obligaciones legales concretas sobre cómo se tratan los datos personales de personas en la UE, con independencia de dónde esté domiciliada la organización que los trata, entre ellas:

- **Principio de minimización:** no recoger ni conservar más datos personales de los estrictamente necesarios para la finalidad declarada.
- **Base legal explícita** para cada tratamiento de datos (consentimiento, contrato, interés legítimo, obligación legal, entre otras).
- **Notificación de brechas de seguridad** a la autoridad de control competente en un plazo de 72 horas desde que se tiene conocimiento de la brecha, cuando exista riesgo para los derechos y libertades de las personas afectadas.
- **Derechos de las personas** (acceso, rectificación, supresión —el "derecho al olvido"—, portabilidad) que la organización debe estar en condiciones técnicas y organizativas de atender.
- **Seguridad del tratamiento "por diseño y por defecto"** (*privacy by design*): la protección de datos personales debe incorporarse desde el diseño inicial de cualquier sistema que los trate, no añadirse como parche posterior.

## 10.5 Cómo encajan ambos marcos en la práctica

No son excluyentes: una organización certificada en ISO/IEC 27001, con sus controles técnicos y organizativos ya sistematizados, parte con buena parte del trabajo técnico ya hecho para cumplir los requisitos de seguridad del tratamiento que exige el RGPD —el propio Anexo A de controles tecnológicos (cifrado, control de acceso, gestión de incidentes) es, en gran medida, lo que un auditor de protección de datos esperaría encontrar implementado—. La diferencia de fondo es de naturaleza: ISO/IEC 27001 certifica un **proceso de gestión de riesgo**, elegido voluntariamente; el RGPD impone **obligaciones legales concretas** sobre datos personales, con sanción directa del regulador si no se cumplen, se tenga o no un SGSI certificado.

## Bibliografía

- ISO/IEC (2022). *ISO/IEC 27001:2022 — Information security, cybersecurity and privacy protection — Information security management systems — Requirements*.
- ISO/IEC (2022). *ISO/IEC 27002:2022 — Information security controls* (guía detallada de implementación de los controles del Anexo A).
- Unión Europea (2016). *Reglamento (UE) 2016/679 (RGPD/GDPR)*. Diario Oficial de la Unión Europea.
- Agencia Española de Protección de Datos (AEPD). Guías de cumplimiento y notificación de brechas de seguridad.
