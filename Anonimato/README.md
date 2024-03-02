<p align="center">
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="dark-web.png">
  <source media="(prefers-color-scheme: light)" srcset="dark-web.png">
  <img alt="Hacking_Labs, más allá de la Ciberseguridad" src="dark-web.png" width="25%">
</picture>
</p>

# :desktop_computer:	Red TOR 

<p align="center">
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="tor.png">
  <source media="(prefers-color-scheme: light)" srcset="tor.png">
  <img alt="Hacking_Labs, más allá de la Ciberseguridad" src="tor.png" width="30%">
</picture>
</p>

> [!CAUTION]
> Laboratorios preparados para el entorno de la <b>red TOR</b> (Kali Linux 2023.4). Este laboratorio trata de realizar una redirección de todo el tráfico de red hacía la red TOR, o lo que es lo mismo, cualquier conexión del equipo que intente conectarse a Internet pasará por ella, evitando filtrar ping, forzando a las aplicaciones a derivar todo el tráfico por TOR (al contrario que ProxyChain, que es ignorado por algunas aplicaciones que tienden a una conexión más rápida ignorando los proxys). Asimismo, rechaza peticiones entrantes y salientes que puedan contener información sensible o pueda revelar la IP real y, por supuesto, realiza una protección de fuga de DNS, por lo que se puede utilizar un DNS remoto anónimo.

> TOR, sí... ¿Pero porqué?
The Onion Router... La conocida como <b>red TOR</b> es una de las mejores opciones a la hora de proteger la identidad en la red, dado que sus métodos de transferencia de datos están encriptados en varias capas, donde cada petición a un mismo servicio se envía por rutas diferentes utilizando múltiples nodos, lo que complica seguir una trazabilidad a un usuario o equipo dentro de una red. Su funcionamiento se basa en la red de servidores voluntarios.

> Ventajas de la utilización de la red TOR:
- Navegación Anónima: ocultación y cambio de IP ya que los nodos que TOR utiliza están encriptados bajo varias capas, además de tener una conexión de tipo HTTPS.
- Acceso a la Web Profunda (Deep Web): la parte de Internet que no está indexada por los motores de búsqueda convencionales y requiere acceso mediante direcciones URL específicas. 
- Acceso a la Dark Web: está oculta intencionalmente y requiere software especial para acceder a ella
- Privacidad en la Comunicación: TOR se utiliza a menudo para comunicaciones en línea privadas y seguras (correo electrónico y la mensajería instantánea, incluyendo WhatsApp y Telegram).
- Evitar la Censura en Internet: en aquellas regiones regiones del mundo donde (aún) existe una estricta censura en línea, ayudando a eludir filtros y restricciones impuestas por los gobiernos y otros actores. 
- Investigación y Periodismo: Los periodistas y activistas utilizan TOR para proteger su privacidad y seguridad mientras investigan temas sensibles o se comunican con fuentes anónimas.
- Protección contra la vigilancia: TOR encripta el tráfico de Internet y oculta la dirección IP del usuario, puede ayudar a proteger contra la vigilancia en línea realizada por gobiernos, proveedores de servicios de Internet y otros terceros.

> Deventajas de la utilización de la red TOR:
  - Bloqueo de scripts: algunos sitios web no funcionan, pueden cargar parcialmente o no cargar nada... Además, los mensajes de error no siempre aclaran las razones. En la mayor parte de los casos, el propio navegador de TOR bloquea ciertos scripts, donde se incluye muchos Javascript y scripts de seguimiento.
  - Lentitud: debido a que TOR rebota la conexión en diferentes nodos, este tipo de sistema hace que la conexión viaje más lejos y durante más tiempo de lo habitual ya que los nodos pueden estar muy lejos unos de otros. 
  - TOR es seguro, no invencible: como en la red indexada en los buscadores más conocidos, lo común y normal es que estén "pululando" diferentes tipos de malware, sitios falsos y miles de archivos de dudoso origen que podrían llegar a perjudicar gravemente a nuestros sistemas.
  - Contenido no deseado: Acceder a la Deep Web y a la Dark Web puede exponer a los usuarios a contenido peligroso o ilegal.

> El funcionamiento se basa en los siguientes elementos:

- <b>Tipo de comunicación</b>: la comunicación con la red Tor se establece mediante una ruta de comunicación más compleja que las conexiones normales a la hora de llegar al site al que se desea acceder, poniendo 3 nodos aleatorios de por medio.
- <b>Manejo de la comunicación</b>: la comunicación se maneja por capas, de modo que cada nodo solo tiene información de la capa inferior y superior a la propia, sin tener acceso a las capas de otros niveles (podrían ser el origen o el destino de la conexión), un tipo de arquitectura que dificulta que un monitoreo la conexión para poder llegar a identificar y localizar la conexión.




