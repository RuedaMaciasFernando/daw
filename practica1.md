### Practica 1 Servicios en red DAW
## Fernando Rueda

# Configuración en Ubuntu Server
 - IMPORTANTE : Recomendable modificar los ficheros con sudo
 - Editar el fichero hostnamme

 Tenemos ir a /etc/hostname y editarlo.

 ![cambiar el nombre de el host server](/capturaspractica1/hostname.PNG)

 - Configurar las tarjetas de red en Ubuntu Server.

 Tenemos que ir a /etc/netplan, le damos a tabular y solo podremos editar un fichero, que lo eidtaremos tal que así.

 ![Añadir red a server ](/capturaspractica1/asignarred.PNG)

 Después de esto haremos ejecutaremos estos comandos.
 - ```sudo netplan generate```
 - ```sudo netplan apply```

    # Configuración en los clientes 

 - Ubuntu 
    Vamos a añadir la ip correspondiente en los clientes para que se comuniquen con el host.

    En Ubuntu vamos a la configuración gráfica de las interfazes de red y la asignamos.

     ![Añadir red a cliente ubuntu ](/capturaspractica1/cambioredfijaubuntu.PNG)

     Ahora nos conectará al servidor sin problemas.

     ![Comunicacion con el server](/capturaspractica1/exitoubuntu.PNG)

     - Windows 
    Haremos lo mismo con windows. Primero cambiaremos el nombre.

     ![Cambiar nombre windows ](/capturaspractica1/cambionombrewindows.PNG)

     Y procederemos a cambiar la ip.

     ![Añadir ip windows](/capturaspractica1/cambioredwindows.PNG)



    # DHCP 
    Vamos a configurar una nueva tarjeta de red en el Servidor Ubuntu llamada enp4s0 con el procedimiento mencionado anteriormente.

    ![Añadir ip dhcp ubuntu](/capturaspractica1/redserverubuntu.PNG)

    Hacemos los siguientes comandos: 

- netplan apply y netplan generate

 - Y procedemos a instalar el servidor dhcp.

 - En caso de que de error nos pedirá de escribir este comando : ```sudo dpkg -configure -a```.

- ```apt-get install isc-dhcp-server```

Una vez instalado tendremos que configurar la interfaz en la que va a trabajar el servidor dhcp.
 ```/etc/default/isc-dhcp-server```.

  ![Modificar tarjeta dhcp ](/capturaspractica1/ficheroiscdhcpserver.PNG.PNG).

  Modificaremos el fichero principal  de configuración de DHCP :  ```/etc/dhcp/dhcpd.conf```. Bajaremos hasta que encontremos una declaracion parecida a esta.

   ![Modificar tarjeta dhcp ](/capturaspractica1/definiciondesubredserver.PNG).

   Solo habrá que descomentarla , poner la ip correspondiente y el rango.

   Y Procederemos a reiniciar el servicio con : ```service isc-dhcp-server restart```.


  # DHCP en los clientes

  Si la configuración anterior ha ido correctamente, solo tendremos que hacer el comando  ```ipconfig /release``` y ```ipconfig /renew ``` en Windows.
 
![Red windows con dhcp ](/capturaspractica1/renewwindows.PNG).



  Y en ubuntu podremos ver que con ```ip a ``` se ha actualizado automáticamente.

  ![Red Ubuntu con dhcp ](/capturaspractica1/ipaubuntu.PNG).

  Si hacemos ping al servidor en cualquier máquina no habrá problema.

  
 ![Ping cliente ubuntu](/capturaspractica1/pinclienteubuntu.PNG).

  # Asignación de leases   

  
Para comprobar el estado del servicio y que el puerto 67 está en espera (netstat), ejecutamos el comando ```service isc-dhcp-server status``` y después

```netstat -lun```, podemos ver que el estado esta vacío, con lo cual esta escuchando.

![Estado servidor](/capturaspractica1/estadoservidor.PNG).

![Puerto 67 ](/capturaspractica1/puerto67.PNG).

- Configurar la reserva de una ip fija en el servidor.

 Para ello vamos a tener que ver si tenemos alguna reserva realizada, para evitar errores. Ejecutando el comando ```dhcp-lease-list``` y mirando el archivo ```/var/lib/dhcp/dhcpd.leases_``` con cat.

 ![Mirar leases 1](/capturaspractica1/capturacomandoipfija1.PNG).

![Mirar leases 2 ](/capturaspractica1/capturacomandoipfija2.PNG).

Ahora procederemos a configurar el archivo ```/etc/dhcp/dhcpd.conf``` , bajaremos hasta que encontremos una declaracion parecida a esta.

![Ip fija](/capturaspractica1/ipreservada.PNG).


IMPORTANTE: Cambiar el nombre de host de fantasy , por el que tu quieras.

Reinciamos el servicio con ```service isc-dhcp-server restart```: .

Y ahora en el cliente windows si hacemos los comandos ```ipconfig /release``` y ```ipconfig /renew ``` podremos ver la ip cambiada.

![paso final](/capturaspractica1/exito.PNG).


