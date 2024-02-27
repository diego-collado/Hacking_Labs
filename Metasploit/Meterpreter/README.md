# <img alt="Hacking_Labs, más allá de la Ciberseguridad" src="llaves.png" width="4%">	Meterpreter 

> [!IMPORTANT]
> Laboratorio Meterpreter en <b>framework Metasploit</b> (Kali Linux 2023.4). Meterpreter (Meterpreter es el diminutivo para meta-interprete) es un payload que permite ejecutar tareas de forma remota en una máquina que se ejecuta en un nivel muy bajo de la máquina, por lo que es bastante difícil de detectar y, por tanto, muy útil para evadir sistemas antivirus, IDS o IPS. Gracias al payload Meterpreter, es posible realizar multitud de acciones sobre el sistema vulnerado, si bien no tiene porqué comportarse de la misma forma en todos los sistemas operativos. Este Payload que se ejecuta después del proceso de explotación o abuso de una vulnerabilidad en un sistema operativo, siempre en memoria, lo que evitará ser detectado.

> ### Trabajando con Meterpreter: MSFVenom y la generación de un Payload :computer:
> Con MSFVenom se simplifica la generación de payloads y los intentos de codificación de éstos.
> Entre las opciones más destacadas de MSFVenom, se encuentra:
- Payload, parámetro que especifica el payload que se utilizará.
- Encoder, otro parámetro que especifica el algoritmo que se utilizará para realizar la codificación.
- Format, donde se especifica el formato de salida del fichero (normalmente EXE, PY...)
- Bad-chars, que indica un listado de bytes que no se deben generar en el proceso de obtención del payload (como por ejemplo, si se quieren evitar los bytes nulos '\x00' se añaden en la lista de este parámetro).
- Iterations, que indica el número de iteraciones que se ejecutará el algoritmo del encoder (codificador)
- Template, que permite elegir la plantilla de ejecutable que se utilizará.
- Keep, que especifica que el payload se ejecutará en un thread (hilo) y no en el main del ejecutable, lo que produce un payload más sigiloso.

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

Si queremos disponer de un listado con los posibles payloads, podemos utilizar el siguiente código:
<b>

```
msfvenom -l payloads
```
</b>