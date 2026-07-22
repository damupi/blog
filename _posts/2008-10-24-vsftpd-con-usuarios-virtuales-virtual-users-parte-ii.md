---
title: "VSFTPD con usuarios virtuales (virtual users) parte II"
date: 2008-10-24 11:44:00.003
tags: ["ubuntu", "virtual users", "vsftpd"]
category: professional
---

Antes de leer este post, sepan que trata sobre informatica y puede aburrirles soberanamente. Si esperaban alguna gilipollez, hoy no es mi dia, lo siento. Hoy vamos a terminar como configurar un servidor FTP, en concreto, probablemente el mas seguro de todos VSFTPD con TLS/SSL explicita.  
  
Como dije en mi anterior parte ([PARTE I](http://damupi.blogspot.com/2008/10/vsftpd-con-usuarios-virtuales-virtual.html)), ya solo quedaba actualizar vsftpd y crear el vsftpd.conf  
  
Resulta que mi cliente ftp es FileZilla, el de Firefox, vamos, y al intentar listar el directorio me daba un error. Googleando, encontre que habia un bug para la version que te bajas de los repositorios de ubuntu por lo que hay q actualizarla.  
  
Ya nos bajamos el db\_load v3 de los fuentes, y esto no sera mas complicado.  
  
#cd /tmp  
#wget ftp://vsftpd.beasts.org/users/cevans/vsftpd-2.0.7.tar.gz  
#tar -zxvf vsftpd-2.0.7.tar.gz  
#cd vsftpd-2(...)  
  
instalamos las dependencias para poder compilarlo (hacer el make, vamos)  
#apt-get install libcurl3-openssl-dev libc6-dev libcap-dev libpam0g-dev  
buscamos el fichero "builddefs.sh" a mi el comando q me mola para buscar es:  
#find / -name "builddefs.sh"  
lo editamos  
#vim /tmp/vsftpd-2.0.7/lo\_que\_sea/builddefs.sh  
y añadimos esto al final con la almohadilla incluida (las otras almodillas antes descritas representan linea de comando, en esta ocasion la almohadilla debe escribirse tambien dentro del archivo builddefs.sh)  
  
#define VSF\_BUILD\_SSL  
  
compilamos  
#make  
(nota del autor como gañan que es: si no te funciona el make es pq no estas en el sitio equivocado. Busca dentro de la carpeta descomprimida el archivo make, porfa)  
  
cambia de nombre el archivo vsftpd por otro por si no te funciona la compilacion  
#cp /usr/sbin(o el directorio donde tengas vsftpd)/vsftpd /usr/sbin/vsftpd.original  
y mueve el archivo vsftpd compilado a donde estaba el anterior  
#mv /tmp/vsftpd-2.0.7/vsftpd /usr/sbin/vsftpd  
  
Ahora por ultimo os dejo mi archivo vsftpd.conf , no sin antes deciros que al final he añadido un apartado que empieza por loggin, donde deja en /var/log/vsftpd.log lo que esta pasando, para de esa manera, si no os funciona algo, poder saber por donde arreglarlo.  
  
------  
  
ftpd\_banner="Bienvenido al servidor FTP del curro de damupi"  
anonymous\_enable=NO  
local\_enable=YES  
write\_enable=YES  
anon\_upload\_enable=YES  
anon\_mkdir\_write\_enable=YES  
anon\_other\_write\_enable=NO  
chroot\_local\_user=YES  
guest\_enable=YES  
listen=YES  
listen\_port=1023  
pasv\_min\_port=12000  
pasv\_max\_port=12100  
  
pasv\_enable=YES  
pasv\_address=tu\_ip\_publica  
#pasv\_promiscuous=YES  
#port\_promiscuous=YES  
#port\_enable=NO  
  
local\_umask=077  
pam\_service\_name=vsftpd  
user\_config\_dir=/etc/vsftpd\_user\_conf  
guest\_username=virtual  
  
user\_sub\_token=$USER  
chroot\_local\_user=YES  
hide\_ids=YES  
local\_root=/home/ftpsite/hosts/$USER  
secure\_chroot\_dir=/var/run/vsftpd  
  
#TLS/SSL/FTPS  
ssl\_enable=YES  
rsa\_cert\_file=/etc/vsftpd/vsftpd.pem  
force\_local\_data\_ssl=NO  
  
#Logging  
#te muestra todos los comandos FTP, bueno para debuguear. Necesita xferlog\_enable=YES  
log\_ftp\_protocol=YES  
#Si se pone con xferlog\_enable en vez de meterse en el log de vsftpd se llevado al log del sistema  
syslog\_enable=NO  
xferlog\_enable=YES  
vsftpd\_log\_file=/var/log/vsftpd.log  
  
  
------------  
  
Muchas gracias a todos los que sin saberlo me ayudaron, os dejo sus webs:  
  
<ftp://vsftpd.beasts.org/users/cevans/untar/vsftpd-2.0.7/EXAMPLE>  
  
<http://blog.joshua.net/2006/07/ftps-with-vsftpd-part-1.html>  
  
<http://ubuntuforums.org/showthread.php?t=880724>  
  
<http://howto.gumph.org/content/setup-virtual-users-and-directories-in-vsftpd/>  
  
siento si me ha olvidado alguno, os quiero igualmente.
