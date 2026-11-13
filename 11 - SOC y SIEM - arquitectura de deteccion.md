---
title: "11 - SOC y SIEM - arquitectura de deteccion"
---

# 11. SOC y SIEM: arquitectura de detección

## 11.1 Qué es un SOC y qué problema resuelve

Un **Centro de Operaciones de Seguridad (SOC)** es el equipo —y la combinación de procesos y tecnología que lo sostiene— responsable de la **vigilancia continua** de los sistemas de una organización para detectar, analizar y responder a incidentes de seguridad. Es, en términos de la función Detectar de NIST CSF 2.0 (tema 1), la estructura organizativa que la hace operativa día a día: sin un SOC —propio o subcontratado— la función de detección se reduce, en la práctica, a descubrir los incidentes cuando ya han causado daño visible, casi siempre por un tercero que avisa.

## 11.2 La estructura por niveles (tiers)

La mayoría de los SOC maduros organizan a sus analistas en niveles de escalado progresivo, pensados para que la mayor parte del volumen de alertas se resuelva en el nivel más rápido y barato, y solo lo verdaderamente complejo llegue a los especialistas más costosos:

- **Nivel 1 (triaje):** revisa las alertas generadas por las herramientas de detección, descarta los falsos positivos evidentes, y escala al nivel 2 lo que requiere análisis más profundo. Es, por volumen, el nivel con más carga de trabajo repetitivo, y el que más se beneficia de la automatización (SOAR, más abajo).
- **Nivel 2 (investigación):** profundiza en los casos escalados, correlaciona múltiples fuentes de datos, determina el alcance real de un posible incidente y decide si procede activar el proceso de respuesta a incidentes (tema 12).
- **Nivel 3 (caza de amenazas y respuesta avanzada):** analistas senior que no esperan a que salte una alerta: buscan de forma proactiva indicios de compromiso que las reglas automatizadas no habrían detectado (*threat hunting*, retomado en el tema 13), y lideran la respuesta en los incidentes más graves o más ambiguos.

## 11.3 SIEM: el sistema nervioso central del SOC

Un **SIEM (Security Information and Event Management)** cumple dos funciones que, históricamente, eran productos separados y que el mercado terminó fusionando: **gestión de información de seguridad** (agregación y almacenamiento centralizado de logs de múltiples fuentes: firewalls, servidores, aplicaciones, controladores de dominio, soluciones de endpoint) y **gestión de eventos de seguridad** (correlación en tiempo casi real de esos eventos para generar alertas).

El valor añadido de un SIEM frente a revisar logs por sistema de forma aislada es la **correlación entre fuentes**: un intento de login fallido en un servidor concreto es, por sí solo, ruido habitual de cualquier red; cien intentos de login fallidos contra cien cuentas distintas en diez minutos, seguidos de un login correcto desde una IP que nunca se había visto antes para una de esas cuentas, es un patrón que ninguna fuente aislada revela por sí sola, pero que una correlación entre el log de autenticación y el histórico de geolocalización de accesos sí puede señalar automáticamente.

Diseñar **casos de uso de detección** concretos —reglas de correlación con un propósito definido ("alertar si una cuenta de servicio, que nunca debería autenticarse interactivamente, inicia sesión de forma interactiva")— suele apoyarse de forma directa en las técnicas catalogadas por MITRE ATT&CK (tema 4): un SOC maduro no escribe reglas genéricas de "algo raro ha pasado", escribe reglas que detectan técnicas adversarias concretas y documentadas, y usa el propio framework para medir qué porcentaje de la matriz tiene cubierto con detección real, no solo con intención.

## 11.4 SOAR: automatizar lo repetitivo para que el analista piense en lo que importa

**SOAR (Security Orchestration, Automation and Response)** complementa al SIEM automatizando las respuestas repetitivas y de bajo riesgo de decisión: ante una alerta de phishing confirmado, un *playbook* de SOAR puede automáticamente extraer los indicadores del correo (remitente, enlaces, adjuntos), consultarlos contra fuentes de inteligencia de amenazas, bloquear el remitente en la pasarela de correo, y buscar si algún otro usuario ha recibido el mismo correo — todo ello sin esperar a que un analista de nivel 1 ejecute manualmente cada uno de esos pasos uno por uno. El objetivo no es eliminar al analista humano, sino reservar su criterio para las decisiones que de verdad lo requieren, reduciendo el **tiempo medio de detección (MTTD)** y el **tiempo medio de respuesta (MTTR)** — las dos métricas de referencia más citadas para medir la eficacia operativa real de un SOC, por encima del volumen bruto de alertas procesadas.

## 11.5 El problema estructural: la fatiga de alertas

Ningún SIEM bien configurado desde el primer día deja de generar, inicialmente, un volumen de falsos positivos que puede saturar con facilidad a un equipo de analistas humanos — el fenómeno conocido como **alert fatigue**, documentado de forma consistente en estudios del sector como una de las causas principales de que incidentes reales pasen desapercibidos entre el ruido: no porque la herramienta no generara la alerta correcta, sino porque esa alerta se perdió entre cientos de alertas de baja calidad que el equipo había dejado de revisar con atención. Es la razón por la que el **ajuste continuo (tuning)** de las reglas de correlación —no la cantidad de reglas activas— es el indicador real de la madurez operativa de un SIEM, y por la que el SOAR, bien implementado, es tanto una herramienta de eficiencia como una herramienta directa de reducción de riesgo.

## Bibliografía

- SANS Institute. *SOC Survey* (series anuales sobre estructura, métricas y madurez de centros de operaciones de seguridad).
- MITRE. *ATT&CK Enterprise*, usado como base de cobertura de casos de uso de detección en SIEM.
- Gartner. Categorías de mercado *SIEM* y *SOAR*, definiciones y criterios de evaluación.
- NIST (2024). *Cybersecurity Framework 2.0*, función *Detect*.