El tráfico en la Red Tor pasa a través de una serie de servidores, conocidos como relés. Hay tres tipos principales de relés en la Red Tor: relé de entrada (o guardia), relé intermedio y relé de salida.

Relé de Entrada: Este es el primer relé por el que pasa la información. Conoce la dirección IP del usuario, pero debido al cifrado de múltiples capas, no puede ver lo que el usuario está haciendo ni cuál es el destino final de los datos.
Relé Intermedio: Este relé actúa como un puente entre el relé de entrada y el relé de salida. No tiene información sobre el usuario ni sobre el destino final de los datos.
Relé de Salida: Este es el último relé en el proceso. Puede ver a qué destino se envían los datos, pero no sabe de dónde provienen.






Además, nuestros datos viajan cifrados mientras están en el circuito de la red Tor, así que ningún nodo puede saber la ruta completa de la conexión. Por ejemplo, en el dibujo de arriba:

El nodo de Francia solo sabe que recibe información de nuestra ubicación y que tiene que enviar la información al nodo de México. No sabe cuál es el contenido de la información.
El nodo de México sabe que recibe información desde Francia y que debe enviarla hacia Japón, pero no puede saber que originalmente estamos realizando la petición desde nuestra ubicación real.
A su vez, el nodo de Japón sabe que recibe información desde México y que debe enviarla al servidor que está en Estados Unidos, sin saber que antes la petición pasó por Francia y que se originó en nuestra ubicación.
Finalmente, el servidor en Estados Unidos simplemente recibe una solicitud de información desde Japón, ignorando completamente el circuito que se realizó para completar esta solicitud. Solo el nodo final -o de salida- sabe hacia dónde se dirige la solicitud de información.
Y al final del proceso solo quien hace la solicitud puede saber cuáles son todos los nodos del circuito que está utilizando. Esto aumenta drásticamente la seguridad y el anonimato en nuestras conexiones.

Existen algunas maneras de utilizar la red Tor que igual pueden revelar quienes somos al servicio que queremos acceder. Si te interesa saber más sobre esto puedes ver la Parte 4: Modelo de amenazas y consideraciones legales.

https://tor.derechosdigitales.org/torificate/p4/
--------------------------------
- <b>Interfaces</b>, es decir, las diferentes plataformas través de las cuales los usuarios pueden acceder a Metasploit Framework:
  - MSFConsole (Metasploit Framework Console): la interfaz Metasploit más utilizada, la consola Metasploit permite a los usuarios acceder a Metasploit Framework a través de una interfaz de línea de comandos interactiva.
  - MSFWeb: una interfaz basada en navegador que permite a los usuarios acceder al marco de Metasploit.
  - Armitage: desarrollado por Raphael Mudge en 2013, Armitage es una interfaz gráfica de usuario basada en Java que permite a los equipos de seguridad colaborar compartiendo su acceso a hosts comprometidos. 
  - RPC (llamada a procedimiento remoto): permite a los usuarios manejar mediante programación Metasploit Framework utilizando servicios de llamada a procedimiento remoto (RPC) basados ​​en HTTP. Además del Ruby nativo de Metasploit, los servicios RPC pueden operar a través de otros lenguajes, como Java, Python y C.

- <b>Bibliotecas</b>, que contienen las diferentes funciones de Metasploit Framework que permiten a los usuarios ejecutar exploits sin escribir código adicional:
  - REX: habilita las tareas más básicas; contiene Base64, HTTP, SMB, SSL y Unicode.
  - MSF Core: proporciona una API común y define Metasploit Framework.
  - Base de MSF: proporciona una API fácil de usar.

