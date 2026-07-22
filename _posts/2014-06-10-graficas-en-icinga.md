---
title: "Graficas en Icinga"
date: 2014-06-10 18:54:00
tags: ["'/var/lib/pnp4nagios/index.php", "apache", "tomcat", "nagios", "pnp4nagios", "icinga", "Apache2", "centos"]
category: professional
---

El otro día me tocó configurar las gráficas en [Icinga](https://www.icinga.org/). Todavía está en beta porque muchas de las máquinas no estan bien configuradas, así como muchos de los servicios. Algunas de ellas están funcionando, y sus servicios también.  
  
Mi jefe queria una gráfica en tiempo real de las máquinas y los servicios monitoreados. En icinga, hay un icono de la gráfica, le daba al mismo y le daba un error. Ahí es donde le tocó a damupi ponerse manos a la obra. La máquina en la que está montado el servidor es un centOS con un apache 2.0 y al que también le tuve que instalar un Tomcat para migrar la intranet que esté hecha en jsp.  
  
Cuando intentaba acceder a las gráficas de icinga le daba un error. Lo primero que hice fue meterme en el error\_log de apache y obtuve el siguiente resultado:  
  
script '/var/lib/pnp4nagios/index.php' not found or unable to stat, referer: http:// [....]  
  
Googleando un poco, las gráficas de icinga se hacen gracias a [pnp4nagios](http://docs.icinga.org/latest/en/icinga-web-pnp.html). Una vez leida la documentacion, revise el archivo de configuracion de apache y vi que había un pnp4nagios. La primera directiva era un alias a pnp4nagios.  
  
Mi instinto me dice que cuando instalaron en la maquina con yum install pnp4nagios se quedó la configuracion por defecto y habia que retocarla. Para ello busqué la carpeta pnp4nagios con el comando:  
  

```
 find / -name 'pnp4nagios'
```

  
  
Y la ruta que apuntaba el alias no estaba apuntando correctamente, por eso, comente con una almohadilla la directiva del alias en apache y lo dejé de la siguiente manera

  
# SAMPLE CONFIG SNIPPETS FOR APACHE WEB SERVER
  

```
 Alias /pnp4nagios "/usr/share/nagios/html/pnp4nagios"  
 #Alias /pnp4nagios "/var/lib/pnp4nagios"  
 
 #  
   AllowOverride None  
   Order allow,deny  
   Allow from all  
   #  
   # Use the same value as defined in nagios.conf  
   #  
   AuthName "Nagios Access"  
   AuthType Basic  
   AuthUserFile /etc/nagios/passwd  
   Require valid-user  
  # Turn on URL rewriting  
  RewriteEngine On  
  Options symLinksIfOwnerMatch  
  # Installation directory  
  RewriteBase /pnp4nagios/  
  # Protect application and system files from being viewed  
  RewriteRule "^(?:application|modules|system)/" - [F]  
  # Allow any files or directories that exist to be displayed directly  
  RewriteCond "%{REQUEST_FILENAME}" !-f  
  RewriteCond "%{REQUEST_FILENAME}" !-d  
  # Rewrite all other URLs to index.php/URL  
  RewriteRule "^.*$" "index.php/$0" [PT]
```

Recargo los módulos de apache con el siguiente comando:  
  
  

```
 service httpd reload
```

  
Y vuelvo a entrar en icinga. Ahora el error es otro: me pide usuario y contraseña. Si revisamos el archivo de arriba veremos esta linea:  
  
 AuthUserFile /etc/nagios/passwd  
  
leemos el fichero:  
  

```
 cat /etc/nagios/passwd
```

  
Aparece un usuario y su clave encriptada. Como la máquina me vino dada, tuve que resetear la contraseña del archivo, para ello utilizamos el comando htpasswd:  
  

```
 htpasspwd -c /etc/nagios/passwd [usuario]
```

  
  
A continuación nos pedira la contraseña y confirmarla de nuevo.  
  
Y ya está.  
  
Fuentes:  
  
<http://docs.icinga.org/latest/en/perfgraphs.html>  
  
<http://andrewpuschak.com/dokuwiki/doku.php?id=installing_pnp4nagios_on_centos>  
  
<http://www.adictosaltrabajo.com/tutoriales/tutoriales.php?pagina=nagios_nagiosql_pnp4nagios>
