---
title: "07 - Seguridad de redes - firewalls segmentacion VPN IDS-IPS"
---

# 7. Seguridad de redes: firewalls, segmentación, VPN, IDS/IPS

## 7.1 Firewalls: de filtrar puertos a entender aplicaciones

Un **firewall de estado (*stateful*)** de primera generación decide si permite o bloquea tráfico en función de direcciones IP, puertos y el estado de la conexión (si forma parte de una sesión ya establecida o es un intento nuevo). Es un control necesario pero, frente a las amenazas actuales, claramente insuficiente por sí solo: dos aplicaciones completamente distintas, una legítima y otra maliciosa, pueden perfectamente viajar por el mismo puerto 443.

Los **firewalls de nueva generación (NGFW)** añaden **inspección a nivel de aplicación**: identifican qué aplicación concreta genera el tráfico (no solo el puerto), pueden descifrar e inspeccionar TLS de forma controlada (*TLS inspection*, con sus propias implicaciones de privacidad y confianza que conviene gestionar con política explícita), e incorporan funciones de prevención de intrusiones y filtrado de contenido directamente integradas. Es, en esencia, la industria llevando al perímetro IT corporativo la misma filosofía de "entender el protocolo, no solo verlo como bytes" que, en el dominio OT, motiva los firewalls industriales con inspección de Modbus o DNP3.

## 7.2 Segmentación: el perímetro ya no es suficiente

El modelo de "perímetro duro, interior blando" —todo lo que cruza el firewall de entrada se considera después de confianza— es precisamente el que Zero Trust (tema 3) vino a desmontar. La **segmentación de red** es su materialización técnica: dividir la red interna en zonas más pequeñas, con control de tráfico entre ellas, para que el compromiso de un segmento no otorgue automáticamente acceso lateral a todos los demás.

La evolución más reciente de este principio es la **microsegmentación**: en vez de segmentar por subredes físicas o VLAN (una granularidad relativamente gruesa), se aplican políticas de acceso a nivel de carga de trabajo individual o de aplicación, a menudo mediante agentes software o mediante las propias capacidades de redes definidas por software de la infraestructura virtualizada o en la nube. El objetivo final es el mismo que el de las zonas y conductos del dominio OT (visto en el curso paralelo): limitar de forma explícita y verificable qué puede hablar con qué, en vez de asumir que "estar en la misma red" ya implica una relación de confianza.

## 7.3 VPN: cifrar el canal, no sustituir la autorización

Una **VPN (Red Privada Virtual)** cifra el tráfico entre dos puntos sobre una red no confiable (típicamente Internet), extendiendo de forma lógica una red privada. Es importante no confundir "cifrar el canal" con "autorizar correctamente lo que viaja por él": una VPN con una cuenta sin MFA —como en el caso de Colonial Pipeline (tema 5)— sigue siendo una puerta de entrada válida para un atacante que ya tiene la contraseña, por mucho que el canal esté cifrado de punta a punta.

Dos paradigmas conviven hoy:

- **VPN de acceso remoto tradicional (IPsec o SSL/TLS VPN):** el usuario remoto obtiene, en la práctica, una posición de red equivalente a estar físicamente en la oficina, lo que reintroduce exactamente el problema de "confianza por ubicación" que Zero Trust intenta eliminar si no se combina con controles de autorización granular adicionales.
- **Acceso a la red Zero Trust (ZTNA):** en vez de conceder acceso a la red completa, se concede acceso aplicación por aplicación, verificando identidad, postura del dispositivo y contexto en cada petición — alineado directamente con los tenets de NIST SP 800-207 vistos en el tema 3, y cada vez más ofrecido como sustituto directo de la VPN tradicional en arquitecturas modernas.

## 7.4 IDS/IPS: detectar frente a bloquear en línea

Un **Sistema de Detección de Intrusos (IDS)** observa tráfico (normalmente una copia, vía puerto espejo, igual que en la monitorización pasiva OT del curso paralelo) y genera alertas cuando identifica patrones sospechosos, sin interferir con el tráfico real. Un **Sistema de Prevención de Intrusiones (IPS)** hace lo mismo pero **en línea**, con capacidad de bloquear activamente el tráfico malicioso identificado — a cambio de introducir el propio IPS como un punto único de fallo y de latencia en la ruta de red, un coste que un IDS puramente pasivo no asume.

Ambos se apoyan en dos enfoques de detección, a menudo combinados:

- **Basado en firmas:** compara el tráfico contra patrones conocidos de ataques documentados. Es preciso y con pocos falsos positivos para amenazas ya catalogadas, pero ciego por definición ante algo nuevo que no tiene firma todavía.
- **Basado en anomalías:** establece una línea base de comportamiento "normal" y alerta ante desviaciones significativas. Puede detectar amenazas desconocidas, pero a costa de una tasa de falsos positivos más alta que exige ajuste (*tuning*) continuo para ser operativamente útil sin saturar al equipo de análisis.

## Bibliografía

- NIST (2023). *SP 800-82 Rev. 3*, secciones de arquitectura de red aplicables también a IT convencional.
- NIST (2020). *SP 800-207 — Zero Trust Architecture*.
- Cisco / Palo Alto Networks / Fortinet. Documentación técnica pública sobre arquitecturas NGFW e IPS.
- Gartner. Categoría de mercado *ZTNA (Zero Trust Network Access)* como sucesor funcional de la VPN de acceso remoto tradicional.
