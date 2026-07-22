---
title: "Cómo automatizar tus datos de Mixpanel con Google Spreadsheet"
date: 2015-06-01 19:17:00.003
tags: ["analytics", "mixpanel", "google spreadsheet"]
category: professional
---

Hace poco me tocó enfrentarme a una nueva herramienta de analítica; mixpanel. Siempre que me preguntan por una aplicacion para analítica les digo: "dame una herramienta, y moveré el mundo". Así que después de ponerme unos días con Mixpanel, la primera conclusión que podemos sacar es que es una herramienta que mide eventos.  
  
Lo siguiente a lo que me tuve que enfrentar fue un reporting en una hoja de cálculo de Google (Google Spreadsheet). En este informe semanal, había datos de mixpanel y de Google Analytics.  
  
El primer día que tuve que hacer el reporting casi me suicido: yendo de una pantalla para otra de mixpanel, poniendo el intervalo de fechas, filtrando por los parametros y seleccionando sus valores... que tedio... no podía más.  
  
Lo terminé y me juré la máxima de mi abuelo: "hay que trabajar para no tener que trabajar". En otras palabras, que me curré un script para atacar a mixpanel y recopilar todos los datos que me pedían con un solo click.  
  
La solución se la debo a Melissa Guyre, [que en está página](https://github.com/melissaguyre) se curró un fabuloso script para poder automatizar los reporting que nos pide cada departamento. La automatización es muy sencilla:  
  
- Creas una nueva hoja de cálculo de Google.  
- Herramientas --> Script editor (no sé si en castellano pone otra cosa)  
  
Abres el editor de script y copias y pegas el [script de melissa](https://github.com/melissaguyre/mixpanel-segmentation-google-spreadsheets/blob/master/mixpanel-segmentation.gs). Rellenas los puntos 1, 2, 3 y 4. y a correr....  
  
En mi caso, tengo como 10 hojas de calculo y aparte el addon de google analytics. Al final todo acaba en una hoja llamada "resumen" que recibe los datos de las otras ojas creadas, ya sea con Mixpanel o con Google Analytics. Y de esta manera, lo que se tardaba 4 horas cada semana en hacer, se automatiza y se saca con un par de clicks ;)
