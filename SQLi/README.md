<p align="center">
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="images/inyeccion-sql.png">
  <source media="(prefers-color-scheme: light)" srcset="images/inyeccion-sql.png">
  <img alt="Hacking_Labs, más allá de la Ciberseguridad" src="images/inyeccion-sql.png" width="25%">
</picture>
</p>

# :syringe:	SQLi: Structured Query Language Injection

> [!CAUTION]
> Laboratorios preparados para la realización de <b>ataques SQLi (SQL Injection)</b> (Kali Linux 2023.4). Este laboratorio trata de realizar un ataque de inyección de código en el lado servidor, es decir, inyectar código malicioso a la hora de realizar una consulta que se envía a una base de datos a través de una aplicación web, si bien este tipo de ataque aprovecha las vulnerabilidades de seguridad que están presentes en las aplicaciones que interactúan con bases de datos de tipo SQL, como pueden ser <b>MySQL</b>, <b>PostgreSQL</b>, <b>Microsoft SQL Server</b>, <b>Oracle Database</b> y <b>SQLite</b>, por ejemplo. 

## SQLi... ¿No es un problema del DBA (DataBase Administrator)?
Como estamos comentando, el objetivo principal de un <b>ataque de inyección SQL</b> es manipular la consulta SQL original de manera que se ejecuten "acciones no autorizadas" en la base de datos (extracción de datos confidenciales, modificación de datos o eliminación de información, entre otras muchas).
Ahora bien, la cruda realidad nos demuestra que debemos considerar la responsabilidad de desarrolladores y DBA en los posibles ataques de este tipo ya que:

- Desarrolladores de software: Tienen la responsabilidad de escribir código seguro y resistente a los ataques de inyección SQL, lo que implica implementar prácticas de codificación segura (uso de consultas parametrizadas, uso del filtrado y validación adecuados de las entradas de usuario, manejo de expresiones regulares o RegExp...)

- Administradores de sistemas: Deben mantener actualizados los sistemas y aplicaciones e implementar medidas de seguridad en capas (firewalls, IDS/IPS o SIEM) para prevenir y detectar posibles ataques.

- Usuarios finales: Aunque su responsabilidad es <b>indirecta</b>, los usuarios finales deberían adoptar buenas prácticas de seguridad, como usar contraseñas seguras (en longitud y tipo), ademásw de estar alerta ante posibles signos de actividad maliciosa en sitios web o aplicaciones.

> El funcionamiento consta de las siguientes fases:
  - <b>Identificación de la vulnerabilidad</b>: El atacante examina la aplicación web en busca de campos de entrada que puedan estar conectados a una base de datos y que puedan ser vulnerables a la inyección SQL, los cuales pueden ser cualquiera de los campos que tenemos disponibles en websites y aplicaciones web, como formularios de búsqueda, campos de inicio de sesión, parámetros de URL, entre otros.
  
  - <b>Inserción de código malicioso</b>: Una vez es identificado un campo vulnerable, el atacante introduce código SQL malicioso en dicho campo, lo que incluye fragmentos de código SQL diseñados para alterar el comportamiento de las consultas a la base de datos.

  - <b>Ejecución del ataque</b>: Cuando el servidor web procesa la solicitud del atacante, la aplicación web toma el código SQL malicioso introducido en el campo vulnerable y lo ejecuta directamente en la base de datos sin una adecuada validación o filtrado.... ¡¡Es el momento de la verdad!!
  
  - <b>Manipulación de la base de datos</b>: Dependiendo de la naturaleza del código SQL malicioso utilizado, el resultado del ataque puede variar desde permitir al atacante extraer información confidencial de la base de datos, modificar o eliminar datos, o incluso tomar el control total del sistema...
  
  -<b>Exfiltración de datos</b>: Después de realizar el ataque de inyección SQL con éxito, el atacante puede <b>exfiltrar</b> los datos obtenidos de la base de datos a través de la misma aplicación web o de otros métodos (descarga de archivos o visualización de información en pantalla). En este momento, cualquier herramienta es válida para conseguir la información deseada.

  