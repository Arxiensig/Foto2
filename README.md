
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

```javascript
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
```javascript
	DB_USER=root
	DB_PASSWORD=dbrootpass
	DB_HOST=add-dbms
	DB_NAME=nba
    DATABASE_URL="mysql://${DB_USER}:${DB_PASSWORD}@${DB_HOST}:3306/${DB_NAME}?serverVersion=5.7"
```
Deberá de quedarnos algo similar a esto:

<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/6.png?raw=true">
</p>

Guardaremos el fichero y pasaremos al siguiente. Ahora buscaremos dentro del directorio *files* el archivo sql llamado **nba_2022_02_02.sql**

Dentro de este archivo deberemos de añadir unas lineas de código que nos permitirán crear las tablas una vez vayamos a crear toda la BBDD.

Justo antes de: *DROP TABLE IF EXISTS `equipos`*

```javascript
    CREATE SCHEMA if not exists nba;
	USE nba;
```

Quedará de la siguiene forma:

<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/7.png?raw=true">
</p>

Una vez este todo listo nos dispondremos a entrar en mySQL desde el root.
Para esto entraremos una vez mas en el contenedor de la api. Recordar

```bash
    docker-compose exec web bash
```
Y una vez dentro ejecutaremos el siguiente comando:
```bash
    mysql -u root -pdbrootpass -h add-dbms < files/nba_2022-02-02.sql
```
Con esto entraremos dentro del mySQL y gracias a la configuración anterior en el archivo **sql** las tablas se cargarán de manera automática.

Una vez dentro podremos utilizar la base de datos con: **use nba;*


<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/8.png?raw=true">
</p>

Desde mySQL podremos ver las tablas creadas, hacer uso de sentencias query y todo lo relacionado con mySQL. Nosotros no haremos nada de esto, simplemente saldremos.

Para salir de este modo solo deberemos escribir "**exit;**"

IMPORTANTE: 
Para volver a entrar en mySQL deberemos de ejecutar lo siguiente:

```bash
    mysql -u root -pdbrootpass -h add-dbms
```
No añadiremos las filas que hacen referencia al **sql**; ya que de hacerlo podría machacar toda la información ya creada.
Trataremos de evitar este posible final.


## Insertar datos en la BBDD:

En esta sección trabajaremos para poder añadir todos los datos de los csv en las tablas de la BBDD.

Dentro del directorio que contiene esta api (api-nba) crearemos un directorio llamado **scripts**

<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/9.png?raw=true">
</p>

Dentro de este directorio crearemos un fichero **py** llamado *cargarEquipos.py*. Este fichero será el encargado de conectarnos con la BBDD y a la vez cargar los datos utilizando el csv correspondiente.

Este archivo se dividirá en dos partes.
Por un lado deberemos de conectarnos a la BBDD:

<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/10.png?raw=true">
</p>

Recordar que deberemos de configurar correctamente el apartado de usuario, password, host y database.

Por otro lado estará la sección donde leeremos el csv y modificaremos la tabla.
Tened cuidado con el Query y la posición de los values.

<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/11.png?raw=true">
</p>

Haremos los mismo con las demás tablas. Adecuando correctamente la información para poder cargar cada una de las tablas con su documento *csv*.


## Cargar los Entity:

Una vez hemos llegado a este punto nos tocará cargar los Entity con información de las tablas de la BBDD.

Para poder crear estos entity ejecutaremos el siguiente comando en el PowerShell

Recordar que deberemos ejecutarlo desde el contenedor de docker de la api-nba.

```bash
    php bin/console doctrine:mapping:convert annotation src/Entity --from-database
```

Una vez ejecutado esto esperaremos a que se instale correctamente todo.





## Controladores y repositorios:

Tras crear los Entity deberemos proceder a crear los Controlador y los Repository de cada tabla.

Debería de quedar algo similar a esto:

<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/12.png?raw=true">
</p>

Ahora nos tocará realizar los "extends" de cada uno de los **repositorios**. 
En la linea donde pone el nombre de la clase pondremos al final: **extends EntityRepository**

```bash
    namespace App\Repository;

    use App\Entity\Equipos;
    use Doctrine\ORM\EntityRepository;


    class EquiposRepository extends EntityRepository
```

En los archivos de los **controladores** pondremos al final:

```bash
    namespace App\Controller;
    use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;

    class EquiposController extends AbstractController
