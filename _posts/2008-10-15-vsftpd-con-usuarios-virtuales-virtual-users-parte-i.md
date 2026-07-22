---
title: "VSFTPD con usuarios virtuales (virtual users) parte I"
date: 2008-10-15 13:07:00.016
tags: ["ubuntu", "virtual users", "vsftpd"]
category: professional
---

Este post no es apto para gente q no le gusten los ordenadores. No voy a hablar de mis mierdas esta vez. Este post trata de como configurar un servicio FTP, en concreto vsftpd, en una maquina con ubuntu server. La configuracion del vsftpd es con usuarios virtuales.  
  
Ante todo deciros que yo soy un mierda, no he estudiado informatica y todo lo que se me lo dio internet. Tambien debo dar las gracias a deck que me ayudo a configurarlo. De paso gracias a mi jefe por depositar su confianza en mi para poder hacerlo (DBA para cuando?).  
  
Resulta que la empresa en la que trabajo necesitaba montar un servidor FTP para que una entidad financiera nos mandase unos ficheros. Esta entidad financiera necesitaba un servidor FTP con un tipo de conexion pasiva. Para diferencias entre lo que es una u otra conexion pinchad en este [link](http://www.slacksite.com/other/ftp.html). Basicamente o por lo que yo he entendido en una conexion pasiva el cliente es el que decide por que puertos se comunica y en una conexion activa es el servidor por que puertos se va a comunicar con el cliente. Que me corrijan en los comentarios, por favor, si me equivoco. Tambien tengo entendido, en lo que a puertos se refiere que exite un puerto por el que se mandan los comandos y hay otro u otros por los que se envian los datos.  
  
Dicho lo dicho, me cojo una distro -ditribucion para los legos-de linux. Yo como soy muy cool a la par que gilipollas (e hijo de puta para algunos, jajaja) me cojo la ubuntu server que es superfacil de montar y esta de moda. :P  
  
Nos bajamos la distro de la [web](http://www.ubuntu.com) de la parte que pone get ubuntu o download o yo q coño se. Cogemos el ordenata, hacemos que arranque con el CD y....SIGUIENTE SIGUIENTE SIGUIENTE. Esta distribucion nos ofrece la posibilidad de montar unos tipos de servidor por defecto: DNS, LAMP, MAIL y no se que coño mas. Para nuestra desgracia o fortuna (en mi caso fortuna pq asi aprendo mas) no aparece FTP server, asi que toca currarselo.  
  
Lo que a partir de ahora os pongo son extractos de unas paginas, cacharreos propios y ayudas de deck.  
  
Lo primero, instalar el ssh para entrar desde consola, aparte de necesitar una conexion segura para servir los ficheros:  
  
#aptitude install openssh-server openssh-client  
  
#/etc/init.d/ssh start  
  
Ahora instalamos el vsftpd  
  
#aptitude install vsftpd  
  
Creamos la carpeta de donde colgaran los usuarios. En mi caso la estructura es esta:  
  
/home/ftpsite/hosts  
  
Luego tenemos:  
  
/home/ftpsite/hosts/usuario1  
/home/ftpsite/hosts/usuario2  
  
Se que podia haber quitado lo de hosts o poner host1, host2, no se...es como lo hice.  
  
asi que  
  
#mkdir /home/ftpsite/  
  
#mkdir /home/ftpsite/hosts  
  
#mkdir /home/ftpsite/usuario1  
  
#mkdir /home/ftpsite/usuario2  
  
Deciros que soy un autentico gañan y me pierdo con los . y los .., asi que paso y pongo toda la ruta, a fin de cuentas es tabular, no?  
  
Para autentificar todos los usuarios utilizamos PAM. Vamos a crear un fichero en texto plano (.txt) y creamos una base de datos encriptada (.db). El quid de la cuestion no es crear el texto plano pq con cualquier editor de texto lo hacemos, el tema es encriptar la base de datos.  
  
ATENCION !!!! no podemos bajarnos cualquier version para hacer la base de datos (db\_load), para vsftpd necesitamos la version 3. nos ponemos en el fichero /tmp y bajamos la version 3 para crear bbdd.  
  
#cd /tmp  
  
#wget http://download.oracle.com/berkeley-db/db-3.2.9.tar.gz  
  
descomprimimos el archivo comprimido:  
#tar -zxvf db-3.2.9.tar.gz  
nos metemos en la carpeta descomprimida  
#cd db-3.2.9  
y ahora en el carpeta que lleva el make  
#cd dist  
  
Para poder hacer un make necesitamos las librerias gcc o instaladores o compiladores o lo que coño sea  
  
#apt-get install gcc  
#apt-get install libc6-dev g++ gcc  
#apt-get install make  
  
Ahora en /tmp/db-3.2.9/dist configuramos y ejecutamos el make  
#configure  
#make  
#make install  
  
#echo '/usr/local/berkeleydb/lib/' >> /etc/ld.so.conf  
#ldconfig  
  
hacemos una copaia de nuestra autentificaion vsftpd via PAM  
#cp /etc/pam.d/vsftpd /etc/pam.d/vsftpd.original  
y la editamos  
#vim /etc/pam.d/vsftpd  
  
su contenido:  
  
auth required /lib/security/pam\_userdb.so db=/etc/vsftpd/vsftpd\_login  
account required /lib/security/pam\_userdb.so db=/etc/vsftpd/vsftpd\_login  
  
ahora creamos el fichero que tiene los usuarios con su password:  
#vim /etc/vsftpd/logins.txt  
  
usuario1  
password1  
usuario2  
password2  
  
ahora creamos la base de datos con la berkeley que nos hemos instalado antes, mediante encriptacion hash (es un codigo, no es un peta, lo siento)  
  
#/usr/local/berkeleydb/bin/db\_load –T –t hash –f /etc/vsftpd/logins.txt /etc/vsftpd/vsftpd\_login.db  
  
ponemos los permisos necesarios para que no lleguen a manos ajentas  
  
#chmod 600 /etc/vsftpd/vsftpd\_login.db  
#chmod 600 logins.txt  
  
Nos creamos un certificado tope guay para que vean lo segura que es nuestra comunicacion TLS/SSL explicita  
  
(si no tenemos openssl:  
#apt-get install openssl  
)  
  
#openssl req -new -x509 -nodes -out vsftpd.pem -keyout vsftpd.pem   
#cp vsftpd.pem /etc/vsftpd  
  
Ya solo nos queda actualizar vsftpd y editar el fichero de configuracion vsftpd.conf  
  
[PARTE II](http://damupi.blogspot.com/2008/10/vsftpd-con-usuarios-virtuales-virtual_24.html)
