---
title: "Actualizacion de Wordpress en menos de 2 horas"
date: 2013-03-24 20:02:00.002
tags: []
category: professional
---

El pasado viernes parecía que iba a ser un viernes como todos los demás en la fabrica, lusers llamando y aguantándoles y poniéndoles una sonrisa telefónica, usuarios con los que da gusto resolverles sus problemas y algún que otro expediente x que da gusto solucionar; aparte de algún marrón... PUES NO!!, aquel día me tocó actualizar Wordpress. Por fin, algo de intelecto en mi trabajo.  
  
Resulta que allá por el 2010 la fabrica pidió un blog, como siempre, sin tener ni puta idea de las tecnologías que existían en el mercado y queriéndolo para ayer y gratis. Deck, acertadamente desde mi punto de vista les montó un Wordpress. Que si... que Drupal es lo mejor, que sí... que Joomla es más potente que Wordpress... creo que para lo que querían, y el manejo que tenía Deck de CMS y el que tenian los usuarios que los iban a dotar de contenido, Wordpress fue una opción acertada.  
  
El viernes pasado, a marzo de 2013, parece que, por fin, fue el día de actualizarlo. Estaba programado desde hacia meses la actualización pero claro, como siempre en la fábrica, todos pasándose la patata caliente y ninguno echándole huevos para hacerlo no vaya a ser que la culpa de que luego no funcione sea del que pone intención de actualizarlo y no del que no hace nada y lo deja obsoleto con sus correspondientes fallos de seguridad. Así que después de unos emails cruzados entre el jefe de mi departamento, el jefe de mi jefe, el que lleva tecnicamente el blog, el que lo dota de contenido y su respectivo jefe común, me rebota la conversación diciendo: damupi, puedes actualizarlo?  
  
Claro que el jefe de mi jefe me pregunte que si puedo actualizarlo me jode, porque parece dudar de mi capacidad. Puedo hacer eso y más, tan sólo es que no me das la oportunidad. Puede que no sea tan diplomático como mi jefe y que no sepa guardar tan bien las composturas como él, pero seguro desde el punto de vista de aptitudes técnicas le doy mil vueltas (sino más). Me empieza a contar un rollo de que no quiere que se estropee lo que ya tenemos, cosa que me conozco de sobra del típico usuario que duda de lo que estás haciendo porque no tiene ni puta idea y tienes que intentar explicarle lo que vas a hacer para actualizarlo sabiendo que no se va a enterar y que lo único que quiere oir es que no hay ningún riesgo.  
  
Así que después de la toma de datos con el técnico del otro departamento que llevaba el blog desde 2010 y que no había actualizado nada, me pongo manos a la obra. Creo una nueva base de datos, exclusiva para el blog no como estaba la otra que, esta dentro de la misma base de datos con otras tablas que tienen otra utilidad. Hago un dump de las tablas, e intento restaurarlo en la nueva: ERROR!!!  
  
Googleo un poco y resulta que en dos años la estructura de tablas no es la misma; no me extraña. Damupi no se da por vencido y hay una herramienta en el propio Wordpress para exportar las entradas, los usuarios y la taxonomía entre otros. Lo exporto y lo importo en la nueva base: Et voila!!!  
  
Ahora la media library, veo donde esta en la antigua base y veo que la estructura de archivos se corresponde con las fechas, copio y pego y ya tenemos la libreria importada. Ahora el tema, copio y pego el tema tuneado de la antigua, lo pego en la nueva y todo perfecto.  
  
Problemas: todos los comentarios quedan sin moderarse. Labor para el editor no para mi.  
  
Ahora los plugins: los detecto, los busco, los instalo en el nuevo y le digo al tecnico que los edite de acuerdo a las necesidades de la fabrica. Labor para el técnico, no para mí que te he actualizado wordpress y sus plugins.  
  
Wordpress actualizado en menos de dos horas. Eso sí, mi email diciendo que ya he hecho el trabajo que nadie quiso hacer, llevó consigo la peticion de que me dejasen conectarme a twitter, herramienta de conocimiento que me sirve de ayuda para mi aprendizaje profesional y que la puta fabrica me tiene capado porque algún zopenco que no tiene ni puta idea, cree que twitter sólo sirve para que el trabajador no produzca y se quede navegando por la red sin hacer nada a cambio por la empresa. Nada más lejos de la realidad, twitter es un medio más, depende de como tú lo quieras utilizar. Si tú confias en mí es porque utilizare las herramientas de una buena manera, sino,.... en fin, actualizado.
