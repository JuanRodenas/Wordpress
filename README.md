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
      mkdir /home/jrodenas/docker/wordpress/redis
~~~~
Y le damos permisos al usuario www-data
~~~~
      sudo usermod -a -G www-data 'YOUR_USER'
      sudo chown -R www-data:www-data /home/jrodenas/docker/wordpress/wordpress
      sudo chown -R www-data:www-data /home/jrodenas/docker/wordpress/mysql
      sudo chown -R www-data:www-data /home/jrodenas/docker/wordpress/redis
      sudo chmod -R 775 /home/jrodenas/docker/wordpress/wordpress
      sudo chmod -R 775 /home/jrodenas/docker/wordpress/mysql
      sudo chmod -R 775 /home/jrodenas/docker/wordpress/redis
~~~~

Directorios:
* **backup** Las copias de seguridad de la base de datos. Para realizar una copia de seguridad de la base de datos y almacenarla en este directorio tan solo tenéis que ejecutar el comando `sudo docker-compose exec db backup`
* **files** Contendrá los archivos almacenados en nuestra nube. También contendrá los ficheros de configuración, ficheros de las aplicaciones instaladas, etc. Es importante realizar una copia de seguridad de este directorio/volumen de persistencia.
* **mysql** Contendrá la totalidad de ficheros de nuestra base de datos MySQL.
* **redis** Contiene las bases de datos que genera el servidor Redis. Obviamente también es interesante realizar una copia de seguridad de este directorio.

#### Crear la red interna para comunicar con los demás contenedores
Creada la red interna, ya podemos levantar el contenedor
~~~~
docker network create wordpress_internal
~~~~

#### ARCHIVO PHP.INI
Agregar su propio archivo por volumen todos los archivos de las carpetas conf.d se cargan: en docker-compose.yml
~~~~
      - ./php.ini:/usr/local/etc/php/conf.d/zzz-custom.ini
~~~~
mediante el comando docker
~~~~
      - v /path/to/your/php.ini:/usr/local/etc/php/conf.d/zzz-custom.ini
~~~~

En la misma ubicación que hemos indicado la carpeta wordpress, descargamos el `php.ini`
☑️ [php.ini](https://github.com/JuanRodenas/Nextcloud/blob/main/php.ini)

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
  - Modificamos las passwords y usuarios del docker-compose de `MySQL, REDIS y WORDPRESS`
  - Introducimos la red que levante en la red internal de docker: `TRUSTED_PROXIES=172.19.0.0/16`
~~~

#### Definir las variables de entorno de Wordpress
Las variables de entorno de configuración de Wordpress:
~~~
    environment:
      - WORDPRESS_DB_PASSWORD: 'YOUR_PASSWORD'
      - WORDPRESS_DB_HOST: db:3306
      - WORDPRESS_DB_USER: wordpress
      - WORDPRESS_DB_PASSWORD:  'YOUR_PASSWORD's
      - WORDPRESS_DB_NAME: wordpress
      - TZ=Europe/Madrid
      - REDIS_HOST=redis
      - REDIS_HOST_PASSWORD= 'YOUR_PASSWORD'
~~~

* Levantamos el contenedor con:
~~~
docker-compose up -d
~~~

Una vez ejecutado el comando se descargarán las imagenes del docker-compose y se crearán, levantarán los contenedores.


#### Acceder al contenedor o ver el log del contenedor
* Acceder al contenedor de Wordpress
~~~
docker exec -u root -t -i wordpress /bin/bash
~~~

* Vemos el contenedor:
~~~
docker logs wordpress
~~~

* Vemos todos los contenedores:
~~~
docker-compose logs -f
~~~

## ACCEDER A LA WEB O DASHBOARD DE WORDPRESS
Con el contenedor levantado tan solo tenemos que abrir el navegador web e ingresar a la URL que hemos indicado en el docker compose.
Una vez ingresadas la credenciales tendremos acceso al panel de control. Fíjense que estamos accediendo de forma segura mediante https y TLS.
![alt text](https://github.com/JuanRodenas/Wordpress/blob/main/pagina_web.png)

## 🎉 ¡Ready!
