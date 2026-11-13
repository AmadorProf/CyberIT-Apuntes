---
title: "02 - Criptografia aplicada - cifrado simetrico asimetrico hashing PKI y TLS"
---

# 2. Criptografía aplicada: cifrado simétrico, asimétrico, hashing, PKI y TLS

## 2.1 Tres primitivas, tres problemas distintos

La criptografía moderna aplicada a seguridad IT se apoya en tres primitivas que conviene no confundir, porque cada una resuelve un problema distinto:

- **Cifrado simétrico:** la misma clave cifra y descifra. Es rápido y eficiente para grandes volúmenes de datos (el estándar de facto es **AES**, *Advanced Encryption Standard*, con claves de 128 o 256 bits). Su problema no es la fuerza del algoritmo, sino la **distribución de la clave**: si dos partes necesitan la misma clave secreta, ¿cómo se la intercambian sin que un tercero la intercepte?
- **Cifrado asimétrico:** un par de claves matemáticamente relacionadas, una pública (se puede compartir libremente) y una privada (nunca sale de quien la posee). Lo que se cifra con la pública solo se descifra con la privada correspondiente, y viceversa para la firma digital. Resuelve exactamente el problema de distribución de clave que el cifrado simétrico no resuelve, a costa de ser mucho más lento computacionalmente. Los algoritmos de referencia son **RSA** (basado en la dificultad de factorizar números muy grandes) y, cada vez más, criptografía de **curva elíptica (ECC)**, que ofrece seguridad equivalente con claves mucho más cortas y, por tanto, operaciones más rápidas.
- **Funciones hash criptográficas:** funciones de una sola vía que convierten una entrada de cualquier tamaño en una salida de tamaño fijo (un "resumen" o *digest*), de forma que es computacionalmente inviable encontrar dos entradas distintas con el mismo resumen (resistencia a colisiones) o reconstruir la entrada a partir de la salida. **SHA-256** es el estándar de referencia actual; **MD5** y **SHA-1**, pese a seguir viéndose en sistemas antiguos, están criptográficamente rotos para fines de seguridad y no deberían usarse en nada nuevo.

## 2.2 Por qué casi todo usa los tres a la vez: el modelo híbrido

En la práctica, prácticamente ningún sistema real usa una sola de estas primitivas en solitario. El patrón dominante —y el que sostiene TLS, descrito más abajo— es el **cifrado híbrido**: se usa cifrado asimétrico, lento pero sin problema de distribución de clave, únicamente para **intercambiar de forma segura una clave simétrica de sesión**; a partir de ahí, todo el tráfico de datos real se cifra con esa clave simétrica, rápida pero ya distribuida de forma segura. Es la combinación que da lo mejor de ambos mundos: la seguridad de distribución del cifrado asimétrico y la velocidad del simétrico para el volumen real de datos.

El hashing, por su parte, no cifra nada: se usa para **verificar integridad** (comprobar que un fichero descargado no se ha alterado comparando su hash con el publicado por la fuente) y, combinado con una clave secreta, para construir **firmas digitales y códigos de autenticación de mensajes (HMAC)**, que certifican tanto la integridad como el origen de un mensaje.

## 2.3 PKI: quién garantiza que una clave pública es de quien dice ser

El cifrado asimétrico resuelve la distribución de claves, pero introduce un problema nuevo: si cualquiera puede generar un par de claves, ¿cómo sé que la clave pública que tengo delante pertenece realmente a "banco.com" y no a un atacante que se hace pasar por "banco.com"? La respuesta es la **Infraestructura de Clave Pública (PKI)**: un sistema de confianza jerárquico en el que **Autoridades de Certificación (CA)** —entidades de confianza previamente aceptadas por los sistemas operativos y navegadores— emiten **certificados digitales** que vinculan criptográficamente una clave pública con una identidad (un dominio, una organización, una persona), firmando ese vínculo con su propia clave privada.

Cuando un navegador valida el certificado de un sitio web, en realidad está verificando una **cadena de confianza**: el certificado del sitio está firmado por una CA intermedia, cuyo certificado a su vez está firmado por una CA raíz que el sistema operativo o el navegador ya incluye como punto de confianza de partida. Si cualquier eslabón de esa cadena falla —un certificado caducado, revocado, o firmado por una CA no reconocida— la confianza se rompe y el navegador debería alertarlo, no ignorarlo.

## 2.4 TLS: las tres primitivas trabajando juntas

**TLS** (*Transport Layer Security*, sucesor de SSL) es el protocolo que protege la inmensa mayoría del tráfico web moderno (el "candado" del navegador) y es, en sí mismo, el mejor ejemplo pedagógico de cómo encajan las tres primitivas anteriores en un sistema real:

1. **Protocolo de negociación (*handshake*):** el cliente y el servidor se ponen de acuerdo en qué versión de TLS y qué conjunto de algoritmos (*cipher suite*) van a usar.
2. **Autenticación mediante PKI:** el servidor presenta su certificado digital; el cliente valida la cadena de confianza descrita arriba para confirmar que está hablando con quien cree que está hablando (y, opcionalmente, en TLS mutuo, el servidor valida también un certificado de cliente).
3. **Intercambio de clave mediante criptografía asimétrica** (hoy, casi universalmente, mediante variantes de Diffie-Hellman de curva elíptica efímero, que además aportan **confidencialidad directa perfecta**: aunque la clave privada del servidor se comprometa en el futuro, el tráfico pasado capturado sigue sin poder descifrarse).
4. **Cifrado simétrico de los datos** de la sesión con la clave acordada, típicamente con AES o ChaCha20.
5. **Verificación de integridad** de cada registro de datos mediante códigos de autenticación de mensajes, para detectar manipulación en tránsito.

Las versiones vigentes en producción hoy son **TLS 1.2** y, de forma creciente, **TLS 1.3** (2018), que simplifica el conjunto de algoritmos permitidos, elimina opciones criptográficamente débiles heredadas de versiones anteriores, y reduce el número de idas y vueltas necesarias para completar el *handshake*, lo que lo hace tanto más seguro como más rápido que su predecesor.

## 2.5 El horizonte: criptografía post-cuántica

Un ordenador cuántico suficientemente potente, ejecutando el algoritmo de Shor, rompería en tiempo práctico tanto RSA como la criptografía de curva elíptica — las dos bases del cifrado asimétrico actual. Aunque esa capacidad no existe todavía a escala operativa, el riesgo de **"capturar ahora, descifrar después"** (un adversario que almacena hoy tráfico cifrado para descifrarlo cuando tenga capacidad cuántica) ha llevado a NIST a estandarizar ya varios algoritmos de criptografía post-cuántica (como ML-KEM, antes conocido como CRYSTALS-Kyber), que empiezan a desplegarse de forma híbrida —combinados con los algoritmos clásicos, no sustituyéndolos todavía— en algunos navegadores y servicios en la nube.

## Bibliografía

- NIST. *FIPS 197 — Advanced Encryption Standard (AES)*.
- NIST. *FIPS 180-4 — Secure Hash Standard (SHS)*, base de la familia SHA-2 (SHA-256).
- IETF. *RFC 8446 — The Transport Layer Security (TLS) Protocol Version 1.3*.
- NIST (2024). *FIPS 203 — Module-Lattice-Based Key-Encapsulation Mechanism Standard (ML-KEM)*, estandarización de criptografía post-cuántica.
- Ferguson, N., Schneier, B. y Kohno, T. (2010). *Cryptography Engineering: Design Principles and Practical Applications*. Wiley.
