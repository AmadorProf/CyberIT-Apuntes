---
title: "06 - Ingenieria social y phishing"
---

# 6. Ingeniería social y phishing

## 6.1 Por qué el eslabón humano sigue siendo el más explotado

Todos los controles técnicos de los temas anteriores —criptografía, MFA, Zero Trust— comparten una misma asunción implícita: que la persona al otro lado de la pantalla toma, en general, decisiones racionales con la información que tiene delante. La **ingeniería social** es la categoría de ataque que explota precisamente el punto donde esa asunción falla: no rompe el cifrado ni explota una vulnerabilidad de software, convence a una persona de que haga algo —hacer clic, revelar una contraseña, autorizar una transferencia— que de otro modo no haría.

Esto no es un indicador de que "los usuarios son el problema": es un indicador de que el cerebro humano usa, de forma constante y en general adaptativa, atajos de decisión (heurísticas) que un atacante entrenado puede activar deliberadamente. Los principios de influencia descritos por el psicólogo Robert Cialdini —autoridad, urgencia/escasez, prueba social, simpatía, compromiso y reciprocidad— son, en la práctica, el manual de instrucciones que sigue de forma más o menos consciente cualquier campaña de *phishing* eficaz.

## 6.2 Taxonomía de phishing

- **Phishing masivo:** el correo genérico ("su cuenta ha sido suspendida") enviado a miles de destinatarios, con una tasa de éxito baja por mensaje pero rentable por volumen.
- **Spear phishing:** dirigido a una persona o grupo concreto, con información personalizada (nombre, cargo, proyecto en curso) que aumenta drásticamente la credibilidad.
- **Whaling:** spear phishing dirigido específicamente a altos cargos (*C-level*), donde el impacto potencial de un solo clic es proporcionalmente mayor.
- **Vishing:** la variante por voz/teléfono. El ataque a Twitter de julio de 2020 —en el que atacantes obtuvieron acceso a herramientas administrativas internas para secuestrar cuentas verificadas de alto perfil mediante una campaña de estafa de criptomonedas— se ejecutó, según la investigación pública posterior, mediante **vishing dirigido a empleados** con acceso a esas herramientas, no mediante explotación de una vulnerabilidad técnica.
- **Smishing:** la variante por SMS, en auge con la proliferación de alertas falsas de paquetería o de banca.
- **Business Email Compromise (BEC) / fraude del CEO:** quizás la categoría con mayor impacto económico agregado. El atacante se hace pasar —a menudo mediante un dominio de correo casi idéntico al legítimo, o mediante una cuenta de correo corporativa ya comprometida— por un directivo o un proveedor habitual, e instruye una transferencia bancaria urgente y confidencial. El caso de **Evaldas Rimasauskas**, que entre 2013 y 2015 engañó a Google y Facebook para que transfirieran más de 100 millones de dólares haciéndose pasar por un proveedor de hardware legítimo mediante facturas e identidades falsificadas, es uno de los ejemplos de BEC con mayor cuantía documentada judicialmente.

## 6.3 Por qué el BEC es, en volumen económico, más grave que el ransomware

Es una afirmación que suele sorprender fuera del sector: según los informes anuales del **Internet Crime Complaint Center (IC3)** del FBI, las pérdidas acumuladas reportadas por BEC superan sistemáticamente, año tras año, a las reportadas por ransomware — con una diferencia de varios miles de millones de dólares en algunos ejercicios. La razón estructural es que el BEC no necesita vulnerar ningún sistema técnico: necesita, únicamente, que una persona con autoridad para mover dinero crea una historia convincente en el momento equivocado. Ninguna cantidad de cifrado o de parches corrige ese punto de fallo; solo lo corrigen **procesos de verificación fuera de banda** (confirmar cualquier cambio de cuenta bancaria o transferencia inusual por un canal distinto al del correo que la solicita, idealmente por teléfono a un número ya conocido, no al que aparece en el propio correo sospechoso).

## 6.4 Por qué la formación por sí sola no basta (y qué sí ayuda)

La respuesta instintiva a la ingeniería social —"hay que formar a los empleados"— es necesaria pero insuficiente si se queda en una charla anual. Lo que la evidencia del sector respalda con más consistencia:

1. **Simulacros de phishing periódicos y realistas**, con retroalimentación inmediata a quien hace clic (no punitiva, formativa), para mantener la alerta fresca en vez de depender de un recuerdo de hace un año.
2. **Controles técnicos que no dependen de que la persona acierte siempre:** autenticación de correo (SPF, DKIM, DMARC) para dificultar la suplantación de dominio; filtrado de enlaces y adjuntos en el perímetro de correo; y, sobre todo, **MFA resistente a phishing** (FIDO2/WebAuthn, tema 3) para que, aunque la contraseña se revele, no baste por sí sola para completar el acceso.
3. **Procesos de verificación fuera de banda obligatorios** para cualquier operación de alto impacto (transferencias, cambios de cuenta bancaria, restablecimiento de credenciales de cuentas privilegiadas) — convertir la verificación en un paso de proceso, no en una decisión individual bajo presión de tiempo.

El denominador común de estas tres capas es que ninguna asume que la persona nunca cometerá un error: asumen que lo cometerá, en algún momento, y diseñan el sistema para que ese error individual no sea, por sí solo, suficiente para causar daño.

## Bibliografía

- Cialdini, R. B. (2006). *Influence: The Psychology of Persuasion* (edición revisada). Harper Business.
- FBI — Internet Crime Complaint Center (IC3). *Internet Crime Report* (informes anuales, series temporales de pérdidas por BEC vs. ransomware). https://www.ic3.gov
- U.S. Department of Justice. Documentación judicial del caso *United States v. Evaldas Rimasauskas* (fraude BEC contra Google y Facebook).
- Cobertura pública de la investigación sobre el incidente de Twitter de julio de 2020 (vishing dirigido a empleados con acceso administrativo).
- M³AAWG / IETF. *SPF (RFC 7208), DKIM (RFC 6376), DMARC (RFC 7489)* — mecanismos de autenticación de correo contra la suplantación de dominio.
