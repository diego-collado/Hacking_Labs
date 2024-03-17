# <img alt="Hacking_Labs, más allá de la Ciberseguridad" src="images/warning.png" width="4%">	TCP-Syn Flood 

> [!IMPORTANT]
> Como ya se ha comentado, un <b>ataque por inundación SYN (SYN flooding)</b> es un método que el atacante realiza un ataque de denegación de servicio (DoS) a un servidor remoto mediante el envío repetido de <b>paquetes SYN (sincronización)</b> a cada puerto en el servidor, usando direcciones IP falsas, cuyo último resultado es la sobrecarga de información en el sistema, algo que ralentizará las respuestas o, incluso, dejará completamente caído el servidor completo, deteniendo el acceso del tráfico legítimo al mismo.

> ### Funcionamiento de este ataque :globe_with_meridians:
> Veamos un esquema de conexión real:

<p align="center">
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="images/SYNFlood_DDoS_1">
  <source media="(prefers-color-scheme: light)" srcset="images/SYNFlood_DDoS_1.png">
  <img alt="Hacking_Labs, más allá de la Ciberseguridad" src="images/SYNFlood_DDoS_1.png" width="25%">
</picture>
</p>

> De entre todos los ataques que utilizan la saturación de sistema, SYN flood sigue un patrón de ataque que constituye un abuso del TCP Threeway Handshake.

<p align="center">
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="images/SYNFlood_DDoS_2">
  <source media="(prefers-color-scheme: light)" srcset="images/SYNFlood_DDoS_2.png">
  <img alt="Hacking_Labs, más allá de la Ciberseguridad" src="images/SYNFlood_DDoS_2.png" width="25%">
</picture>
</p>

> Así, como hemos comentado anteriormente, el cliente (atacante) envía un paquete de sincronización (SYN) al servidor, el cual, al recibir este paquete, responde con un paquete de sincronización (SYN) y una confirmación (ACK). La conexión concluye con el acuse de recibo (ACK) por parte del cliente pero, en caso de que esta no se produzca, los sistemas se pueden paralizar, ya que el servidor no cuenta en su memoria con suficientes conexiones confirmadas. 


> ### ¿Cómo detectar un ataque de inundación SYN? :shield:
> La detección como tal puede llegar a ser muy dificil debido a que no se puede distinguir de los picos de tráfico legítimo, y más cuando el posible atacante está enmascarando y ocultando su IP real o si utiliza puertos aleatorios de origen. Así, se pueden utilizar múltiples técnicas que permiten identificar este tipo de ataques:
> - <b>Monitoreo del tráfico de red en busca de actividad sospechosa<b>:Se podrían utilizar herramientas de captura y análisis de paquetes de red (Paessler PRTG Network Monitor, ManageEngine Netflow Analyzer, Scrutinizer, NetflowAuditor, nTop, Pandora NTA, Wireshark, sflowtool, Nfsen, Intermapper Flows, FlowViewer o ManageEngine Flow Analyzer)para inspeccionar el tráfico entrante y saliente del server en búsqued de anomalías como pueden ser niveles inusualmente altos de tráfico, tráfico proveniente de ubicaciones o fuentes poco comunes o un gran número de paquetes SYN sin paquetes ACK correspondientes. 

> - Verificar el estado de los recursos del server</b>: Se podrían utilizar comandos como <b>netstat</b>, <b>ss</b> o <b>iptraf</b> para verificar el estado de las conexiones TCP y comprobar si hay muchas conexiones tipo <b>SYN_RECV</b> o solicitudes de conexión <b>SYN_SENT</b> que no se completan. 

> - <b>Comprobación de uso de CPU</b>: donde se podría comprobar parámetros como el consumo de memoria, ancho de banda de la red del servidor y otros tantos. 


Utiliza SYN COOKIES u otros mecanismos de protección contra inundaciones SYN. Las SYN COOKIES son una técnica que permite al servidor manejar paquetes SYN sin asignar recursos hasta que se reciba el paquete ACK final. De esta manera, el servidor puede evitar mantener conexiones a medio abrir y desperdiciar recursos. Otros mecanismos de protección contra inundaciones SYN incluyen firewalls, balanceadores de carga, proxies o servicios de mitigación de DDoS que pueden filtrar el tráfico malicioso y bloquear paquetes SYN provenientes de direcciones IP enmascaradas.
> - 
> - 
> - 





- <b>Paso 1</b>: Se deberá clonar el repositorio en el equipo, para cual se codifica:
<b>

```
git clone https://github.com/susmithHCK/torghost.git
```
</b>

- <b>Paso 2</b>: Ahora, se procede entrar en la carpeta del sistema en la que se ha descargado TorGhost. Una vez dentro de la correspondiente carpeta, se dan los permisos de ejecución al archivo de instalación mediante el siguiente código:
<b>

```
chmod +x build.sh
```
</b>

Se instalará el paquete completo en el sistema gracias al siguiente código:
<b>

```
./build.sh
```
</b>

Es posible que no funcione la construcción del archivo sh, por lo que se podrá optar por instalar los requerimientos que pide la aplicación al sistema utilizando el siguiente código:
<b>

```
pip install -r requirements.txt
```
</b>


- <b>Paso 3</b>: Se arranca la aplicación TorGhost:
<b>

```
python3 torghost.py
```
</b>

En este momento, aparecerá una pantalla similar a la siguiente imagen:
<p align="center">
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="images/torghost_1.png">
  <source media="(prefers-color-scheme: light)" srcset="images/torghost_1.png">
  <img alt="Hacking_Labs, más allá de la Ciberseguridad" src="images/torghost_1.png" width="50%">
</picture>
</p>

- <b>Paso 4</b>: Para iniciar la redirección de todo el tráfico por la red TOR, utilizamos el siguiente comando:
<b>

```
python3 torghost.py -s
```
</b>

El script cargará y aparecerán los correspondientes mensajes de inicio de forzado, cambios de DNS y toda la carga para obtener un resultado óptimo, es decir, CURRENT IP: la nueva IP
<p align="center">
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="images/torghost_2.png">
  <source media="(prefers-color-scheme: light)" srcset="images/torghost_2.png">
  <img alt="Hacking_Labs, más allá de la Ciberseguridad" src="images/torghost_2.png" width="30%">
</picture>
</p>

- <b>Paso 5</b>: Posteriomente, se comprobará la dirección IP pública de la misma forma que hemos hecho la primera comprobación, ara lo cual se utilizará el navegador de Kali Linux (Mozilla Firefox), donde se introducirá la URL: 
<b>

```
https://www.cualesmiip.com/
```
</b>

Es posible que el cambio de IP motive la falta de carga de algunos sites... ¡¡Ya estamos en la red TOR!!

Para detener el script:
<b>
```
python3 torghost.py -x
```
</b>


Para cambiar el nodo de salida:
<b>
```
python3 torghost.py -r
```
</b>

> A partir de este momento, todo nuestro tráfico estará redirigido a través de la red TOR.