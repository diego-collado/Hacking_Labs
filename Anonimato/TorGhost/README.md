# <img alt="Hacking_Labs, más allá de la Ciberseguridad" src="hacker.png" width="4%">	TorGhost 

> [!IMPORTANT]
> Laboratorio TorGhost (Kali Linux 2023.4). TorGhost es un script de anonimización que redirige todo el tráfico de Internet a través de SOCKS5 TOR Proxy. Las solicitudes de DNS también se redirigen a través de Tor, evitando así DNSLeak, además de desactivar los paquetes inseguros que salen del sistema (solicitud de ping entre otros), los cuales pueden comprometer su identidad.

> ### Trabajando con TorGhost: Instalación y configuración :computer:
> TorGhost tiene las siguientes características:
> - Redirección de todo el tráfico de red hacía la red TOR, es decir, cualquier conexión del equipo que intente conectarse a Internet pasará por ella.
> - No se filtrará ningún ping, lo que protege nuestra identidad.
> - Fuerza a las aplicaciones a pasar por ella (al contrario que ProxyChain). 
> - Rechaza peticiones entrantes y salientes que puedan contener información sensible o pueda revelar nuestra IP real.
> - Protege de la fuga de DNS, permitiendo usar un DNS remoto anónimo.

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
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="torghost_1.png">
  <source media="(prefers-color-scheme: light)" srcset="torghost_1.png">
  <img alt="Hacking_Labs, más allá de la Ciberseguridad" src="torghost_1.png" width="50%">
</picture>

- <b>Paso 4</b>: Para iniciar la redirección de todo el tráfico por la red TOR, utilizamos el siguiente comando:
<b>

```
python3 torghost.py -s
```
</b>
El script cargará y aparecerán los correspondientes mensajes de inicio de forzado, cambios de DNS y toda la carga para obtener un resultado óptimo: CURRENT IP: la nueva IP

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="torghost_2.png">
  <source media="(prefers-color-scheme: light)" srcset="torghost_2.png">
  <img alt="Hacking_Labs, más allá de la Ciberseguridad" src="torghost_2.png" width="30%">
</picture>

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