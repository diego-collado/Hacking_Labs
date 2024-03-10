# <img alt="Hacking_Labs, más allá de la Ciberseguridad" src="images/DoS.png" width="4%">	Ataque DoS (Denial Of Service) utilizando SYNFlood

> [!IMPORTANT]
> Laboratorio SynFlood en <b>framework Metasploit</b> (Kali Linux 2023.4). Un ataque DoS o DDoS no es más que el envío de un número exageradamente elevado de peticiones a una dirección IP que, normalmente, suele ser un servidor o infraestructura, el cual llega a ser incapaz de gestionar dicho número de peticiones, causando un error o crasheo del sistema y la detención y/o reinicio de servicios, quedando inaccesible al resto de usuarios. 

> Ahora bien, la diferencia entre un ataque <b>DoS (Denial of Service o Denegación de Servicio)</b> y un <b>ataque DDoS ( Distributed Denial of Service o Denegación de Servicio Distribuido)</b> es muy sencilla ya que en el ataque DoS, el atacante cuenta con un único equipo, mientras que para el ataque DDoS se usan múltiples máquinas simultáneamente. Estas máquinas suelen pertenecer a <b>botnets</b> (redes de equipos/servidores controlados por un único atacante).


> ### Primeros conceptos: MODELO TCP/IP :computer:
> Como primer concepto, se ha de conocer el <b>modelo TCP/IP</b>, protocolo de red que permite la comunicación a través de Internet (abreviatura de <b>Protocolo de control de transmisión/Protocolo de Internet</b>), el cual es un protocolo estándar y un modelo (en la actualidad) de 4 capas que define cómo se transmiten los datos a través de una red y cómo se comunican los dispositivos. Su origen se da en la década de 1970 gracias al Departamento de Defensa de USA (DOD), ya que se pretendía crear una red que pudiera funcionar incluso si partes de ella resultaran dañadas o destruidas. Posteriormente, el modelo TCP/IP se publicó por primera vez en 1981 (versión 4) y luego se actualizó a la versión 6 en 1995. Veamos todas las capas TCP/IP: 

> <b>1.- Capa de aplicación</b>: es la capa más alta del modelo TCP/IP, un marco que define cómo se comunican los dispositivos a través de una red, es decir, es la capa más cercana al usuario final y representa las aplicaciones que se ejecutan en el dispositivo y utilizan la red para comunicarse y cuya función principal es proporcionar un medio para que las aplicaciones accedan a la red y se comuniquen con otros dispositivos. Así, se presentan un conjunto de protocolos que permiten a las distintas aplicaciones enviar y recibir datos a través de la red, como por ejemplo:
> - HTTP (Protocolo de transferencia de hipertexto): HTTP es el protocolo principal para transferir páginas web y otros datos en la World Wide Web.
> - SMTP (Protocolo Simple de Transferencia de Correo): SMTP se utiliza para enviar correos electrónicos.
> - DNS (Sistema de Nombres de Dominio): DNS traduce nombres de dominio (por ejemplo, www.example.com) en direcciones IP.
> - RDP (Protocolo de Escritorio Remoto): Este protocolo se utiliza para acceder remotamente a un ordenador de sobremesa.
> - Telnet: Telnet permite que un ordenador se conecte al ordenador local.
> - SNMP (Protocolo simple de gestión de red): SNMP supervisa y gestiona los dispositivos de red conectados a través de una IP.
> - FTP (Protocolo de Transferencia de Archivos): El protocolo FTP transfiere archivos entre ordenadores.
> - HTTPS (Protocolo Seguro de Transferencia de Hipertexto): HTTPS es una versión segura de HTTP que cifra la comunicación entre un servidor web y un cliente.








- <b>Paso 1</b>: Ya dentro de Metasploit Framework, se utilizará el exploit multi handler, payload que se utiliza para conectar con el objetivo. Dependiendo del tipo de payload, el handler queda a la espera (está en modo escucha) de una conexión por parte del payload cargado en el objetivo (reverse payload), llegando a iniciar una conexión contra el host y puertos objetivo en ciertos casos (bind payload). En la consola, se codifica:
<b>

```
use exploit/multi/handler
```
</b>

