---
title: "03 - Identidad y control de acceso - MFA RBAC ABAC y Zero Trust"
---

# 3. Identidad y control de acceso: MFA, RBAC/ABAC y Zero Trust

## 3.1 Autenticación vs. autorización: dos preguntas que no son la misma

Es el error de vocabulario más persistente en seguridad IT, así que conviene fijarlo desde el principio:

- **Autenticación:** ¿eres quien dices ser?
- **Autorización:** ya sé quién eres, ¿qué tienes permitido hacer?

Un sistema puede autenticar perfectamente a un usuario legítimo y aun así fallar estrepitosamente si lo autoriza a hacer más de lo que su función requiere. De hecho, el **A01 — Broken Access Control** (control de acceso roto) es, según el OWASP Top 10 de 2021, la categoría de riesgo más grave documentada en aplicaciones web actuales (tema 8) — y casi siempre es un fallo de autorización, no de autenticación.

## 3.2 Autenticación: de la contraseña al MFA

La contraseña como único factor de autenticación es, desde hace años, un control que la industria considera insuficiente por sí solo: se reutiliza entre servicios, se filtra en brechas de terceros, y es vulnerable a *phishing* (tema 6). La respuesta estándar es la **autenticación multifactor (MFA)**, que exige combinar al menos dos de estas tres categorías de factores:

- **Algo que sabes:** una contraseña o PIN.
- **Algo que tienes:** un token físico, una aplicación de autenticación que genera códigos de un solo uso (TOTP), una llave de seguridad FIDO2/WebAuthn.
- **Algo que eres:** biometría (huella, reconocimiento facial).

No todos los segundos factores ofrecen la misma resistencia. Los códigos enviados por SMS, aunque mejor que nada, son vulnerables a ataques de **SIM swapping** e interceptación, y NIST los desaconseja como factor principal en escenarios de riesgo elevado. Las llaves de seguridad basadas en **FIDO2/WebAuthn** son, hoy, el estándar más resistente a *phishing*, precisamente porque la clave criptográfica queda ligada al origen (dominio) exacto de la petición de autenticación: un sitio de *phishing* con un dominio parecido pero distinto no puede completar el desafío criptográfico, aunque la víctima caiga en el engaño visual.

El caso de **Colonial Pipeline** (mayo de 2021) es el ejemplo de referencia de lo que falla cuando el MFA simplemente no está activado: el acceso inicial del grupo DarkSide se produjo a través de una cuenta VPN heredada, sin MFA, cuya contraseña había quedado expuesta en una filtración de credenciales previa y no relacionada — la cuenta ni siquiera estaba en uso activo, pero seguía siendo válida.

## 3.3 Autorización: RBAC, ABAC y el principio de mínimo privilegio

El **principio de mínimo privilegio** es la idea rectora de toda autorización bien diseñada: cada identidad —humana o de servicio— debe tener exactamente los permisos que necesita para su función, ni uno más. Dos modelos dominan la implementación práctica de ese principio:

- **RBAC (*Role-Based Access Control*):** los permisos se asignan a **roles** (por ejemplo, "analista de soporte", "administrador de base de datos"), y a los usuarios se les asignan roles, no permisos individuales. Es el modelo más extendido porque escala bien organizativamente: dar de alta a un empleado nuevo es "asignarle el rol X", no configurar permiso por permiso.
- **ABAC (*Attribute-Based Access Control*):** la decisión de autorización se calcula dinámicamente a partir de **atributos** del sujeto (departamento, nivel de autorización), del recurso (clasificación de la información) y del contexto (hora del día, ubicación, estado del dispositivo). Es más expresivo y granular que RBAC, pero también más complejo de auditar, porque la regla de acceso no está en una tabla fija de roles sino en una política evaluada en tiempo real.

Ninguno de los dos es intrínsecamente mejor: RBAC es más simple de gestionar y auditar a gran escala; ABAC permite políticas sensibles al contexto que RBAC no puede expresar de forma natural (por ejemplo, "permite el acceso solo si el dispositivo cumple la postura de seguridad exigida y la conexión viene de un país autorizado"). Muchas arquitecturas reales usan RBAC como capa base y añaden reglas ABAC encima para los casos de mayor sensibilidad.

## 3.4 Zero Trust: de "confía pero verifica" a "nunca confíes, verifica siempre"

El modelo de seguridad perimetral clásico —"todo lo que está dentro de la red corporativa es de confianza, todo lo de fuera no"— se construyó para un mundo de oficinas con un perímetro de red claro. Ese mundo prácticamente ha dejado de existir: empleados en remoto, dispositivos personales, aplicaciones SaaS fuera del perímetro, y —la lección que más costó aprender— atacantes que, una vez dentro del perímetro por cualquier vía, se movían lateralmente sin apenas fricción porque "estar dentro" ya se consideraba sinónimo de confianza.

**NIST SP 800-207** (agosto de 2020) formalizó la alternativa: la **Arquitectura Zero Trust**, cuyo principio rector es que **ninguna ubicación de red es intrínsecamente de confianza**; cada petición de acceso se evalúa de forma independiente, sin importar si el origen está "dentro" o "fuera" del perímetro tradicional. El documento define siete tenets fundamentales, de los que destacan tres por su impacto práctico directo:

1. Todo recurso de datos y de cómputo se trata como un recurso a proteger, con independencia de dónde esté.
2. El acceso se concede **por sesión**, no de forma permanente: la confianza no se "hereda" de una verificación anterior.
3. La autenticación y la autorización son **dinámicas** y se aplican estrictamente antes de conceder cualquier acceso, evaluando señales en tiempo real (postura del dispositivo, comportamiento, contexto).

Arquitectónicamente, NIST describe este modelo mediante dos componentes lógicos: el **Punto de Decisión de Política (PDP)**, que evalúa cada petición contra la política vigente, y el **Punto de Aplicación de Política (PEP)**, que materializa esa decisión permitiendo o bloqueando el acceso real. Es importante remarcar, como hacen varias fuentes técnicas, que Zero Trust **describe un resultado arquitectónico, no un producto concreto** que se pueda comprar e instalar: ninguna herramienta por sí sola "es" Zero Trust, y cualquier proveedor que lo presente como una casilla a marcar está simplificando en exceso.

## Bibliografía

- NIST (2020). *SP 800-207 — Zero Trust Architecture*. https://csrc.nist.gov/pubs/sp/800/207/final
- NIST (2017, con actualizaciones posteriores). *SP 800-63B — Digital Identity Guidelines: Authentication and Lifecycle Management* (recomendaciones sobre MFA y desaconsejo de SMS como factor único de riesgo elevado).
- FIDO Alliance. *FIDO2 / WebAuthn specifications*. https://fidoalliance.org
- OWASP (2021). *OWASP Top 10:2021 — A01 Broken Access Control*.
- Cobertura pública sobre el incidente de Colonial Pipeline (mayo de 2021), incluyendo el análisis del vector de acceso inicial vía cuenta VPN sin MFA.
