- Máquina:Escritorio Pr. Combinada Serv. Ap.
	- Instalado php
	- Instalado nginx
	- Instalado node
	- Instalado mysql
	- PENDIENTE php y complementos
	- Opcional phpmyadmin

- Despliegue 1:
	- Apache puerto 8080
	- http://localhost:8080
	- Aplicación demo de otros días.

      Clonamos la aplicacion del repositorio de Rafa con `sudo git clone https://github.com/rafacabeza/demoappphp.git`.

   Cambiamos el fichero ports.conf al 8080 y creamos el fichero apache.

 Instalamos las dependencias con `sudo apt install php libapache2-mod-php php-mysql`.


 Creamos la base de datos y cambiamos el fichero index.php.

![](/capturaspractica4/codigobdd1.PNG)

Y daremos permisos al usuario  con `GRANT ALL PRIVILEGES ON demo.* TO 'demo'@'localhost';`. 

Cambiamos el fichero php.

![](/capturaspractica4/modificaphp.PNG)


 Reiniciamos apache con `sudo systemctl restart apache2.service` y listo, tendríamos nuestra aplicacion corriendo.

 ![](/capturaspractica4/puerto8080.PNG)
	
- Despliegue 2
	- Node puerto 3000
	- http://localhost:3000
	- Aplicación demo de otros días

    Clonamos la aplicacion del repositorio de Rafa con `sudo git clone https://github.com/rafacabeza/demoapinode.git`
    Le damos permisos a la carpeta con `sudo chmod -R 777 demoapinode/`


    Modificamos el fichero para que se conecte a la base de datos , que previamente tendremos que crear como en el ejemplo anterior y crear la tabla deseos,
    dentro de mysql nos conectamos a demo y la creamos con este código .

    ![](/capturaspractica4/codigobdd2.PNG)

    Iniciamos el servidor con `npm start` dentro de la carpeta de la aplicacion y lo tendríamos corriendo en el puerto 3000.


    ![](/capturaspractica4/puerto3000.PNG)


- Configuración proxy inverso
	- Sitio1: http://php.local	
	- Sitio1: http://node.local
	- Puerto 80
	- Recuerda que debes configurar el fichero hosts

    En nginx, en sites available creamos el fichero proxy.

     ![](/capturaspractica4/proxy.PNG)


    Hacemos un enlace simbolico con `sudo ln -s /etc/nginx/sites-available/proxy  /etc/nginx/sites-enabled/` y reiniciamos nginx con `sudo systemctl restart nginx.service `.


    En el fichero hosts modificamos con la ip local.

     ![](/capturaspractica4/hosts.PNG)


     Y tendremos las dos aplicaciones corriendo en el puerto 80.

     ![](/capturaspractica4/exito1.PNG)

     ![](/capturaspractica4/exito2.PNG)

