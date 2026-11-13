---
title: "09 - Seguridad en la nube - responsabilidad compartida IAM y CSPM"
---

# 9. Seguridad en la nube: responsabilidad compartida, IAM y CSPM

## 9.1 El modelo de responsabilidad compartida

La confusión más costosa en seguridad en la nube es asumir que "está en la nube" significa "es responsabilidad del proveedor". Los tres grandes proveedores (AWS, Microsoft Azure, Google Cloud) formalizan, con matices propios pero un principio idéntico, el **modelo de responsabilidad compartida**: el proveedor es responsable de la seguridad **de** la nube (la infraestructura física, la virtualización, la red global), y el cliente es responsable de la seguridad **en** la nube (cómo configura esa infraestructura, quién tiene acceso, cómo protege sus propios datos y aplicaciones).

La frontera exacta de esa responsabilidad se desplaza según el modelo de servicio:

| Modelo | Qué gestiona el proveedor | Qué sigue gestionando el cliente |
|---|---|---|
| **IaaS** (Infraestructura como Servicio) | Hardware, virtualización, red física | Sistema operativo, parches, configuración de red lógica, aplicaciones, datos, identidad |
| **PaaS** (Plataforma como Servicio) | Lo anterior + sistema operativo y *runtime* | Configuración de la aplicación, datos, identidad, lógica de negocio |
| **SaaS** (Software como Servicio) | Prácticamente toda la pila técnica | Configuración de seguridad de la aplicación (permisos, políticas), gestión de identidad de usuarios, clasificación de los datos propios |

La mayoría de las brechas de seguridad en la nube documentadas no son fallos del proveedor: son fallos en la parte que le corresponde al cliente — casi siempre, configuración.

## 9.2 El caso de referencia: Capital One (2019)

La brecha de **Capital One**, divulgada en 2019, es el ejemplo pedagógico más citado de este principio. El vector de ataque combinó dos elementos: una vulnerabilidad de **Server-Side Request Forgery (SSRF)** —la misma categoría A10 del OWASP Top 10 vista en el tema 8— en una aplicación web mal configurada (en este caso, un firewall de aplicación web con permisos excesivos), y un **rol de IAM (Identity and Access Management) sobreprovisionado** en la infraestructura de AWS de la compañía. El atacante usó la SSRF para engañar al servidor y que éste realizara una petición al servicio de metadatos de instancia de AWS, obteniendo así credenciales temporales asociadas a un rol con permisos mucho más amplios de los que esa aplicación concreta necesitaba, y usó esas credenciales para acceder y exfiltrar datos de más de cien millones de clientes almacenados en buckets de almacenamiento en la nube. **Ningún control del proveedor de nube falló**: el servicio de metadatos y el modelo de IAM funcionaron exactamente como estaban diseñados y configurados — el fallo fue la combinación de una aplicación vulnerable con un principio de mínimo privilegio (tema 3) no aplicado al rol asociado a esa aplicación.

## 9.3 IAM en la nube: el perímetro ya no es la red, es la identidad

En un entorno cloud, donde los recursos se crean y destruyen de forma dinámica y pueden ser accesibles desde cualquier lugar, la gestión de identidad y acceso se convierte en el control de seguridad más determinante, por encima incluso del propio firewall de red. Principios que se repiten en las guías de seguridad de los tres grandes proveedores:

- **Mínimo privilegio por defecto**, con roles específicos para cada carga de trabajo en vez de credenciales de administrador reutilizadas ampliamente — exactamente el principio que faltó en Capital One.
- **Credenciales temporales y rotación automática** en vez de claves de acceso estáticas de larga duración, que si se filtran (por ejemplo, subidas accidentalmente a un repositorio de código público) siguen siendo válidas indefinidamente.
- **Separación de cuentas o proyectos** por entorno (desarrollo, pruebas, producción) y por unidad de negocio, para contener el radio de impacto de un compromiso.
- **MFA obligatorio para cuentas con privilegios elevados**, en particular la cuenta raíz/propietaria de la suscripción o el proyecto, cuyo compromiso equivale a perder el control total del entorno.

## 9.4 CSPM: encontrar la configuración incorrecta antes de que lo haga un atacante

Dada la velocidad con la que cambian los entornos cloud —nuevos recursos creados por equipos de desarrollo varias veces al día, en ocasiones sin pasar por una revisión de seguridad centralizada—, depender de auditorías manuales periódicas es, en la práctica, garantizar que la configuración real diverge de la política de seguridad declarada mucho antes de la siguiente revisión. La **Gestión de la Postura de Seguridad en la Nube (CSPM)** automatiza esa vigilancia: herramientas que inventarían de forma continua los recursos desplegados en uno o varios proveedores de nube, los comparan contra líneas base de buenas prácticas (por ejemplo, "ningún bucket de almacenamiento debe ser público salvo excepción explícita y documentada", "ningún grupo de seguridad debe permitir SSH abierto a cualquier IP de Internet") y generan alertas —o, en los despliegues más maduros, remediación automática— cuando detectan una desviación.

El valor de una herramienta CSPM no está solo en detectar errores evidentes, sino en dar visibilidad agregada y continua sobre un entorno que, por su propia naturaleza elástica, es casi imposible de auditar manualmente con la frecuencia que requeriría para ser eficaz.

## Bibliografía

- AWS, Microsoft Azure, Google Cloud. Documentación pública de cada proveedor sobre el *Shared Responsibility Model*.
- Cobertura pública y documentación judicial del caso Capital One (2019), incluyendo el análisis técnico del vector SSRF + rol de IAM sobreprovisionado.
- CISA (2023). *Advisory AA23-144A* y guías relacionadas sobre configuraciones incorrectas en la nube como vector de intrusión más citado.
- NIST SP 800-53 Rev. 5 — familia de controles de control de acceso (AC) y gestión de identidad aplicables a entornos cloud.
