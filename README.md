# Wordpress
Proyecto para crear un blog con wordpress con Docker.

![alt text](https://github.com/JuanRodenas/Wordpress/blob/main/wordpress.jpeg)

📁 [Documentación oficial](https://codex.wordpress.org/es:)



* Un blog usando la imagen oficial de WordPress.

## PREPARACIÓN DE LOS ARCHIVOS Y DIRECTORIOS

#### Crear los directorios donde se montarán los volúmenes de persistencia
Creamos los directorios donde se montarán los volúmenes de persistencia
~~~
      mkdir /home/jrodenas/docker/wordpress/wordpress
      mkdir /home/jrodenas/docker/wordpress/mysql
~~~~
Y le damos permisos al usuario www-data
~~~~
      sudo usermod -a -G www-data 'YOUR_USER'
      sudo chown -R www-data:www-data /home/jrodenas/docker/wordpress/wordpress
      sudo chown -R www-data:www-data /home/jrodenas/docker/wordpress/mysql
      sudo chmod -R 775 /home/jrodenas/docker/wordpress/wordpress
      sudo chmod -R 775 /home/jrodenas/docker/wordpress/mysql
~~~~

Directorios:
* **backup** Las copias de seguridad de la base de datos. Para realizar una copia de seguridad de la base de datos y almacenarla en este directorio tan solo tenéis que ejecutar el comando `sudo docker-compose exec db backup`
* **files** Contendrá los archivos almacenados en nuestra nube. También contendrá los ficheros de configuración, ficheros de las aplicaciones instaladas, etc. Es importante realizar una copia de seguridad de este directorio/volumen de persistencia.
* **mysql** Contendrá la totalidad de ficheros de nuestra base de datos MySQL.

#### Crear la red interna para comunicar con los demás contenedores
Creada la red interna, ya podemos levantar el contenedor
~~~~
docker network create wordpress_internal
~~~~

## LEVANTAR EL CONTENEDOR DE WORDPRESS
En la misma ubicación que hemos indicado la carpeta WordPress, descargamos el `docker-compose.yml`

☑️ [docker-compose.yml](enlace)(enlace)

Abra el archivo en su editor: docker-compose.yml
~~~
nano docker-compose.yml
~~~

Y modificamos las siguientes variables:
~~~
  - Modificamos la ruta de los volúmenes a la ruta donde estén los archivos.
  - Modificamos la red `networks:`
  - Modificamos las passwords y usuarios del docker-compose de `MySQL y WORDPRESS`
~~~

#### Definir las variables de entorno de Wordpress
Las variables de entorno de configuración de Wordpress:
~~~
    environment:
      WORDPRESS_DB_PASSWORD: 'YOUR_PASSWORD'
      WORDPRESS_DB_HOST: mysql_wordpress
      WORDPRESS_DB_NAME: wordpress
      WORDPRESS_DB_USER: wordpress
~~~

#### Levantamos el contenedor wordpress:
~~~
docker-compose up -d
~~~

Una vez ejecutado el comando se descargarán las imagenes del docker-compose y se crearán, levantarán los contenedores.

#### Ver el log del contenedor
* Vemos el contenedor:
~~~
docker logs nextcloud
~~~
* Vemos todos los contenedores:
~~~
docker-compose logs -f
~~~

#### Acceder al contenedor o ver el log del contenedor
* Acceder al contenedor de Wordpress
~~~
docker exec -u root -t -i wordpress /bin/bash
~~~
* Una vez que hemos accedido al contenedor, tenemos que actualizar e instalar `sudo` y `nano` para poder modificar el archivo
~~~
apt update && apt upgrade
~~~
~~~
apt install sudo nano
~~~


## ACCEDER A LA WEB O DASHBOARD DE WORDPRESS
Con el contenedor levantado tan solo tenemos que abrir el navegador web e ingresar a la URL que hemos indicado en el docker compose.
Una vez ingresadas la credenciales tendremos acceso al panel de control. Fíjense que estamos accediendo de forma segura mediante https y TLS.
![alt text](https://github.com/JuanRodenas/Wordpress/blob/main/pagina_web.png)

## 🎉 ¡Ready!
