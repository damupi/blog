---
title: "Plsql en w7 64 bits y el error oci.dll"
date: 2011-02-26 17:21:00.009
tags: ["tnsnames.ora", "oci.dll", "32 bits", "plsql"]
category: professional
---

-----  
  
PROLOGO: Si eres asiduo lector de este blog, seguramente este post no te gustara, pero a veces, damupi, se siente en la obligacion de devolverle a la comunidad internet, las cosas que aprende.  
  
----  
  
El pasado lunes, llegue tan contento al curro (o fabrica) y me encontre con un maravilloso, ironicamente hablando, pantallazo azul pq se rompio el disco duro. Me toco ponerme manos a la obra y empezar de cero. Como el ordenador me lo permitia, le puse un Windows 7 de 64 bits.  
  
Entre las aplicaciones que utilizo, esta el PL/SQL Developer, pero para hacer 2 querys basicas y poco mas. Mis compañeros utilizan forms y TOAD y cosas mas complicadas, pero a mi con este programa para atacar a la base de datos, me basta.  
  
[Deck](http://deckerix.com) me dijo de montarme el cliente de 64 bits y despues arrancar plsqldev.exe como si fuera portable pero me salia un error diciendome que me faltaba el archivo oci.dll y que si tenia un cliente de 32bits.  
  
Al final lei [este articulo](http://xp-rience.blogspot.com/2009/10/toad-97-oracle-instant-client-111.html) y sali de dudas. Basicamente has de seguir lo pasos siguientes:  
  
1- Crear unas carpetas en unos sitios determinados.  
2- Bajarte el cliente instantaneo de Oracle (Oracle instant client) y ponerlo en la carpeta determinada.  
3- Crear variables de entorno en para el usuario.  
4- Configurar tu tnsnames.ora y tu sqlnet.ora  
5- Ejecutar PL/SQL Developer (plsqldev.exe)  
  
1.- Creacion de carpetas  
  
Como me dio el error la primera vez, diciendo que necesitaba un cliente de 32 bits y la instalacion del cliente normal no permite hacerlo en un directorio que contenga parentesis, cree las siguientes carpetas:  
  
C:\Program Files (x86)\oracle  
C:\Program Files (x86)\oracle\bin  
C:\Program Files (x86)\oracle\network  
C:\Program Files (x86)\oracle\network\admin  
  
2.- Bajarse instant client de Oracle de 32 bits.  
  
[Bajarse Oracle Instant Client](http://www.oracle.com/technetwork/database/features/instant-client/index-097480.html).  
  
Sino [lo buscamos en google](http://lmgtfy.com/?q=instant+client+downloads) por si ha cambiado la url  
  
Nota: yo me baje la de 32 bits ( Instant Client for Microsoft Windows (32-bit) )  
  
Una vez que nos hemos bajado el instant client de 32 bit, copiamos el contenido, es decir, todos los archivos en la carpeta "C:\Program Files (x86)\oracle\bin"  
  
3.- Crear las variables de entorno  
  
Para crear variables de entorno en windows 7 os remito de nuevo a la pagina de donde saque esto. [Pulsa aqui](http://xp-rience.blogspot.com/2009/10/toad-97-oracle-instant-client-111.html)  
  
Se crean las siguientes variables de entorno:  
  
LD\_LIBRARY\_PATH = C:\Program Files (x86)\oracle\bin  
ORACLE\_HOME = C:\Program Files (x86)\oracle  
ORACLE\_HOME\_NAME = C:\Program Files (x86)\oracle  
SQL\_PATH = C:\Program Files (x86)\oracle  
TNS\_ADMIN = C:\Program Files (x86)\oracle\network\admin  
  
4- Configurar tu tnsnames.ora y tu sqlnet.ora  
  
Una vez tenemos configuradas nuestras varibles de entorno, solo nos queda poner nuestros tnsnames.ora y sqlnet.ora en la carpeta "C:\Program Files (x86)\oracle\network\admin"  
  
5- Ejecutar PL/SQL Developer (plsqldev.exe)  
  
Yo, para ordenarme todo el software de oracle, me cree otra carpeta "C:\Program Files (x86)\oracle\software" y meti pl/sql developer en su carpeta, quedando asi: "C:\Program Files (x86)\oracle\network\PLSQL Developer"  
  
Dentro de esta carpeta, ejecutamos plsqldev.exe y a correr.  
  
Espero os haya sido de ayuda.   
  
Gracias
