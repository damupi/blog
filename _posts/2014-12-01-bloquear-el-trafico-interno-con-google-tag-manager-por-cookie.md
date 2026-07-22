---
title: "Bloquear el tráfico interno con Google Tag Manager por cookie"
date: 2014-12-01 16:17:00
tags: ["analytics", "gtm", "excluir", "google tag manager", "cookie", "filtrar"]
category: professional
---

Este artículo trata de cómo filtrar el tráfico interno a
través de una cookie en [Google Tag Manager](https://www.google.com/tagmanager/).

Los que nos hemos puesto a analizar
sitios web, queremos que nuestros datos sean lo más fieles posibles a la
realidad, pero a veces, no es posible por muchos motivos. Por ejemplo, nosotros
mismos entramos en el site para hacer pruebas y se quedan registradas nuestras
visitas en analytics, o cuando los empleados de la empresa entran en la web, o
cuando vemos el site desde otro sitio, etc…

Hasta ahora conocía la manera de filtrar el tráfico por IP, de
manera que, por ejemplo, si toda la empresa, se conecta a internet a través de
una IP, la excluíamos y así no quedaban registradas las visitas. Pero, ¿y qué
pasa, por ejemplo, cuando nos conectamos desde otros dispositivos con 3G como un Tablet,
nuestro teléfono, o cuando estamos en nuestra casa con nuestro portátil? Para eso podemos excluir el
tráfico añadiendo una cookie a través de Google Tag Manager.

Todo empezó hace 15 días más o menos, cuando asistí a clase magistral de
[Xavier Colomés](https://twitter.com/xavier_colomes), en el master de analítica web de [Kschool](http://kschool.com/). Xavier nos estaba
explicando un site y al entrar en él, dijo de pasada que analytics no
registraba las visitas de su equipo porque tenía una cookie.

Desde ese momento empecé a darle vueltas a la cabeza para ver cómo
hacerlo; y ese mismo día por la noche, lo configuré en un site que tengo de
pruebas con Universal Analytics. Sin embargo, la cosa no quedó ahí, porque después
lo que habló [Rubén Gallardo](http://www.rubengallardo.com/), me di cuenta de que debía de ir más allá y
configurarlo con Google Tag Manager. Pues bien, esta es la forma en la que lo
he hecho.

## Creamos una nueva etiqueta en Google Tag Manager

[![](/img/gtm1.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiwQGPpN8z1g-lupFGvJwafaatCo553iBIP-Dz77gaLrcO8z35kGX0rVKX8_Ufq-JViaQrjStiyaaZuCj8NS1Mj0apzGyVfBH9XantPgw7zLbKzANkeh_uv9F5U6T0Tgv3_SMGn/s1600/gtm1.png)

[![](/img/gtm2.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhYrZjJjuwx5J12LlxIMs165xrUpom9P4d6r147Cjyyt3JSrD9gsdhHfh-ul9QLlIDvXdaiiZDKdr6szIJMe6qrw1THiHmwjmjazs5yBSx6ugIhMG_ta7eCCciJTmXT7lS2BEPx/s1600/gtm2.png)

  

En mi caso, la etiqueta lleva el
nombre “generar\_cookie\_tipo\_visitante” y lo que hacemos es añadir código HTML
personalizado que crea una cookie con el nombre “tipo\_visitante” con el valor
1. Vosotros podéis llamar a la cookie como queráis y decidir el valor
que le queráis poner. En mi caso elegí que la cookie se llamaria tipo\_visitante, y el valor que identificaría el tráfico interno sería el 1

El código HTML es el siguiente:

```
 <script type="text/javascript">  
 function createCookie(name,value,days) {  
    // Funcion para crear la cookie  
   if (days) {  
     var date = new Date();  
     date.setTime(date.getTime()+(days*24*60*60*1000));  
     var expires = "; expires="+date.toGMTString();  
   }  
   else var expires = "";  
   document.cookie = name+"="+value+expires+"; path=/";  
 }  
 // Generar and fijar la cookie tipo_visitante con valor 1 que expira al año  
 createCookie("tipo_visitante", "1", 365);  
 </script>
```

  

## Creamos una macro que lea la variable de la URL

Como no queremos molestar a nuestro departamento de IT, para
que nos haga una página con el código, lo que haremos será que cuando nos
conectemos a la  URL <http://damupi.com/?tipo_visitante=1>
nos guarde la cookie "tipo\_visitante", con el valor 1. Para hacerlo, creamos
una macro, “URL consulta tipo visitante es 1” en mi caso, de tipo “URL” y de
tipo de Componente “Consulta”

[![](/img/gtm3.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgmiGV8M6tutF3Q0NBUNpGcvcEF_FuSm5wOTAvIB28uSYzWoadMn8dCXGfaxkuELwamy3GSH7ev-HoyWkEn7k_PXf1ieffZjMDFhMWx6Ub8ANg6dFiXEm7T9Z1jEr4ytldWhIZy/s1600/gtm3.png)

  
  

[![](/img/gtm4.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhs8_bzV5r0qXAku22SbFYROTM8bhNwDmcxlx1fZ-nLBj80FZ5FU3h-Id6dHuo29c8vM5q7xFqzPmsEIqUeujwTKEBMcF2elBm0f4GYTtPBKmFeP-NqnnlfnQf0-sKX2g88E6p6/s1600/gtm4.png)

## Creamos una regla para que se lance la macro anterior

A esta regla le voy a llamar “URL de consulta tipo\_visitante
es 1”  y lo que hace es comprobar que el
valor que le hemos dado en la variable tipo\_visitante de la URL <http://damupi.com/?tipo_visitante=1>
es igual a 1

[![](/img/gtm5.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiohi0jWK291n4D7O4WWtwMU58XizXI0XG0n9XnnGRXOK16YTFDmPIjMtZA8Yvo7n8JtJK-6kgO7dB_nV4EAtr0OaSEoJ73mM0LY1mWpQ6uFjP2GfnMfbXe_mjinlT2FDeuIrZz/s1600/gtm5.png)

## Añadimos la regla a la etiqueta que nos crea la cookie

Nos vamos a la etiqueta que creamos en el punto 1 y que
llamé “generar\_cookie\_tipo\_visitante” y le decimos que queremos que lo haga
cuando se cumpla la regla que creamos antes. Es decir, queremos que nos cree la
cookie que le hemos dicho, cuando pongamos en la barra de direcciones <http://damupi.com/?tipo_visitante=1>

[![](/img/gtm6.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg7M-bTYJmA80B_kaYs80_xkxB8vbzjT8PvBXNp_8XOJROD1JzTkKhcfn_fGfoS1kI7MEOI-JaH_aEl_xkDmbFDMW2TB669ClDpNtznvfIF8cAMdK-B9v3nbjnA6hegv0mk8fuK/s1600/gtm6.png)

  

[![](/img/gtm7.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgmIT4s2yR8-7Rj4d_xsa3_JDgDfDmawHI9JDMS18ai0bey1a2YZZa3kHmSsCg91Ida9zzGP5ntGJBcAO4OvvmBb1oyQkEbGpHHS9UAa_7FZCKCH2JBLsMv1QO5nzAZ36om6K_R/s1600/gtm7.png)

## Añadimos una regla de bloqueo a nuestra etiqueta de analytics

Lo que ahora queremos hacer, es que analytics no nos cuente como
visita cuando entremos en la URL <http://damupi.com/?tipo_visitante=1>,
para ello, añadimos como regla de bloqueo, la que ya creamos en el punto 3, “URL
de consulta tipo\_visitante es 1”, para que analytics no nos lance su código.

[![](/img/gtm8.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiL6tsm9heF6fts8C_ecJ3FHXOvJqzyDAE-N0lQ-oo833TuWz491fND0iexUB0Q03buCd7MKQNfi6Af-0Yi83ed6gFfjzeSXaVllVGgLx7p0MiumpmcVc6ZkjLGHP2m_wY0t26P/s1600/gtm8.png)

  

[![](/img/gtm9.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgKHGoVTB3QF9Dg7Vc8At3T4XI8sjd1sFSEME2tSDFBQxMnn-8Ouc0O7VpZKSo8Ygbh8sISpTo7_-W_kK-k8eRItox7RuIMZNYX88kd68fHzKQls2yet9xCy8ZsQ4kzzMZE2Htq/s1600/gtm9.png)

## Añadimos una macro que se encargue de leer el valor de la cookie

El siguiente paso es leer el valor de la cookie que creamos
en el punto 1, para que, si entramos en nuestro dominio, google tag manager lea
nuestra cookie y sepa que no nos tiene que contar como visita. Para ello
creamos una macro con el nombre “cookie tipo\_visitante” y nombre de la cookie “tipo\_visitante”
que es como la hemos llamado.  
  

[![](/img/gtm10.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiho7lyzk9a4EpnYKKU2bNACXzV9o_waE8DLSzQa9VMD6o4oqFsIM-cC5rFOL9_SwjrFB_xRGG6b8knJspxjvGg8HT7D9NtyM8EH6J6SS07ThidoXkclDflxcfIbuWwbMJupcHP/s1600/gtm10.png)

  
  
  

## Creamos una regla para que compruebe el valor de la regla

  

[![](/img/gtm11.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiSBbxfvWNS2_cIpIKQC9Z6ROan7rut5pipcVA5iCXBquLXYnS2dhnYaY_UXXx8l5w9rIx5U9wA1bhaMODBm1hivKzuHIahyTbXnE0kufz3Yf3LHedG5beT4Ki06sQ4YzE39Ma3/s1600/gtm11.png)

## Añadimos una regla, que nos verifique si el valor de la cookie “tipo\_visitante” es igual a 1

[![](/img/gtm11.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiSBbxfvWNS2_cIpIKQC9Z6ROan7rut5pipcVA5iCXBquLXYnS2dhnYaY_UXXx8l5w9rIx5U9wA1bhaMODBm1hivKzuHIahyTbXnE0kufz3Yf3LHedG5beT4Ki06sQ4YzE39Ma3/s1600/gtm11.png)

## Añadimos la regla de bloqueo que comprueba el valor de la cookie

Por ultimo, como queremos navegar por todo el site sin que
nos cuente como visita, tenemos que decirle que si detecta en cualquier lado
por el que naveguemos, que tenemos una cookie llamada “tipo\_visitante” con el
valor 1, no nos escriba el código de analytics y por lo tanto no nos cuente
como visita.

[![](/img/gtm12.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgiVwjWvU07A95vgTWWweD6OJyKE6UCarPEwerVYvfMAL_x-eCJJGPSTMUPuGgGYTydzxKe7YlCdy-larRykntkIxMJSSzzAgvRlKYtqvrcsoc85isnsuKek8ueAMODCoyGZGuK/s1600/gtm12.png)

  

[![](/img/gtm13.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgPMG93u57BUZe9R3OCC-yUHKW4ZKj01q6ULmp1xbHJgGDd-2UZqeF27OEvCk_RWp5zQ_aqXyPAcZDw8pXNkjbmFURGG3fL0WBUj3XNkYZfwSG2cXppxSllaFtWF0N5lSpE_Scd/s1600/gtm13.png)

Ya está, ahora lo primero que tenemos que hacer si no queremos que analytics nos cuente como una visita en nuestro sitio web, es poner en la barra de direcciones http://damupi.com?tipo\_visitante=1  
  
Namaste.  
  
------------  
  

### Agradecimientos:

  
Todo este artículo no hubiese sido posible sin, aparte de los profesores que me inspiraron y me ilusionaron para hacerlo, a los siguientes recursos:  
  
[Blog de Simo Ahava](http://www.simoahava.com/analytics/block-internal-traffic-gtm/)  
[Lunametrics](http://www.lunametrics.com/blog/2014/03/11/goodbye-to-exclude-filters-google-analytics/#sr=tjnpbibwb.dpn&m=r&cp=(sfgfssbm)&ct=/bobmzujdt/cmpdl-joufsobm-usbggjd-hun/-tmc&ts=1414511740)  
[Guia de macros de Aureka](http://www.aukera.es/blog/guia-macros-google-tag-manager/#macro-cookie)