- <b>Módulos</b>, que se utilizan para realizar tareas como escaneos y explotación de objetivos:
  - Cargas útiles: las cargas útiles son códigos de shell que realizan las acciones previstas por el usuario una vez que un exploit ha comprometido un sistema objetivo. Se pueden usar para abrir Meterpreters o comandos de shells. Los Meterpreters son cargas útiles sofisticadas que se utilizan durante un ciberataque para ejecutar código y realizar más tareas exploratorias.
  - Exploits: ejecuta secuencias de comandos para aprovechar las debilidades del sistema o de la aplicación y obtener acceso a los sistemas de destino.
  - Publicaciones (módulos posteriores a la explotación): las publicaciones permiten a los usuarios realizar una recopilación de información más profunda e infiltrarse aún más en un sistema de destino después de la explotación. Por ejemplo, las publicaciones se pueden utilizar para realizar la enumeración de servicios .
  - Codificadores: los codificadores ocultan las cargas útiles en tránsito para garantizar que se entreguen correctamente al sistema de destino y eviten la detección del software antivirus, los sistemas de detección de intrusiones (IDS) y los sistemas de prevención de intrusiones (IPS).
  - NOP (No Operation): los generadores NOP crean secuencias aleatorias de bytes para evitar los sistemas de detección y prevención de intrusiones.
  - Auxiliares: los módulos auxiliares incluyen escaneo de vulnerabilidades, escaneo de puertos, fuzzers, sniffers y otras herramientas de explotación.

- <b>Complementos</b>, los cuales amplian la funcionalidad de todo el framework

<br>
<br>

# <img alt="Hacking_Labs, más allá de la Ciberseguridad" src="previo.png" width="4%"> Antes de todo... 

> [!IMPORTANT]
> Antes de arrancar el framework Metasploit, se hace necesario seguir una serie de pasos imprescindibles para que el sistema esté completamente preparado para la realización correcta del laboratorio.

> ### Pre-requisitos 📋
Paso 1: Iniciar del servicio de base de datos <b>PostgreSQL</b>, ya que Metasploit Framework trabaja con un base de datos de este tipo para el almacenamiento de la información:
<b>
```
systemctl start postgresql
```
</b>

Paso 2: <b>Solamente la 1primera vez que se vaya a iniciar Metasploit</b> debemos inicializar a 0 la base de datos:
<b>
```
msfdb init
```
</b>

Paso 3: Se arranca el framework:
<b>
```
msfconsole
```
</b>

También se puede utilizar el arranque rápido (sin logotipos):
<b>
```
msfconsole -q
```
</b>

Nuestra pantalla deberá ser similar a esta:

<p align="center">
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="metasploit_1.png">
  <source media="(prefers-color-scheme: light)" srcset="metasploit_1.png">
  <img alt="Hacking_Labs, más allá de la Ciberseguridad" src="metasploit_1.png" width="50%">
</picture>
</p>


> ### Ejecutando las pruebas previas en busca de información: <b>INFORMATION GATHERING</b> ⚙️

Antes de realizar un ataque, y para que este sea óptimo, es necesario el escaneo de la dirección IP del objetivo, donde se podrá adquirir información valiosa como servicios levantados, puertos abiertos, sistemas operativos y mucho más. Para ello, se ejecuta:

```
nmap -sS -sV -A [IP_del_objetivo]
```
Veamos el porqué de esta codificación:

- <b>-sS</b>: sondeo TCP SYN, utilizado por omisión para el sondeo de miles de puertos por segundo en una red rápida en la que no existan cortafuegos. Este sondeo SYN es relativamente sigiloso y poco molesto, ya que no llega a completar las conexiones TCP, por lo que también funciona contra cualquier pila TCP en lugar de depender de una plataforma concreta. Su funcionamiento se basa en la casi apertura de una conexión TCP completa, ya que se envía un paquete SYN, como si se fuera a abrir una conexión real y después se espera una respuesta. Si se recibe un paquete SYN/ACK esto indica que el puerto está en escucha (abierto), mientras que si se recibe un RST (reset) indica que no hay nada escuchando en el puerto. Si no se recibe ninguna respuesta después de realizar algunas retransmisiones entonces el puerto se marca como filtrado. 
- <b>-sV</b>: Detección de versiones, activa la detección de versiones de los servicios. 
- <b>-A</b>: Se activa tanto la detección de versiones como la detección de sistema operativo.

<p align="center">
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="metasploit_2.png">
  <source media="(prefers-color-scheme: light)" srcset="metasploit_2.png">
  <img alt="Hacking_Labs, más allá de la Ciberseguridad" src="metasploit_2.png" width="50%">
</picture>
</p>

<br>

## :bricks:	CheatSheets de utilidad	:books:
- [CheatSheet](CheatSheet): CheatSheet (hoja de trucos) para <b>Metasploit</b> y <b>Nmap</b>.


<br>
<br>

# <img alt="Hacking_Labs, más allá de la Ciberseguridad" src="fuego.png" width="4%"> Accede a los laboratorios  :floppy_disk:

- [LABORATORIO I](Meterpreter):  - Creación, instalación y manejo de Meterpreter (Metasploit + MSFVenom)