```

Y finalmente en cada una de sus Entity:

```bash
/**
* Equipos
*
* @ORM\Table(name="equipos")
* @ORM\Entity(repositoryClass="App\Repository\EquiposRepository")
*/
```

Haremos lo mismo con todos los *controladores, repositorios y entity*.

Tras realizar esto deberemos de crear los **Getter** y los **Setter** dentro de cada uno de los Entity. De lo contrario no podremos trabajar correctamente.

Una vez todo este configurado pasaremos a gestionar los **ENDPOINTS**.




##  Creando Endpoints

Ahora comenzaremos a crear los primeros endpoints. 

Lo primero será dirigirnos al archivo de **routes**.

<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/13.png?raw=true">
</p>

Aquí deberemos configurar la ruta que escribiremos en el navegador para poder accionar la función que crearemos en el controlador.
También le indicaremos a que controlador y que función hace objetivo cada vez que lanzamos la *url*

Deberemos de tener claro que cada vez que lancemos la *url* en el navegador llamaremos a una función en el **controlador**, y este **controlador** al mismo tiempo llamará a una función en el **repositorio**.

routes:

```bash
equipos_equipos:
  path: /equipos
  controller: App\Controller\EquiposController::getTeamInfo

equipos_equipo:
  path: /equipos/{equipo}
  controller: App\Controller\EquiposController::getTeamInfoByName

equipos_equipo_jugadores:
  path: /equipo/jugadores
  controller: App\Controller\EquiposController::jugadoresByEquipos

equipos_equipo_nombre:
  path: /equipo/jugadores/{nombre}
  controller: App\Controller\EquiposController::jugadoresByEquipo

jugadores_jugadores:
  path: /jugadores
  controller: App\Controller\JugadoresController::getPlayerInfo

jugadores_nombre:
  path: /jugadores/{nombre}
  controller: App\Controller\JugadoresController::getPlayerInfoByName

jugadores_fisico:
  path: /jugador/fisico/{nombre}
  controller: App\Controller\JugadoresController::getPlayerAttrByName

estadisticas_jugador:
  path: estadisticas/jugador/{nombre}
  controller: App\Controller\EstadisticasController::getStatsByPlayer

estadisticas_jugador_media:
  path: estadisticas/jugador/{nombre}/avg
  controller: App\Controller\EstadisticasController::getStatsByPlayerMedia

partidos_local:
  path: partidos/resultados/local/{nombre}
  controller: App\Controller\PartidosController::getLocalByName

partidos_visitante:
  path: partidos/resultados/visitante/{nombre}
  controller: App\Controller\PartidosController::getVisitByName

partidos_media_local:
  path: partidos/resultados/media/local/{nombre}
  controller: App\Controller\PartidosController::getMediaByLocalName

partidos_media_visitante:
  path: partidos/resultados/media/visitante/{nombre}
  controller: App\Controller\PartidosController::getMediaByVisitanteName
```


ENDPOINT a-> /equipos

Controlador:
<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/20.png?raw=true">
</p>

Resultado:
<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/a1.png?raw=true">
</p>

ENDPOINT b-> /equipos/{equipo}

Controlador:
<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/21.png?raw=true">
</p>

Resultado:
<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/a2.png?raw=true">
</p>

ENDPOINT c-> /equipo/jugadores

Controlador:
<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/22.png?raw=true">
</p>

ENDPOINT d-> /equipo/jugadores/{nombre}

Controlador:
<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/23.png?raw=true">
</p>

ENDPOINT e-> /jugadores

Controlador:
<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/30.png?raw=true">
</p>

ENDPOINT f-> /jugadores/{nombre}

Controlador:
<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/31.png?raw=true">
</p>

ENDPOINT g-> /jugador/fisico/{nombre}

Controlador:
<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/32.png?raw=true">
</p>

ENDPOINT h-> estadisticas/jugador/{nombre}

Controlador:
<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/40.png?raw=true">
</p>
Repositorio:
<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/41.png?raw=true">
</p>

ENDPOINT i-> estadisticas/jugador/{nombre}/avg

Controlador:
<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/42.png?raw=true">
</p>

Repositorio:
<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/43.png?raw=true">
</p>
<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/44.png?raw=true">
</p>

ENDPOINT j-> partidos/resultados/local/{nombre}

Controlador:
<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/50.png?raw=true">
</p>

Repositorio:
<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/51.png?raw=true">
</p>

ENDPOINT k-> partidos/resultados/visitante/{nombre}

Controlador:
<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/52.png?raw=true">
</p>

Repositorio:
<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/53.png?raw=true">
</p>

ENDPOINT l-> partidos/resultados/media/local/{nombre}

Controlador:
<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/54.png?raw=true">
</p>

Repositorio:
<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/55.png?raw=true">
</p>

ENDPOINT m-> partidos/resultados/media/visitante/{nombre}

Controlador:
<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/56.png?raw=true">
</p>

Repositorio:
<p align="center">
	<img src="https://github.com/Arxiensig/Foto2/blob/main/57.png?raw=true">
</p>

## Authors

- Martin Gregorio Abad
- [@arxiensig](https://github.com/Arxiensig)

