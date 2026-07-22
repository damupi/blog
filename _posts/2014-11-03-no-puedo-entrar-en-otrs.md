---
title: "No puedo entrar en OTRS"
date: 2014-11-03 15:39:00.001
tags: ["mysql", "otrs", "apache", "Apache2"]
category: professional
---

Esta mañana nuestro proveedor ha estado haciendo "mejoras" en los sistemas y nos ha dejado sin OTRS. (entiéndanse de ahí las comillas a llamar mejoras a algo que hace que luego los aplicativos no funcionen).  
  
Olvidando el tema de la gestión de estas mejoras, en que lo suyo hubiese sido parar los servicios del aplicativo, esto es, parar MYSQL y despues Apache, resulta que intentando hacer login, no se podía. Me explicare: ponias tanto como agente, como usuario, tu nombre de usuario y contraseña y no validaba y se borraban los datos que habias puesto.  
  

[![](/img/gancho.jpg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEi8E3jjJVFqxvzwsTMwBYnM8vWLu4mOC69gwupTtG-RKCBSB-1G_4WjLwIVQiLPuML7TZG2UuOBYAnqdfdNqIgJBcNnSlKJ9iDu0QhnfqFCDkC9ZRmTikWWms98YfVF7INSGSw5/s1600/gancho.jpg)

  
  
Qué es lo primero que debemos de hacer? Exacto !!! Ver los logs. Os acordais de esos juguetes de tres ojos de Toy Story que decían "el gancho es nuestro amo", pues para toda persona que trabaje en IT, los logs son nuestros amos. En ellos podemos obtener toda la información que necesitamos.  
  
En este caso el log de apache nos decia lo siguiente:  
  
"-e: DBD::mysql::db do failed: Table '.\otrs\sessions' is marked as crashed and should be repaired at ..."  
  
"'.\otrs\sessions' is marked as crashed and should be repaired, SQL: 'INSERT INTO sessions  (session\_id, session\_value) VALUES (?, ?)'"  
  
googleando un poco me encuentro el [siguiente post](http://forums.otterhub.org/viewtopic.php?f=62&t=12064) que nos da la solucion:  
  
**mysql> repair table sessions;**  
  
  

Ejecutado el siguiente script, pudimos inciar sesión sin problemas ;)
