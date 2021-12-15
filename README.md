# Wordpress
Proyecto para crear un blog con wordpress con Docker.

![alt text](https://github.com/JuanRodenas/Wordpress/blob/main/wordpress.jpeg)

📁 [Documentación oficial](https://codex.wordpress.org/es:)



* Un blog usando la imagen oficial de WordPress.
* Un servidor de administración de bases de datos utilizando la imagen oficial de Adminer.

#### archivo php.ini


# LEVANTAR EL CONTENEDOR DE WORDPRESS
En la misma ubicación que hemos indicado la carpeta WordPress, descargamos el `docker-compose.yml`

☑️ [docker-compose.yml](enlace)(enlace)

Abra el archivo en su editor: docker-compose.yml
~~~~
nano docker-compose.yml
~~~~
Y modificamos las siguientes variables:
~~~~
  - Modificamos la ruta de los volúmenes a la ruta donde estén los archivos.
  - Modificamos la red `networks:`
  - Modificamos las passwords y usuarios del docker-compose de `MySQL, REDIS y WORDPRESS`
  - Introducimos la red que levante en la red internal de docker: `TRUSTED_PROXIES=172.19.0.0/16`
 ~~~~
 ```
#### Definir las variables de entorno de Wordpress
Las variables de entorno de configuración de Wordpress:
~~~
    environment:

~~~

* Levantamos el contenedor con:
~~~~
docker-compose -up d
~~~~

Una vez ejecutado el comando se descargarán las imagenes del docker-compose y se crearán, levantarán los contenedores.


#### Acceder al contenedor o ver el log del contenedor
* Acceder al contenedor de Wordpress
~~~~
docker exec -u root -t -i wordpress /bin/bash
~~~~

* Vemos el contenedor:
~~~~
docker logs wordpress
~~~~

* Vemos todos los contenedores:
~~~~
docker-compose logs -f
~~~~

## ACCEDER A LA WEB O DASHBOARD DE WORDPRESS
Con el contenedor levantado tan solo tenemos que abrir el navegador web e ingresar a la URL que indicamos en TRUSTED_DOMAINS.
Una vez ingresadas la credenciales tendremos acceso al panel de control. Fíjense que estamos accediendo de forma segura mediante https.

## 🎉 ¡Ready!
