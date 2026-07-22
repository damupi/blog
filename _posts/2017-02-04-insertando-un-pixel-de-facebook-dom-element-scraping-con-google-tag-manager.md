---
title: "Insertando un pixel de Facebook: DOM element scraping con Google Tag Manager "
date: 2017-02-04 12:42:00
tags: ["gtm", "search", "scraping", "DOM", "google tag manager", "facebook", "pixel"]
category: professional
render_with_liquid: false
---

A cuantos de nosotros no nos ha pasado que nos llegue un compi de marketing y nos diga que de la noche a la mañana - y si fuese posible para ayer - van a hacer una campaña de marketing superideal y que necesitan etiquetar unas cosas.  
  
Tú, super analista digital que vas sobrado y que crees que has etiquetado perfectamente todo tu site, le preguntas a tu compi qué es lo que quiere medir y.... te enseña una parte que se te olvido etiquetar. Para colmo no puedes contar con los chicos de IT porque son muy malos y no te quieren, o porque a pesar de que te quieren mucho, tienen una cola de trabajo y las tareas de implementar analítica se encuentran relegadas a la ultima posición de cosas por hacer.  
  
Ante esta perspectiva, no queda más que darle un poco al ingenio y hacer alguna ñapa que otra: Por suerte o por desgracia, Google Tag Manager dispone de un par de variables que nos pueden ayudar. La primera de ellas es la de elementos DOM, y la segunda es la variable javascript. La documentación oficial la podéis encontrar en [este enlace](https://support.google.com/tagmanager/answer/6106899). Y si queréis leer al dios Simo, tenéis su explicación en este [otro enlace](https://www.simoahava.com/analytics/variable-guide-google-tag-manager/).  
  
En mi caso, lo que me paso, consistia en implementar los [típicos pixels de facebook](https://www.facebook.com/business/help/952192354843755), con sus acciones de pagina vista, búsqueda, añadir al carrito y compra. En mi caso la cosa estaba un poco más jodida porque casi todos los eventos (búsqueda, añadir al carrito y compra) estaban en la misma página y no podía crear triggers en Google Tag Manager en función de la página vista. También tuve que descartar que los tags se lanzase en funcion de unos triggers que escuchaban unos eventos que ya le había dicho a IT que los configurasen porque no estaban. ¿Qué hacer?. Pues bien, como no hay nada imposible para superdamupi, cree los eventos con funciones javascript que leían los valores de las variables y en función de los valores de estas variables, se lanzaba el tag o no.  
  
Como ya me estoy enrollando, voy a hacer un ejemplo como si quisieramos implementar el pixel de búsqueda de facebook. Para ello, he creado una página de ejemplo en mi dominio que podéis acceder desde [el siguiente enlace](http://www.damupi.com/test/damupi/index.html). El ejemplo que os he puesto tiene una pagina con dos campos de texto y el botón buscar.  
  
Normalmente en los ejemplos que hay por internet, el código de la página, o bien tiene un id el control que estamos buscando, o bien la página tiene poco código html y el elemento DOM es relativamente fácil de recuperar. En el ejemplo que os pongo lo es, pero creedme cuando os digo que puede que haya páginas cuyo codigo sea un maldito infierno.  
  
  
Cuando eso os pase, para recuperar el valor de un elemento DOM, yo creé en GTM una variable javascript que no es más que una función que recupera el primer elemento con la clase que le indiquemos. Por eso, por muy complejo que sea el código de tu página, y aunque no tenga un id, siempre podremos recuperar el valor de ese elemento DOM. La funcion javascript se llama [querySelector](https://developer.mozilla.org/es/docs/Web/API/Document/querySelector).  
  
Así que con esta función, en nuestro Google Chrome (supongo que se podrá hacer lo mismo con Firefox) nos vamos al elemento que queremos y le damos con el boton derecho en inspeccionar:  
  

[![](/img/querySelector.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEja9rcrCWA8Y7ua47QC-A7JWKMZpoewB9w5gDgYWdxVw7piMjvk5vYbiANrLXVe8l6E21W2nTqZsfWJZ1yfqLOqKi98B6Mfvw3izYjcMY47E2jZdi-PaPMyE3jVL3bwW8HqzwcK/s1600/querySelector.png)

  
  
Una vez que tenemos el control, tenemos que saber qué propiedad de ese elemento nos va a devolver lo que está escrito en ese control de texto. Yo normalmente suelo comprobarlo con 3 propiedades: innerText, innerHTML o value. En el ejemplo que os pongo, el método que nos vale es "value"  
  

[![](/img/domValue.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh4c-i3lUqfgQ4-8yLEM-onGcF4OlZHcGg9lGqJjrq-h50AVLrKQ9wDcFWDTHRXqIIFiMwWbUgrHy5OwhUEWJ97CFSI0bCr7Dla4kdLeGSNEuqAG0IGYAFBWEDqCuKOBOfGMSkm/s1600/domValue.png)

  
  
  
Pero imaginaros si no os funciona value... en ese caso, sustituid las líneas de arriba en lugar de "value" por "innerHTML" o "innerText". En el caso que me paso, era innerHTML. El código en la consola es el siguiente:  
  
> var filtro1 = document.querySelector('body > div > div > div:nth-child(2) > input');  
> var filtro1Str = filtro1.value;  
> filtro1Str = filtro1Str.trim();  
> console.log('El Filtro1 es: ' + filtro1Str);

  
De esta manera, la última linea, console.log, nos va a mostrar si recoge o no ese control. En este caso nos devuelve *"Hola"*, que es lo que he escrito en el cuadro de texto.  
  
Una vez que tenemos identificados los selectores y sus controles para recuperar lo que introduce el usuario, es el momento de crear esas dos variables en tag manager. Nos vamos a variables, elegimos Custom Javascript y pegamos la funcion:  
  

[![](/img/gtm_cj_filtro1.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjFr9qkjOgBd1I4SAzE1MpF0_CChHLa4P_Yom7lVvpSLuitpi0pUJV8zIrau7fqTt7GDO-nosrBz-d4QPnI9t1LBT3Dq95ZttDUedLkd10WzZqIT6Dkj8LRmVOyxJnts9zUOfiX/s1600/gtm_cj_filtro1.png)

  
  
  
Para los que son tan vagos como yo (...o más), os copio el código de la función:  
  
> function(){  
>  var filtro1 = document.querySelector('body > div > div > div:nth-child(2) > input');  
>    var filtro1Str = filtro1.innerText;  
>  filtro1Str = filtro1Str.trim();  
>    // console.log('El Filtro1 es: ' + filtro1Str);  
>    return filtro1Str;  
>   }

  
Ahora hacemos lo mismo con el filtro 2, es decir, inspeccionamos el elemento, nos vamos al código fuente que está resaltado, y en windows (no tengo un Mac, si alguien se siente altuísta que me mande un DM) con el boton derecho --> Copiar --> Copiar Selector.... Y de nuevo los mismos pasos que para el filtro 1.  
  

[![](/img/console_log_filtros.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj1f2RYOWlSHxw-IZuJyCg3moKQKOYoGzbtRO_nYHc-nrOkoHxwIceOQ549bvR-G0zZWClxrquw4_7ROKbzXOajTvr28ZuO4vRSHegqo7B8nsB0ydNRxRJ2R01A1UtBd5e-g9Iw/s1600/console_log_filtros.png)

  
  
  
En GTM lo mismo, creamos otra variable para el filtro 2  
  

[![](/img/gtm_cj_filtro2.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg7r4LJtjwFIOA5cotVOJZkN2ttPFWeQgtJnQ2ITiGsMuIc2Qn29J3cHYJP3UjRrn2QeDLHY830LneYOQdQ7Ihx4QdAthHtpGuoacGrQlsUlY4lly4qhZGZOPIS2vBAdLp7L2xk/s1600/gtm_cj_filtro2.png)

  
  
  
  
Ahora lo que necesitamos es un trigger en nuestro GTM que diga algo como: "cuando tenga rellenos los dos campos de los controles de texto y le de al botón buscar.......". Para ello tenemos que hacer un trigger de tipo click, porque queremos que se lance cuando el usuario haga click sobre el botón Buscar. Para ver como se llama el click ID o la class de ese botón, abrimos GTM en modo preview pulsamos en el botón de búsqueda y nos vamos al evento gtm.click y a variables para ver alguna variable que nos pueda servir como referencia. En este ejemplo podemos coger "click classes"  
  

[![](/img/gtm_click_boton_classes.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjJZvTh-irBP4ga_yKPLaMhQuZ2pZCQw6D5IH5e3ZDrruXd5pCkZk4UxqTozS4oCVvx4ZJ_gKzuSS0EXPteDtHwSJ0U1qtcMXGA7Fxi5B1Z53p4eySrWoLbH7CyVAkgLNfZMbpD/s1600/gtm_click_boton_classes.png)

  
  
  
  
En este ejemplo, el valor que nos da click classes cuando hacemos click sobre el botón es "btn btn-lg btn-primary btn-block".  
  
Ahora es turno de la creación del trigger, en este ejemplo lo he llamado "Click Boton Buscar";  así que ahora construimos nuestro trigger diciéndole algo así como: si los controles de texto están rellenos y me pulsas ese botón.... entonces me lanzas el tag que yo te diga. Así que como diría jack el destripador, vamos por partes:  
  
Si el control de texto filtro 1 está relleno:  
  
{{damupi - CJ - filtro1}} -- does not mach RegEx -- (undefined|^$)  
  
Si el control de texto filtro 2 está relleno:  
  
{{damupi - CJ - filtro2}} -- does not mach RegEx -- (undefined|^$)  
  
Y se hace click sobre el elemento con las clases:  
  
click classes -- equals -- btn btn-lg btn-primary btn-block  
  

[![](/img/gtm_trigger.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjMTqVp_vqUKwItSZNXWE467clCdmBy0T360nuASXzcNsJj0lSOZNsxMiHM9LvGACLkNbhfFEwGm8vcpj6RVgg0Rm4S2EZSddWEoFlIweTB-6tqXCCbrd0YTFagmLOLzPJgwSCK/s1600/gtm_trigger.png)

  
  
Ahora ya solamente nos falta poner el tag que queremos, que en este caso, sería el pixel de facebook de búsqueda. La documentación dice que el pixel sería este:  
  
fbq('track', 'Search', { search\_string: 'leather sandals' });  

Como nosotros queremos también saber lo que busca tanto para el filtro 1 como para el filtro 2, concatenaremos la cadena de búsqueda unida por un guión, retocamos un poco el tag de facebook para que incluya los dos campos. El pixel sería así:

  

[![](/img/gtm_tag_buscar.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiZ18gzSEOiglu465rVBGhPQ_bJBMn7obRcEMigCO8y0Lrx7HodJQK6OwOnV9wOu9HcoR-71Hua8tH5oZkx4oUPPc4sQpwX4ruybzW62_AKKlW6YPQ0ujfw05HXsZVAPS-RjZjD/s1600/gtm_tag_buscar.png)

  
  
  
Nota: recordad cambiar vuestro id de facebook  
  
Y ya está :)
