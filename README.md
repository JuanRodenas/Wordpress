# Wordpress
Proyecto para crear un blog con wordpress con Docker.

![alt text](https://github.com/JuanRodenas/Wordpress/blob/main/WP%2Bnginx%2Bredis.png)
<p align="center">
    <a href="https://www.ui.com/">
        <img src="https://github.com/JuanRodenas/Ubiquiti/blob/main/files/UbiquitiXEdgeMax.jpg" alt="UbiquitiXEdgeMax">
    </a>
    <br>
    <strong>Ubiquiti networks x EdgeMax©</strong>
</p>
<!-- markdownlint-enable MD033 -->

#
---
### Imágenes docker y arquitecturas
- Supported architectures:
amd64, arm32v5, arm32v6, arm32v7, arm64v8, i386, mips64le, ppc64le, s390x

- Imágenes recomendadas:
fpm-alpine
php8.1-fpm-alpine
wordpress:latest
---
#### Documentación oficial
Enlaces a la documentación oficial de wordpress:
<p> <a href="https://es.wordpress.org/support/">Documentación oficial</a></p>
<p> <a href="https://github.com/WordPress/wordpress-develop/tree/5.9/src">Código oficial Github</a></p>
<p> <a href="https://hub.docker.com/_/wordpress">Código oficial en docker hub</a></p>
* Un blog usando la imagen oficial de WordPress.

## PREPARACIÓN DE LOS ARCHIVOS Y DIRECTORIOS

#### Crear los directorios donde se montarán los volúmenes de persistencia
Creamos los directorios donde se montarán los volúmenes de persistencia
~~~
mkdir /patch/to/data/wordpress/wordpress
mkdir /patch/to/data/wordpress/mysql
mkdir /patch/to/data/wordpress/redis
~~~~

Directorios:
* **files** Contendrá los archivos almacenados en nuestra nube. También contendrá los ficheros de configuración, ficheros de las aplicaciones instaladas, etc. Es importante realizar una copia de seguridad de este directorio/volumen de persistencia.
* **mysql** Contendrá la totalidad de ficheros de nuestra base de datos MySQL.
* **redis** Contiene las bases de datos que genera el servidor Redis. Obviamente también es interesante realizar una copia de seguridad de este directorio.
* **backup** Las copias de seguridad de la base de datos. En caso de utilizar un volumen llamado `/backup`, puede realizar una copia de seguridad de la base de datos y almacenarla en este directorio tan solo tenéis que ejecutar el comando `sudo docker-compose exec db backup`

#
<blockquote class="is-info"><p>Los pasos que se explican a continuación están basados en una red que puede diferir de la que tú tienes montada. Si sigues al pie de la letra todos los pasos, pueden no coincidir con la configuración de tu <em>red</em> y dejarla inservible. Adapta en todo momento lo que a continuación se expone para que cuadre con tu red.</p></blockquote>

#### Crear la red interna para comunicar con los demás contenedores
Creada la red interna, ya podemos levantar el contenedor
~~~~
docker network create wordpress_internal
~~~~

## LEVANTAR EL CONTENEDOR DE WORDPRESS
En la misma ubicación que hemos indicado la carpeta WordPress, descargamos el `docker-compose.yml`
<p>☑️ <a href="https://github.com/JuanRodenas/Wordpress/blob/main/traefik/docker-compose.yml">docker-compose.yml</a></p>
<p>Y creamos el archivo de configuración .env con la configuración del archivo <a href="https://github.com/JuanRodenas/Wordpress/blob/main/traefik/.env">.env 📦</a></p>

~~~
touch .env
nano .env
~~~

Cuando se abra el editor de texto configuraremos la configuración del archivo con el dominio, la password, la database y el usuario.

#### Definir las variables del archivo .env
Las variables de entorno de configuración del archivo <code>.env</code> y modificamos los volúmenes del <code>docker-compose.yml</code>:
<p>variables de entorno de configuración del archivo <code>.env</code>:</p>
<p>  &nbsp;&nbsp;<code>HOST_DOMAIN=domain.com</code></p>
<p>  &nbsp;&nbsp;<code>DATABASE=wordpress</code></p>
<p>  &nbsp;&nbsp;<code>PASSWORD=password</code></p>
<p>  &nbsp;&nbsp;<code>USER=user</code></p>
<p>  &nbsp;&nbsp;<code>ROOT_PASSWORD=password</code></p>

Levantamos el contenedor con:
~~~
docker-compose up -d
~~~
Una vez ejecutado el comando se descargarán las imagenes del docker-compose, se crearán y levantarán los contenedores.
<p>  &nbsp;&nbsp;<sup>Si ya hemos descargado las imagenes previamente, sólo se crearán y levantarán los contenedores.</sup></p>

#### Levantar REDIS
Una vez hecho esto, tienes que instalar un plugin para WordPress que te permite interaccionar con Redis. Este plugin o complemento se llama <a href="https://wordpress.org/plugins/redis-cache/">Redis Object Cache.</a></p>
Una vez lo tengas configurado y levantado, hay trabajo que realizar. Tienes que editar el archivo `wp-config.php`
![alt text](https://github.com/JuanRodenas/Wordpress/blob/main/Redis.PNG)

y añadir los siguientes parámetros en el archivo:
```
define('WP_REDIS_HOST', 'wpredis');
define('WP_REDIS_PORT', 6379);
define('WP_REDIS_TIMEOUT', 1);
define('WP_REDIS_READ_TIMEOUT', 1);
define('WP_REDIS_DATABASE', 0);
```
Una vez instalado, tienes que acceder a su configuración y habilitarlo para que entre en funcionamiento. Comprueba que todos los parámetros que te devuelve son correctos y que todo funciona como se espera.

#### Acceder al contenedor o ver el log del contenedor
* Vemos todos los contenedores:
~~~
docker-compose logs -f
~~~
* Acceso al contenedor
~~~
docker exec -u root -t -i wordpress /bin/bash
~~~

## ACCEDER A LA WEB O DASHBOARD DE WORDPRESS
Con el contenedor levantado tan solo tenemos que abrir el navegador web e ingresar a la URL que hemos indicado en el docker compose.
Una vez ingresadas la credenciales tendremos acceso al panel de control. Fíjense que estamos accediendo de forma segura mediante https y TLS.
![alt text](https://github.com/JuanRodenas/Wordpress/blob/main/pagina_web.png)

## 🎉 ¡Ready!
