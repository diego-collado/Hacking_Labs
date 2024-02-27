<p align="center">
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="hacked.png">
  <source media="(prefers-color-scheme: light)" srcset="hacked.png">
  <img alt="Hacking_Labs, más allá de la Ciberseguridad" src="hacked.png" width="25%">
</picture>
</p>

# :desktop_computer:	Metasploit 

> [!CAUTION]
> Laboratorios preparados con el <b>framework Metasploit</b> (<u>Kali Linux 2023.4</u>). En este laboratorio se trata de comprometer los sistemas utilizando cualquiera de sus más de 1600 exploits que, de una forma u otra, tratarán de aprovechar las vulnerabilidades que se hayan encontrado en las fases previas de analítica y OSINT.
Es un software de código abierto, originalmente escrito en Perl y posteriormente traducido a Ruby para una mayor eficiencia. Está muy enfocada a  auditores de seguridad y equipos Red Team y Blue Team.

> Metasploit incluye:
- Cargas útiles del shell de comandos que permiten a los usuarios ejecutar scripts o comandos aleatorios en un host.
- Dinámicas que permiten a los evaluadores generar cargas útiles únicas para evadir el software antivirus.
- Cargas útiles de Meterpreter que permiten a los usuarios controlar los monitores de dispositivos mediante VMC y hacerse cargo de las sesiones o cargar y descargar archivos.
- Estáticas que permiten el reenvío de puertos y las comunicaciones entre redes.

>Las funciones de Metasploit más destacadas son: 
  - <b>Escaneo y recopilación de información</b>: Utilizando herramientas como Nmap, Metasploit realiza una recolección exhaustiva de datos sobre el objetivo del ataque.
  - <b>Identificación y exploración de vulnerabilidades</b>: La plataforma detecta vulnerabilidades conocidas en sistemas, analizando el sistema Common Vulnerabilities and Exposures (CVE) para encontrar los exploits correspondientes.
  - <b>Escalada de privilegios</b>: Metasploit incorpora herramientas para conseguir privilegios de administrador en diversos sistemas operativos (Windows, Android, Linux...), lenguajes de programación y, por supuesto, sistema propios de Cisco y mucho más.
  - <b>Instalación de backdoors</b>: A través de su módulo de payloads, Metasploit permite la instalación de backdoors (puertas traseras) en el sistema objetivo para la extracción de información confidencial, cifrado y un largo etcétera.
  - <b>Realización de fuzzing</b>: Automatizando el ingreso de valores aleatorios, Metasploit busca activamente fallos que posibiliten la infiltración en dispositivos o redes.
  - <b>Evasión de antivirus, IDS/IPS</b>: La plataforma incluye herramientas para la ofuscación de código, reescribiéndolo de manera que se vuelva indetectable para los sistemas de defensa.
  - <b>Eliminación de rastros</b>: Metasploit ofrece métodos para borrar la huella digital del atacante, eliminando logs y archivos maliciosos utilizados durante el hackeo.

> El funcionamiento se basa en las siguientes partes:
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

> [!IMPORTANT]
> Antes de arrancar el framework Metasploit, se hace necesario seguir una serie de pasos imprescindibles para que el sistema esté completamente preparado para la realización correcta del laboratorio.

### Pre-requisitos 📋
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
O también se puede utilizar el arranque rápido (sin logotipos):
<b>
```
msfconsole -q
```
</b>

Nuestra pantalla deberá ser similar a esta:

<p align="center">
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="metasploit_1.jpg">
  <source media="(prefers-color-scheme: light)" srcset="metasploit_1.jpg">
  <img alt="Hacking_Labs, más allá de la Ciberseguridad" src="metasploit_1.jpg" width="50%">
</picture>
</p>


<img alt="Hacking_Labs, más allá de la Ciberseguridad" src="metasploit_1.jpg" width="50%">




<br>
<br>
## <img alt="Hacking_Labs, más allá de la Ciberseguridad" src="fuego.png" width="4%"> Accede a los laboratorios  :floppy_disk:

- [Meterpreter](Meterpreter): <b>LABORATORIO Nº 1</b> - Creación, instalación y manejo de Meterpreter (Metasploit + MSFVenom)

