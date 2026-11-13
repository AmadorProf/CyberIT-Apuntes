---
title: "08 - Seguridad de aplicaciones web - OWASP Top 10 y SDLC seguro"
---

# 8. Seguridad de aplicaciones web: OWASP Top 10 y SDLC seguro

## 8.1 Por qué las aplicaciones web concentran tanto riesgo

Una aplicación web expone, casi por definición, una superficie de ataque accesible desde cualquier punto de Internet, procesando entrada de usuarios que, por diseño, no son de confianza. A diferencia de una red interna protegida por varias capas de perímetro, una aplicación web mal construida puede ser vulnerable con una sola petición HTTP malformada, sin necesidad de que el atacante tenga ningún acceso previo a la infraestructura. Es, por volumen de incidentes documentados, una de las categorías de riesgo más consistentemente alta del sector.

## 8.2 OWASP Top 10:2021

El **Open Worldwide Application Security Project (OWASP)** publica, de forma periódica, un catálogo de referencia de los riesgos más críticos en aplicaciones web, construido a partir de datos agregados de numerosas organizaciones. La edición de **2021** —la vigente de forma más extendida en la industria en el momento de escribir este tema, aunque conviene verificar en owasp.org si existe ya una edición posterior— es:

| # | Categoría | Idea central |
|---|---|---|
| A01 | Control de acceso roto | Un usuario autenticado accede a datos o funciones que no debería (el fallo de **autorización**, tema 3) |
| A02 | Fallos criptográficos | Datos sensibles sin cifrar, o cifrados con algoritmos débiles/obsoletos (tema 2) |
| A03 | Inyección | Entrada de usuario no validada que se interpreta como código o comandos (SQL, NoSQL, comandos de sistema) |
| A04 | Diseño inseguro | El riesgo nace en la arquitectura, no en un bug de implementación: faltan controles de seguridad desde el propio diseño |
| A05 | Configuración de seguridad incorrecta | Valores por defecto sin endurecer, servicios innecesarios activos, mensajes de error que filtran información interna |
| A06 | Componentes vulnerables y desactualizados | Librerías y dependencias de terceros con vulnerabilidades conocidas y sin parchear |
| A07 | Fallos de identificación y autenticación | Gestión de sesión débil, credenciales predecibles, ausencia de protección contra fuerza bruta |
| A08 | Fallos de integridad de software y datos | Pipelines de CI/CD o procesos de actualización sin verificación de integridad (la categoría donde encaja conceptualmente un ataque de cadena de suministro como el vector de NotPetya, tema 5) |
| A09 | Fallos de registro y monitorización de seguridad | Sin suficiente *logging*, un incidente puede pasar inadvertido durante meses — la causa más citada del tiempo medio de detección de brechas tan elevado en estudios del sector |
| A10 | Server-Side Request Forgery (SSRF) | El servidor, engañado, realiza peticiones a recursos internos que el atacante no podría alcanzar directamente desde fuera |

Es revelador que el riesgo número uno, de forma consistente, sea un fallo de **autorización** y no una vulnerabilidad técnica exótica: la inmensa mayoría de los incidentes de control de acceso roto son errores de lógica de negocio (comprobar el rol en el frontend pero no en el backend, confiar en un identificador de objeto manipulable en la URL) más que fallos criptográficos sofisticados.

## 8.3 Inyección: el ejemplo canónico

**A03 — Inyección**, y en particular la inyección SQL, sigue siendo el ejemplo pedagógico de referencia porque ilustra con total claridad el principio general de seguridad de aplicaciones: **nunca construir código ejecutable concatenando directamente entrada de usuario sin tratar**. Una consulta como `"SELECT * FROM usuarios WHERE nombre = '" + entrada + "'"` permite a un atacante que controla `entrada` alterar la estructura lógica de la consulta, no solo su contenido. La mitigación estándar —**consultas parametrizadas / sentencias preparadas**— separa de forma estricta el código de la consulta de los datos que la rellenan, de modo que la entrada del usuario nunca pueda reinterpretarse como sintaxis SQL, sin importar qué caracteres contenga.

## 8.4 SDLC seguro: mover la seguridad hacia la izquierda

El patrón histórico de "desarrollar primero, auditar seguridad justo antes de producción" es estructuralmente ineficiente: cuanto más tarde se detecta un fallo de seguridad en el ciclo de vida del software, más caro resulta corregirlo, tanto en coste de ingeniería como en riesgo de exposición si ya está en producción. El **Ciclo de Vida de Desarrollo Seguro de Software (SDLC seguro)** —popularizado el término *"shift left"* ("mover hacia la izquierda", hacia las fases más tempranas— integra actividades de seguridad en cada fase:

1. **Requisitos y diseño:** modelado de amenazas (identificar de forma estructurada qué puede salir mal antes de escribir una línea de código), requisitos de seguridad explícitos junto a los funcionales.
2. **Desarrollo:** análisis estático de código (SAST) integrado en el propio entorno del desarrollador o en el pipeline de integración continua, listas de dependencias con escaneo automático de vulnerabilidades conocidas (abordando directamente A06).
3. **Pruebas:** análisis dinámico (DAST) contra la aplicación en ejecución, pruebas de penetración antes de cada lanzamiento mayor.
4. **Despliegue:** verificación de integridad de artefactos y de la cadena de suministro del propio pipeline de CI/CD (abordando A08), configuración segura por defecto (A05).
5. **Operación:** monitorización y registro de eventos de seguridad en producción (A09), gestión continua de parches de dependencias.

Marcos de referencia como el **OWASP Software Assurance Maturity Model (SAMM)** o el histórico **Microsoft Security Development Lifecycle (SDL)** formalizan estas prácticas en modelos de madurez evaluables, permitiendo a una organización medir en qué fase de adopción se encuentra, en vez de tratar "seguridad en el desarrollo" como una aspiración difusa sin métrica.

## Bibliografía

- OWASP (2021). *OWASP Top 10:2021*. https://owasp.org/Top10/
- OWASP. *Software Assurance Maturity Model (SAMM)*. https://owaspsamm.org
- Microsoft. *Security Development Lifecycle (SDL)*. https://www.microsoft.com/en-us/securityengineering/sdl
- OWASP. *OWASP Application Security Verification Standard (ASVS)*, catálogo detallado de requisitos de verificación de seguridad.
