---
title: "Apache2/SSL y host virtuales basandos en el nombre - Apache2/SSL and Name Based Virtual Hosts"
date: 2012-04-21 15:47:00
tags: ["Virtual Hosts", "VirtualHost", "443", "SSL", "Apache2"]
category: professional
---

En la [#fabricadechocholate](http://twitter.com/#%21/search/realtime/%23fabricadechocolate) tenemos alojados varios dominios, por ejemplo:  
  
- fabricadechocolate.com  
- chocolatitosblog.com  
  
Resulta que tenemos funcionando un apache2 con [virtualhost](http://httpd.apache.org/docs/2.0/vhosts/) "rulando" para todos los dominios de la [#fabricadechocolate](http://twitter.com/#%21/search/realtime/%23fabricadechocolate). El problema viene porque tenemos un certificado para fabricadechocolate.com de manera que si la gente quiere comprar cosas a la [#fabricadechocolate](http://twitter.com/#%21/search/realtime/%23fabricadechocolate) lo puede hacer mediante una conexion encriptada, esto es https://www.fabricadechocolate.com  
  
Hasta aqui todo bien hasta que nos llegan los de marketing y nos dicen: oye, si te metes en https://www.chocolatitosblog.com te lleva a https://www.fabricadechocolate.com  
  
[Deckerix](http://www.deckerix.com/) y damupi cuando nos pusimos con el apache y sus respectivos certificados, lo configuramos solo para fabricadechocolate.com porque por aquel entonces solo albergabamos un dominio y como todo en la fabricadechocolate todo lo hacemos sin prevision y a marchas forzadas. Asi que configurarmos el certificado y la conexion cifrada SSL y nos quedamos tan a gusto.  
  
Pero ahora los de marketing se han dado cuenta que nuestro apache 2, cuando lee por el puerto 443, SSL o lo te conectas por el protocolo https te lleva al dominio que creamos por defecto, esto es, fabricadechocolate.com.  
  
Bueno, con independencia de que me tocara mañana tocar el servidor web de produccion, ya que no tenemos uno de desarrollo, "a pelo" como el que dice empiezo a leer de esta URL que no se pueden albergar mas de un host virtual en SSL en la misma IP y en el mismo puerto ([mirar la URL](http://wiki.apache.org/httpd/NameBasedSSLVHosts)).  
  
Este articulo tiene fecha 2011-06-16 12:12:14, sigo googleando y encuentro el siguiente [articulo](http://en.gentoo-wiki.com/wiki/Apache2/SSL_and_Name_Based_Virtual_Hosts) que dice que lo dicho en el anterior parrafo es historia.  
  
Para ello hay que ver la version de apache 2 que se tiene, y que modulos se tienen activados. Mañana veremos como acaba la historia en la fabrica.  
  
PD: Legionela, no me digas que es sencillo, pq si asi fuese ya lo hubieras hecho tu
