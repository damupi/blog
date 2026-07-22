---
title: "Actualizando getTimeParting : el plug-in de Adobe SiteCatalyst"
date: 2016-04-14 18:35:00.001
tags: ["plug-in", "getTimeParting", "SiteCatalyst", "Adobe"]
category: professional
---

En el día de hoy me ha tocado actualizar el plug-in de Adobe SiteCalayst getTimeParting. Para los que no lo conozcan, y como su propio nombre indica, te permite "partir el tiempo". En otras palabras, en lugar de tener un campo con todo el tiempo del tipo: "1460654529097" o "Thu Apr 14 2016 19:23:16 GMT+0200", lo que te permite hacer este plugin, es que puedas almacenar partes del tiempo, como las horas, o las horas y los minutos, o los meses, o el tipo de días de la semana, por ejemplo, en tus variables personalizadas.  
  
De esta manera, podemos hacer informes por fines de semana o laborables, o por meses, o por miercoles, o lo que se nos quiera ocurrir. En el caso que me ocupa, sólo hemos recogido el día, es decir, si era lunes, martes,.... pero en inglés (Monday, Tuesday, Wednesday, Thursday,....), la hora y los minutos en formato 9:00AM, así como el tipo de día, es decir, si era día de la semana o fin de semana (weekday o weekend).  
  
La documentación del plugin la tenéis en la siguiente URL: [Adobe SiteCatalyst plug-in](https://marketing.adobe.com/resources/help/en_US/sc/implement/getTimeParting.html)  
  
Y yo, para poder hacer la actualización, me he basado en esta página:  [webanalyticsland](http://webanalyticsland.com/sitecatalyst-plugins/gettimeparting)  
  
Para probar el javascript on line, he utilizado la siguiente pagina: [webtoolkitonline](http://www.webtoolkitonline.com/javascript-tester.html)  
  
Al final de este artículo os dejo los archivos que he utilizado, ya que no se me formatea muy bien el código en el blog :(  
  
El código de la función es el siguiente:  
  

```
 getTimeParting=new Function("h","z",""  
 +"var s=this,od;od=new Date('1/1/2000');if(od.getDay()!=6||od.getMont"  
 +"h()!=0){return'Data Not Available';}else{var H,M,D,U,ds,de,tm,da=['"  
 +"Sunday','Monday','Tuesday','Wednesday','Thursday','Friday','Saturda"  
 +"y'],d=new Date();z=z?z:0;z=parseFloat(z);if(s._tpDST){var dso=s._tp"  
 +"DST[d.getFullYear()].split(/,/);ds=new Date(dso[0]+'/'+d.getFullYea"  
 +"r());de=new Date(dso[1]+'/'+d.getFullYear());if(h=='n'&&d>ds&&d<de)"  
 +"{z=z+1;}else if(h=='s'&&(d>de||d<ds)){z=z+1;}}d=d.getTime()+(d.getT"  
 +"imezoneOffset()*60000);d=new Date(d+(3600000*z));H=d.getHours();M=d"  
 +".getMinutes();M=(M<10)?'0'+M:M;D=d.getDay();U='AM';if(H>=12){U='PM'"  
 +";H=H-12;}if(H==0){H=12;}tow=parseFloat(D);tow=(tow<5)?'Weekday':'We"  
 +"ekend';D=da[D];tm=H+':'+M+U;"  
 +"if(h=='h'){return H}if(h=='d'){return tm}if(h=='w'){return tow}}");
```

  
Recodad, que según la documentación de Adobe, antes de este código, en el s\_code, en la parte "CONFIG SECTION", teneis que poner el array s.\_tpDST de vuestra localización que en mi caso era el siguiente:

  

```
 //Europe  
 _tpDST = {  
 2012:'3/25,10/28',  
 2013:'3/31,10/27',  
 2014:'3/30,10/26',  
 2015:'3/29,10/25',  
 2016:'3/27,10/30',  
 2017:'3/26,10/29',  
 2018:'3/25,10/28',  
 2019:'3/31,10/27'};
```

  
Por último, y tal y como indica webanalyticsland, si queréis almacenar este "tiempo partido", debéis de hacer lo siguiente antes de poner la función:  
  

```
 s.prop1=s.getTimeParting('h','+2');  
 s.prop2=s.getTimeParting('d','+2');  
 s.prop3=s.getTimeParting('w','+2');
```

  
De esta manera, se os almacenará en las propiedades 1,2 y 3, respectivamente, la hora, el día de la semana y el tipo de semana.  
  
Como no se ve bien el formato al pasarlo al html, os pongo los enlaces de los archivos que he utilizado:  
  
- [html que llama al archivo javascript timeparting.js](http://www.damupi.com/files/test_timeparting.html)  
- [archivo javascript timeparting.js](http://www.damupi.com/files/timeparting.js)  
- [archivo js de pruebas en webtoolkitonline](http://www.damupi.com/files/test.js).  
  
Saludos.
