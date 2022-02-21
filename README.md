
# Documentación Proyecto ADD

Documentación realizada paso a paso para la correcta realización de esta práctica.


## Antes de la instalación:


Lo primero que deberemos hacer es descargar el fichero "**add-project-files**" de la web utilizando tu usuario como alumno: [AULES](https://aules.edu.gva.es)

Este fichero contendrá una carpeta con todos los CSV que más adelante utilizaremos en el proyecto para insertar datos en la BBDD.

Tras esto nos dirigiremos al directorio "**add-env**" que contendrá de antemano una carpeta que necesitaremos para continuar con el proyecto.

Haremos clic derecho al directorio llamado: "**sf-app-provisioning**" y lo copiaremos en la ruta donde deseemos tener nuestra API.

<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/1.png?raw=true">
</p>

Una vez copiado, renombraremos dicho directorio y le llamaremos "**api-nba**"
## Configuracion IDE (Visual Studio Code)

El siguiente paso seria abrir el directorio creado con anterioridad con nombre "*api-nba*" con Visual Studio.

Una vez dentro nos dirigiremos al fichero con nombre ***"env.webapp***" que se encuentra en la carpeta llamada *vendor* y cambiaremos los nombres por "apinba.local". Quedará algo como esto:

```bash
APACHE_SERVER_NAME=apinba.local
APACHE_SERVER_ALIAS=apinba.local
APACHE_DOCUMENT_ROOT=/code/public
```
Lo siguiente será en el fichero **docker-compose.yml** modificar el nombre del contenedor.

```bash
container_name: apinba
```

<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/2.png?raw=true">
</p>
## Instalación paso a paso

Ahora que ya hemos realizado las configuraciones previas podemos proceder a la instalación

Lo primero será levantar la api de docker.

```bash
  docker-compose up -d
```
Esperaremos a que termine de montarse y procederemos a entrar en el contenedor.

```bash
    docker-compose exec web bash
```    
Ahora que ya hemos entrado como root podemos proceder a la instalación de symfony. Para proceder con la instalación ejecutaremos el siguiente comando desde el root.

```bash
    symfony new api-nba --version=4.4 --full --no-git
```
Una vez se haya instalado symfony deberemos proceder a copiar y despues borrar los siguiente ficheros.
Deberemos copiar todos los archivos(menos los dos docker-compose que hay en la carpeta) un directorio atrás. Para ello ejecutaremos:

```bash 
	cd api.nba/
	cp .env ../
```
Volveremos una carpeta atras y borraremos el directorio api-nba:

```bash
    rm -r api.nba/
```
Ahora deberemos configurar tu dispositivo para poder conectarnos a la API.
## Configuración: apinba.local (hosts):

Deberemos dirigirnos al archivo *hosts* dentro del directorio que tendrás en System32 ( Windows).
La ruta por defecto sería: **C:\Windows\System32\drivers\etc\hosts**

<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/3.png?raw=true">
</p>

Deberemos añadir el nombre que hemos configurado en el apartado de "*env.webapp*"


## Mostrar "apinba.local" en el navegador:

Abriremos el PowerShell y entraremos en el contenedor de docker.
Recordemos:

```bash
    docker-compose exec web bash
```
Una vez dentro procederemos a dar permisos totales sobre el fichero **var/cache** y **var/log**.
```bash
    chmod -R 777 var/cache/* var/log
```
De esta forma lograremos entrar en la api de symfony desde el navegador.

<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/4.png?raw=true">
</p>

La ruta que deberemos escribir en el navegador para entrar será:

```bash
    apinba.local:8082
```
Recordar que el puerto puede ser distinto dependindo del que tengas por defecto o hayamos configurado.


## Ficheros CSV

Ahora tendremos que manipular los csv que utilizaremos para cargar toda la base de datos.

En este caso utilizaremos una BBDD basada en la NBA. Pero podriamos utilizar cualquier tipo de CSV (dependiendo de la temática que deseemos utilizar).

En nuestro caso ya tendremos descargados los csv. Estos ficheros se encontrarán dentro del fichero: **add-project-files**

Lo descomprimiremos y lo copiaremos en la carpeta de api-nba. Una vez copiado reenombraremos este directorio copiado como **files**. Una vez hecho debería de quedar de la siguiente forma:


<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/5.png?raw=true">
</p>


## Configuración de la API

Ahora ya podremos comenzar a configurar los propios archivos dentro del contenedor de la api.


Iniciaremos el *IDE* llamado **PhpStorm** y modificaremos el fichero **.env**. 
Pondremos lo siguiente:
```bash
	DB_USER=root
	DB_PASSWORD=dbrootpass
	DB_HOST=add-dbms
	DB_NAME=nba
    DATABASE_URL="mysql://${DB_USER}:${DB_PASSWORD}@${DB_HOST}:3306/${DB_NAME}?serverVersion=5.7"
```
<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/6.png?raw=true">
</p>

Guardaremos el fichero y pasaremos al siguiente. Ahora buscaremos dentro del directorio *files* el archivo sql llamado **nba_2022_02_02.sql**

Dentro de este archivo deberemos de añadir unas lineas de código que nos permitirán crear las tablas una vez vayamos a crear toda la BBDD.

Justo antes de: *DROP TABLE IF EXISTS `equipos`*

```bash
    CREATE SCHEMA if not exists nba;
	USE nba;
```

Quedará de la siguiene forma:

<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/7.png?raw=true">
</p>
## Authors

- Martin Gregorio Abad ( arxiensig@gmail.com)


## Usage/Examples

```javascript
import Component from 'my-project'

function App() {
  return <Component />
}
```

