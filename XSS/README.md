# <img alt="Hacking_Labs, más allá de la Ciberseguridad" src="images/xss.png" width="10%">	Ataque XSS con evasión de WAFs


> [!IMPORTANT]
> Laboratorio ataque XSS realizado en la distro Kali Linux 2023.4, gracias a la instalación de repositorios y otras herramientas. Un <b>ataque XSS</b> (<b>Cross-Site Scripting</b>) aprovecha un tipo de vulnerabilidad de seguridad en aplicaciones y sistemas web que permite a un atacante inyectar <b>scripts</b> maliciosos (JavaScript) en páginas determinadas muy visitadas por los usuarios ejecutando directamente el código en el propio navegador del usuario final víctima sin su consentimiento.

> Entre los tipos más reconocidos de XSS, podemos encontrar:
- <b>XSS reflejado (Reflected XSS)</b>: El script se envía a través de una solicitud (enlace o formulario) y se refleja directamente en la respuesta de la página.
Ejemplo: https://ejemplo.com/?q=<script>alert(1)</script>
- <b>XSS almacenado (Stored XSS)</b>: El código malicioso se guarda permanentemente en la base de datos de la aplicación y se muestra a los usuarios cada vez que cargan una página. Este tipo de códigos los podemos insertar en áreas muy determinadas como comentarios, foros, perfiles de usuario, etc.
- <b>XSS basado en DOM (DOM-based XSS)</b>: este ataque se produce cuando el navegador procesa contenido dinámico mediante JavaScript sin validación adecuada, manipulando directamente el DOM.

> Hecha la ley, hecha la trampa… Como sabemos desde hace tiempo, los sitios web están muy protegidos contra ataques de tipo inyección de código gracias a entornos e infraestructuras que proporcionan empresas como Cloudflare, Imperva y muchos más. La cuestión es que, como es común, un atacante va a <b>APROVECHAR NUESTROS FALLOS PARA ENTRAR EN EL SISTEMA, DONDE TAMBIÉN INCLUIMOS NUESTRA PEREZA POR PROGRAMAR DE FORMA SEGURA</b>.
> Técnicas tan sencillas como revisar lo que introduce el usuario, sanitizar código y muchas cosas más no se realizan por cientos de motivos diferentes (aunque principalmente suele ser el económico). Por tanto, y como podremos imaginar, existe múltiples técnicas utilizadas para evadir los cortafuegos de aplicaciones web (WAF) durante ataques de Cross-Site Scripting (XSS). Veamos qué significado tiene cada cosa y cómo podemos trabajar para mitigar este tipo de inyecciones.
<b>WAF = Web Application Firewall → Cortafuegos de aplicaciones web</b>

> Un <b>WAF</b> se pone entre el usuario y el servidor (o API) para inspeccionar el tráfico HTTP/HTTPS y poder detectar y/bloquear bloquear ataques dirigidos contra aplicaciones web. Pero… ¿De qué nos protege realmente un WAF? Pues puede llegar a defender nuestra infraestructura de ataques tipo inyecciones SQL (SQLi), Cross-Site Scripting (XSS), fuerza bruta, file inclusion (RFI/LFI), Cross-Site Request Forgery (CSRF), exposición de datos sensibles y comandos no autorizados (RCE)

> La misión del WAF es proteger las aplicaciones que un firewall tradicional no puede proteger, porque los firewalls de red operan en niveles más bajos (IP/TCP). Los WAFs se clasifican por su forma de instalación y operación. Veamos los modelos que existen:

| **TIPO DE WAF**        | **EXPLICACIÓN**                                                                                                                                                                                                                                     | **VENTAJAS Y DESVENTAJAS**                                                                                                                                                 | **EJEMPLOS**                                 |
|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------|
| **BASADO EN RED**  | - Se instala físicamente en la red<br> - Súper rápido y potente <br> - Instalado en el perímetro de la red (hardware o máquina virtual) <br> - Protege todo el tráfico (salida) <br> - Ideal para grandes empresas         | ✅ **Ventaja:** Bajísima latencia, control total <br> ❌ **Desventaja:** Costosos y más difícil de escalar                                                                  | F5 BIG-IP ASM, Imperva                       |
| **BASADO EN NUBE** | - Servicio online, no tienes que instalar nada, solo configurar <br> - No se instala nada físico <br> - Se usan servicios como *Cloudflare, AWS WAF, Akamai, Sucuri* <br> - Funciona como proxy inverso (redirecciona el DNS hacia ellos)          | ✅ **Ventaja:** Fácil de desplegar/mantener, escala automáticamente <br> ❌ **Desventaja:** Dependencia de terceros y configuración de políticas puede ser limitada         | Cloudflare WAF, AWS WAF, Akamai              |
| **BASADO EN HOST** | - Se instala en el mismo servidor donde corre la aplicación <br> - Instala directamente un software en el servidor (app web) <br> - Protege solo esa máquina específica                                                                         | ✅ **Ventaja:** Mucho control y personalización de reglas <br> ❌ **Desventaja:** Consume recursos del propio servidor (CPU/RAM)                                            | ModSecurity, NAXSI (para Nginx)              |

> Estos WAF suelen utilizar para la detección sistemas como el basado en firmas, el cual reconoce patrones de ataques conocidos, el basado en comportamiento, que detecta tráfico anómalo (por ejemplo, tráfico que rompe patrones normales), o también el basado en aprendizaje automático (los más modernos), que aprenden cómo es el tráfico normal de una aplicación.

> En realidad, ¿dónde tenemos que instalar el WAF? Veamos un pequeño esquema:

<p align="center">
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="/images/xss1.png">
  <source media="(prefers-color-scheme: light)" srcset="/images/xss1.png">
  <img alt="Hacking_Labs, más allá de la Ciberseguridad" src="/images/xss1.png" width="25%">
</picture>
</p>

> Ahora nos hacemos esta pregunta: <b>¿Qué hace un WAF moderno para evitar ser evadido?</b> Pues, la respuesta es fácil:
- Análisis de comportamiento: aprende cómo se ve un tráfico normal y detecta desvíos
- Normalización de solicitudes: antes de analizarlas, decodifica toda la entrada
- Machine learning: algunos WAFs como AWS WAF y Cloudflare usan IA para detectar nuevos patrones

> En realidad, son muchos los parámetros que afectan a la facilidad de evasión:
- 🔥 Configuración: Un WAF mal configurado es fácil de saltar, por muy caro que sea
- 🔥 Actualización: Si las firmas de ataques no se actualizan, los nuevos exploits pasan
- 🔥 Uso de Machine Learning: Los WAFs que "aprenden" el tráfico normal son mucho más difíciles de evadir
- 🔥 Normalización previa: Si el WAF decodifica correctamente el tráfico antes de filtrarlo, es muy difícil atacarlo

<h4 align="center">⚠️ ¡¡Si configuran reglas personalizadas, la evasión es más difícil!! ⚠️</h4>
