# <img alt="Hacking_Labs, más allá de la Ciberseguridad" src="llaves.png" width="4%">	Meterpreter 

> [!IMPORTANT]
> Laboratorio Meterpreter en <b>framework Metasploit</b> (Kali Linux 2023.4). Meterpreter (Meterpreter es el diminutivo para meta-interprete) es un payload que permite ejecutar tareas de forma remota en una máquina que se ejecuta en un nivel muy bajo de la máquina, por lo que es bastante difícil de detectar y, por tanto, muy útil para evadir sistemas antivirus, IDS o IPS. Gracias al payload Meterpreter, es posible realizar multitud de acciones sobre el sistema vulnerado, si bien no tiene porqué comportarse de la misma forma en todos los sistemas operativos. Este Payload que se ejecuta después del proceso de explotación o abuso de una vulnerabilidad en un sistema operativo, siempre en memoria, lo que evitará ser detectado.

> ### Trabajando con Meterpreter :computer:

- Paso 1: Ya dentro de Metasploit Framework, se utilizará el exploit multi handler, payload que se utiliza para conectar con el objetivo. Dependiendo del tipo de payload, el handler queda a la espera (está en modo escucha) de una conexión por parte del payload cargado en el objetivo (reverse payload), llegando a iniciar una conexión contra el host y puertos objetivo en ciertos casos (bind payload). En la consola, se codifica:
<b>

```
use exploit/multi/handler
```
</b>

