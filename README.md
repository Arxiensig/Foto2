
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



## Configuracion IDE ( Visual Studio Code)

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
Una vez se haya instalado symfony estaremos listos para proceder con la continuación de la instalación.

## API Reference

#### Get all items

```http
  GET /api/items
```

| Parameter | Type     | Description                |
| :-------- | :------- | :------------------------- |
| `api_key` | `string` | **Required**. Your API key |

#### Get item

```http
  GET /api/items/${id}
```

| Parameter | Type     | Description                       |
| :-------- | :------- | :-------------------------------- |
| `id`      | `string` | **Required**. Id of item to fetch |

#### add(num1, num2)

Takes two numbers and returns the sum.


## Authors

- Martin Gregorio Abad ( arxiensig@gmail.com)


## Usage/Examples

```javascript
import Component from 'my-project'

function App() {
  return <Component />
}
```

