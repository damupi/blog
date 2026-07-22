---
title: "Hacer consultas en el Directorio Activo (LDAP) via excel"
date: 2015-03-18 17:34:00.003
tags: ["excel", "directorio activo", "AD", "ldap"]
category: professional
---

Ultimamente mi trabajo se reduce a rellenar tediosos Excel de información y más información. Seamos optimistas y pensemos que sirve para documentar las cosas y para estructurarlas mejor, pero seamos realistas y admitamos que es un trabajo rutinario y aburrido.  
  
Como mi abuelo me dijo una vez, "yo trabajo para no trabajar", lo que viene a traducirse en hacer un script que automatice las tareas. En esta ocasión, se trata de extraer datos desde el Directorio Activo de la empresa, o como a mi me gusta llamarla, la fabrica de chocolate; a un archivo de Excel.  
  
Como persona que lleva trabajando más de 10 años en informática, en sistemas y programación a bajo nivel, cuando le hablan de Excel se le queda un poco cara de decepción, pero después de haber hecho el [máster de Analítica Web en Kschool](http://kschool.com/cursos/master-analitica-web/), uno se da cuenta de que Excel es una herramienta potentisíma, y lo que es más importante, es capaz de unir a los departamentos. Esto es, si le pones un Excel a una persona de marketing, te lo agradecerá más que un SQL.  
  
En esta ocasión mi jefe me pidio extraer ciertos datos a excel cuando ya estaban en el Directorio Activo de la empresa. Supongo que mis compañeros de trabajo (y hasta puede que mi jefe) en estos momentos estarán copiando los datos de un lado a otro. Yo, sin embargo, trabajé para no trabajar más. Esto es, instale un complemento en Excel que lee los datos de tu Directorio Activo y los vuelca en tu Excel.  
  
Y sin más dilación, paso a explicaros como lo hice. Lo primero, no es un complemento mio, sino de [Remko](http://www.remkoweijnen.nl/). Para instalar el complemento, primero tenemos que [descargarlo de su web](http://www.remkoweijnen.nl/blog/download/RWADAddin.zip). Una vez descargado, tenemos que ubicarlo en la carpeta adecuada de nuestros complementos de Excel. En mi caso, utilizo Excel 2013 y mis complementos están ubicado en la siguiente carpeta:  
  
C:\Users\damupi\AppData\Roaming\Microsoft\AddIns  
  
(donde damupi sería mi nombre de usuario).  
  
Una vez que hemos guardado ese magnifico archivo de Remko, tenemos que habilitarlo en Excel. En mi versión, nos vamos a opciones.  
  

[![](/img/1.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh-xL09HXeai4h-7RYr1B1aKlrAAxYr3MvONeqLTrWbBTFTHw4g_8qeIOIChKqFBBrq2qInGRXvKYi-gJZvnTrGPTAjsWemje9A6DDkNOtpxH4ImOxpukXISSLJVF_RArlVE1my/s1600/1.png)

Despues nos vamos a complementos y le damos al botón "Ir"

[![](/img/2.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgrRHxl8wCvcfmlfQqRSFKMgw7nc05TDUP4-hxt5sNF83pdHSo-7iuV0UXQo1opHQAH1yhNoavQwGJCQUHI-pv45AJf6a5qkgtAeDCKSzgRmcPzxQAwv4wukTv1ShVpMHnROawj/s1600/2.png)

Por último seleccionamos el complemento "Rwadaddin"

[![](/img/3.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhs5B15KTdNl9653fu2RjZ3iiYj1DIqw-Uplla0CrHlpt4D8WPOf7Ey4HpfTndLpftTNxvAh_ovj5IR5sHwPIll_VC5S88HoPWx1YORxQP6BTu27MtIBPJWkdVbIkNPtYLj3MzR/s1600/3.png)

Una vez tenemos el complemento instalado, en una columna, en mi caso A2, ponemos el nombre completo del usuario (en mi caso el nombre completo es "soporte") y en la columna B2 ponemos la siguiente funcion: *=GetAdsProp("cn";A2;"mail")* para que nos devuelva el email del usuario

El resultado es el siguiente:

[![](/img/4.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhFbs_tI0-K3bTeknWkRE3gxvswnI-91krxYC7V9CWSIH8fGwYAc-AOP8UGp7r5oaWPWPpKSyw3BBA7euEpNc5aJSe7FkVaCGVInOXclY826-4AbeXy_9dJf1Kzo8-3Y-lLLhsz/s1600/4.png)

Ahora, si queremos obtener mas datos, en otras columnas, podemos utilizar los atributos que nos brinda LDAP como el login (SamAccountName), el número de móvil (Mobile), el departamento al que pertenece (department) o la descripción (description)

Espero haberos ayudado y muchas gracias a Remko, sin el cual, este articulo no hubiese sigo posible.

Fuentes: [Query Active Directory from Excel](http://www.remkoweijnen.nl/blog/2007/11/01/query-active-directory-from-excel/)
