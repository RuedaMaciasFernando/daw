### Practica 2 Servicios en red DAW
## Fernando Rueda

## Objetivos

- Conocer el funcionamiento del servicio DNS
- Conocer los distintos registros asocidados a un dominio DNS.
- Conocer cómo suplantar el DNS de forma local.

## Qué entregar

- Toma el fichero markdown del repositorio del profesor y añádelo a tu repositorio de prácticas
- Añade los comentarios y fotos que expliquen y justifiquen tu trabajo.
- Vamos aprovechar esta práctica para aprender a añadir etiquetas en git:
  - https://git-scm.com/book/es/v2/Fundamentos-de-Git-Etiquetado
  - Definie una etiqueta P1 asocida al commit con el que terminas tu trabajo. Puedes hacerlo de una de estas maneras:
    - `git tag -a P2 <hash del commit>`
    - `git tag -a P2 -m 'Práctica 2 acabada'`
  - Sube tu trabajo y la etiqueta:
    - `git push origin P2`  //para subir la etiqueta
    - `git push origin --tags` //para subir todas las etiquetas
- Respeta la fecha de entrega.

### Herramientas de diagnóstico

- Toma un dominio público de acuerdo a tu número de alumno:
  1. gitlab.com
  2. bitbucket.com
  3. android.com
  4. trello.com
  5. atlassian.com
  6. ibercaja.es
  7. facebook.com
  8. twiter.com
  9. bbva.es
  10. instagram.com
  11. wikipedia.com
  12. elpais.com
  13. marca.com
  14. heraldo.es
  15. apple.com

- Intenta resolver las siguientes preguntas con con las 3 herramientas presentadas: host,nslookup y dig.
  - Busca la IP asignada

  ![](capturaspractica2/resolveriptrescomandos.PNG)


  - Quien resuelve su DNS

   ![](capturaspractica2/2apartado.PNG)


  - Cuál es el servidor de correo electrónico. Si hay varios, determina cual es primero por su prioridad.
    - En mi caso no se han encontrado registros de correo electrónico de marca.es . 

    ![](capturaspractica2/3apartado.PNG)
    ![](capturaspractica2/correomarca.PNG)

  - Haz la búsqueda de forma autorizada, es decir, que el servidor que contesta sea uno de los registos NS del dominio.

  ![](capturaspractica2/busquedautorizada.PNG)
  

### Suplantar servicio DNS localmente.

- Edita el fichero `/etc/hosts` para que resuelva un nombre de dominio falso con el siguiente esquema: 
  - `miapellido.local` asociado a la dirección 127.0.0.1
  - `miapellido.es` asociado a una dirección pública inventada.
  - `miapellido.com` asociado a la dirección del dominio de la primera parte de la práctica. Si eres el nº 1 sería `github.com`

   ![](capturaspractica2/2partapracticahosts.PNG)

- Comprueba la resolución de los tres registros con alguna de las herramientas de diagnóstico.

    ![](capturaspractica2/comprobacionapartado2_1.PNG)

- Comprueba la misma resolución pero haciendo que el servidor consultado sea el 8.8.8.8

  - Como podemos apreciar el archivo /etc/hosts es resuelve los nombres de manera local, al hacerlo con el DNS de google no podrá porque no existirán.

 ![](capturaspractica2/comprobacionapartado2_2.PNG)

### Configuración del servidor  DNS con BIND9

- Configura el dominio `miapellido.edu` en bind9. Debes hacerlo:
  - En el servidor linux
  - Conectándote al mismo mediante SSH
  - Utiliza las direcciones IP de tu red privada (192.168.1xx.0/24)
  - Deberás usar un fichero con un nombre similar a este: *db.miapellido.edu*
- Debes definir:
  - Número de versión 1
  - Un correo de administrador dentro del dominio.
  - Dos registros MX.
  - Dos registros NS con máquinas del propio dominio (por ejemplo *dns1* y *dns2*).
  - Varios registros A, al menos: *www, dns1, dns2*
  - Un registro CNAME asociado a la misma dirección que *www*
- Para comprobar que tu configuración es correcta debes:
  - Usar el comando de verificación sintáctica.
  - Reiniciar el servicio
  - Probar la resolución usando nslookup y dig. Ojo, la resolución de nombres debe correr a cargo del servidor Ubuntu.
- Trabajo extra. Para mejorar tu calificación se te propone:
  - Añadir una zona de resolución inversa
  - Instalar Bind9 en el cliente Ubuntu y definirlo como DNS esclavo del primero.

  ### Configuración del servidor  DNS con BIND9 : Capturas y Pasos

   - Primero instalamos bind 9 con `sudo apt install bind9 bind9utils bind9-doc dnsutils -y`

   - Editaremos el archivo de zona con el nombre de la nuestra.

      ![](capturaspractica2/archivozonas.PNG)

   - Ahora crearemos en archivo de zona sudo cp `/etc/bind/db.local /etc/bind/db.rueda.edu` y lo editaremos con sudo nano `/etc/bind/db.rueda.edu`.

   - Añadiremos:
  - Número de versión 1
  - Un correo de administrador dentro del dominio.
  - Dos registros MX.
  - Dos registros NS con máquinas del propio dominio (por ejemplo *dns1* y *dns2*).
  - Varios registros A, al menos: *www, dns1, dns2*
  - Un registro CNAME asociado a la misma dirección que *www*
   
    ![](capturaspractica2/archivopricipalapartado3.PNG)


     - Reiniciamos el servidor con sudo `systemctl restart bind9`.
     - Comprobamos que el archivo esta con la sintaxis correcta `sudo named-checkzone example.com /etc/bind/db.example.com`.

        ![](capturaspractica2/comprobacionservidor.PNG)

      - Como paso final EN EL CLIENTE comprobamos si resuelve los nombres.

        ![](capturaspractica2/exito1.PNG)
        ![](capturaspractica2/exito2.PNG)





