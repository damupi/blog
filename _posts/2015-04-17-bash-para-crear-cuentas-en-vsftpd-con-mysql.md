---
title: "Bash para crear cuentas en vsftpd con mysql"
date: 2015-04-17 17:35:00
tags: ["bash", "mysql", "script", "vsftpd"]
category: professional
---

Hace ya bastante tiempo, os enseñe en este blog, como [montar un servidor vsftpd en ubuntu](http://blog.damupi.com/2008/10/vsftpd-con-usuarios-virtuales-virtual.html). Hace bastante tiempo, decidieron actualizarlo a otra maquina, en concreto una CentOS. Además, la empresa quería montarlo con MySQL, para que el mantenimiento de los usuarios de ftp fuese más fácil.  
  
Pues nada, dicho y hecho. La instalación no difiere mucho de [la que ya comente en su momento](http://blog.damupi.com/2008/10/vsftpd-con-usuarios-virtuales-virtual_24.html), una vez instalado este servidor FTP, instalado MySQL, y phpmyadmin, del cual soy superfan, ya solo faltaba incluir los usuarios en la base de datos.  
  
En este ejemplo, los usuarios se encuentran en la carpeta "/var/www/virtual", al estar en la misma máquina la base de datos (bbdd) y el servidor FTP, el host es "localhost", para este ejemplo, los datos de conexion con la bbdd, son usuario: "mi\_usuario", bbdd: "mi\_bbdd" y la tabla donde se almacenan los usuarios: "usuarios". Como no quería que se guardase la contraseña en el script que no iba a estar encriptado, lo que hice fue un sistema para que el bash preguntase por ella y la guardase para la sesion.  
  
Aparte de para darla de alta, tambien sirve para darla de baja, y configurarla. Espero que se entienda el codigo, sino, me comentais en el blog, o me mandais un correo.  
  
Namaste.

```
 #!/bin/sh  
 Fecha=$(date +"%d-%m-%y[%H:%M:%S]")  
 #### Defino los parametros de conexión a la BD mysql  
 sql_host="localhost"  
 slq_usuario="mi_usuario"  
 sql_database="mi_bbdd"  
 echo "Selecciona la opcion que mas se adecua a tus necesidades"  
 echo "1) DAR DE ALTA UNA CUENTA FTP"  
 echo "2) DAR DE BAJA UNA CUENTA FTP"  
 echo "3) PERSONALIZAR PERMISOS DE UNA CUENTA"  
 echo "4) salir"  
 read opcion;  
 if [ -z ${opcion} ]  
 then  
      echo "No has introducido un parametro correcto"  
      echo "Se sale del programa"  
      exit  
 fi  
 echo "Escribe la contraseña del usuario ftp de MySQL y pulsa [ENTER]:"  
 read DBPASS  
 if [ -z ${DBPASS} ]  
 then  
      echo "No has introducido la clave de vsftpd de MySQL"  
      echo "Se sale del programa"  
      exit  
 fi  
 case $opcion in  
 # /********************************************  
 #      DAR DE ALTA UNA CUENTA  
 # /********************************************/  
 1)   
 echo "Escribe en el nombre de la cuenta ftp que quieres crear y pulsa [ENTER]:"  
 read EMPRESA  
 if [ -z ${EMPRESA} ];  
 then  
      echo "No has introducido valor para el nombre de la cuenta"  
      echo "Se sale del programa"  
      exit  
 fi  
 echo "Escribe la contraseña de esta cuenta ftp y pulsa [ENTER]:"  
 read PASS  
 if [ -z ${PASS} ];  
 then  
      echo "Has dejado la contraseña en blanco"  
      echo "Se sale del programa"  
      exit  
 fi  
 echo "Creamos una carpeta en donde queremos que el usuario envie/reciba los ficheros"  
 if [ -d /var/www/virtual/${EMPRESA} ];  
 then  
      echo "No se ha podido crear la carpeta en /var/www/virtual/${EMPRESA} porque ya existia."  
 else  
      mkdir /var/www/virtual/${EMPRESA}  
      mkdir /var/www/virtual/${EMPRESA}/carpeta1  
      mkdir /var/www/virtual/${EMPRESA}/carpeta2  
      #Otorgamos permisos adecuados en las carpetas  
      chown -R applftp:applftp /var/www/virtual/${EMPRESA}  
 fi  
 #comprobamos que el usuario no existe  
 RESULTADO=`mysql -h ${sql_host} -u ${slq_usuario} -p${DBPASS} -e"select count(nrregistro) from vsftpd.usuarios where nombre='${EMPRESA}'" | grep -e 0`   
 echo $RESULTADO  
 if [ "$RESULTADO" == "0" ];  
 then  
      echo -e "Añadimos el registro"  
      mysql -h ${sql_host} -u ${slq_usuario} -p${DBPASS} -e "INSERT INTO vsftpd.usuarios (nombre,contrasena,alta,estado ) VALUES ('${EMPRESA}', '${PASS}',NOW(),'0')"  
      echo "salimos de mysql"       
 else  
      echo -e "Ya existe ese usuario. Se procede a reactivarlo y cambiarle la contraseña"  
      mysql -h ${sql_host} -u ${slq_usuario} -p${DBPASS} -e "UPDATE vsftpd.usuarios SET estado=0, contrasena='${PASS}', tipo=CONCAT('reactivado ',NOW()) WHERE nombre='${EMPRESA}'"  
 fi  
 echo "Los datos del servidor son los siguientes:"  
 echo "url: ftp://ftp.tuempresa.com:2021"  
 echo ""   
 echo "Puerto de escucha: 2021"  
 echo ""  
 echo "Tipo de conexion: pasiva"  
 echo "Tipo de servidor: FTPES (FTP sobre TLS/SSL explicito)"  
 echo "Tipo de transferencia: Binaria"  
 echo ""   
 echo "usuario: ${EMPRESA}"  
 echo "password: ${PASS}"  
 ;;  
 # /********************************************  
 #      DAR DE BAJA UNA CUENTA  
 # /********************************************/  
 2)  
 #desactivamos la cuenta en MySQL cambiando el estado  
 echo "Escribe en el nombre de la cuenta ftp que quieres dar de baja y pulsa [ENTER]:"  
 read EMPRESA  
 if [ -z ${EMPRESA} ];  
 then  
      echo "No has introducido valor para el nombre de la cuenta"  
      echo "Se sale del programa"  
      exit  
 fi  
 echo -e "Se procede a desactivar el usuario en la BBDD"  
 RESULTADO=`mysql -h ${sql_host} -u ${slq_usuario} -p${DBPASS} -e "select count(nrregistro) from vsftpd.usuarios where nombre='${EMPRESA}'" | grep -e 0`   
 echo $RESULTADO  
 if [ "$RESULTADO" == "0" ];  
 then  
      echo -e "No se puede dar de baja un usuario que no existia. Se sale del programa"  
      exit  
 else  
      echo -e "Se procede a desactivar el usuario"  
      mysql -h ${sql_host} -u ${slq_usuario} -p${DBPASS} -e "UPDATE vsftpd.usuarios SET estado=1,baja=NOW() WHERE nombre='${EMPRESA}'"  
 fi  
 #si tenia configuracion personalizada, la borramos  
 if [ -f "/etc/vsftpd/usuarios/"${EMPRESA} ]  
 then  
      echo -e "Borrada la configuracion personalizada de este usuario"  
      rm -rf /etc/vsftpd/usuarios/${EMPRESA}  
 else  
      echo "no tenia configuracion personalizada"  
 fi  
 echo "salimos de mysql"  
 #comprimimos los archivos de la carpeta y los borramos  
 if [ -d /var/www/virtual/${EMPRESA} ];  
 then  
      tar -czvf /var/www/virtual/${Fecha}_${EMPRESA}.tar.gz /var/www/virtual/${EMPRESA}  
      mv /var/www/virtual/${Fecha}_${EMPRESA}.tar.gz /etc/vsftpd/usuarios/bajas/  
      rm -rf /var/www/virtual/${EMPRESA}  
      if [ -d /etc/vsftpd/usuarios/${EMPRESA} ];  
      then  
           mv /etc/vsftpd/usuarios/${EMPRESA} /etc/vsftpd/usuarios/___${EMPRESA}  
      else  
           echo "No existia archivo de configuracion personalizado"  
      fi  
 else  
      echo "La carpeta /etc/vsftpd/usuarios/${EMPRESA} no existia. No se ha podido comprimir"   
 fi  
 ;;  
 # /********************************************  
 #      CONFIGURAR CUENTA  
 # /********************************************/  
 3)  
 echo -n "Deseas que la cuenta tenga permisos para borrar? [si o no]: "  
 read yno  
 case $yno in  
     [sS] | [Ss][iI] )  
         echo "Si"  
         echo "Escribe en el nombre de la cuenta para la que quieres personalizar los permisos [ENTER]:"  
           read EMPRESA  
           if [ -z ${EMPRESA} ]  
           then  
                echo "No has introducido valor para el nombre de la cuenta"  
                echo "Se sale del programa"  
                exit  
           fi  
         if [ -d /etc/vsftpd/usuarios/${EMPRESA} ];  
           then  
                echo -e "Ese usuario ya tenia personalizados los permisos. Se sale del programa"  
                exit  
           else  
                echo "write_enable=YES" >> /etc/vsftpd/usuarios/${EMPRESA}  
                echo "anon_upload_enable=YES" >> /etc/vsftpd/usuarios/${EMPRESA}  
                echo "anon_other_write_enable=YES" >> /etc/vsftpd/usuarios/${EMPRESA}  
                echo "local_umask=777" >> /etc/vsftpd/usuarios/${EMPRESA}  
                echo -n "Quieres que entre en otro directorio? [si or no]: "  
                read sino  
                case $sino in  
                     [sS] | [Ss][iI])  
                          echo "Si"  
                          echo "Escribe en el nombre de la cuenta donde quieres que se conecte el usuario [ENTER]:"  
                          read USUARIO  
                          if [ -z ${USUARIO} ]  
                          then  
                               echo "No has introducido el nombre de la cuenta"  
                               echo "Se sale del programa"  
                               exit  
                          fi  
                          if [ -f /etc/vsftpd/usuarios/${EMPRESA} ];  
                          then  
                               echo "local_root=/var/www/virtual/${USUARIO}/" >> /etc/vsftpd/usuarios/${EMPRESA}  
                          else  
                               echo "no lo he podido cambiar"  
                          exit  
                          fi  
                          ;;  
                     [nN] | [nN][Oo])  
                          echo "No. Se sale de la aplicacion.";  
                          exit  
                          ;;  
                     *) echo "Invalid input"  
                       ;;  
                esac  
           fi  
         ;;  
     [nN] | [Nn][Oo] )  
         echo "Not agreed, you can't proceed the installation";  
         exit 1  
         ;;  
     *) echo "Invalid input"  
       ;;  
 esac  
 ;;  
 ## /********************************************  
 ##      SALIR DEL PROGRAMA  
 ## /********************************************/  
 #  
 *) echo "Se sale del programa"  
 exit  
 ;;  
 esac
```
