---
title: "Como analista de datos, me reemplazará la IA?"
date: 2025-07-06
tags: ["MCP", "GA4", "IA", "analista de datos"]
category: professional
source: medium
source_url: "https://medium.com/@damupi/como-analista-de-datos-me-reemplazar%C3%A1-la-ia-4c96ed9cf86f"
---

![](/img/2025-07-06-como-analista-de-datos-me-reemplazara-la-ia-1.png)

Ya ha pasado casi 2 semanas desde el ultimo Measurecamp Madrid, y queria hacer una recapitulacion.

Por si no lo sabeis, al final [Javier Lázaro](https://www.linkedin.com/in/javi-lazaro/) me incito a exponer. Quedaban aun varios huecos a ultima hora de la tarde, asi que escribi una ficha y escribí:

“1 MCP vale mas que 1000 dashboards”, lo expuse en ingles “1 MCP worths more than 1000 Dashboards”.

Estuve hablando de lo [que es un MCP](https://modelcontextprotocol.io/introduction) y había creado uno de Google Analytics 4, para la empresa en la que trabajo,

Hay MCPs de GA4 por internet, pero veía que todos estaban bastante en pañales porque solo valían para una propiedad y se limitaban a tirar de la [API de runReport](https://developers.google.com/analytics/devguides/reporting/data/v1/rest/v1alpha/TopLevel/runReport).

No puedo entrar en detalles aquí, pero donde trabajo, tenemos mas de 20 propiedades de GA4, así que me cogí el SDK de python y me puse a crear unas “tools”

Cuando les enseñe a los invitados a la exposición, de lo que un MCP era capaz, a muchos le explotó la cabeza como me paso a mi cuando lo descubrí. Podia ver sus caras de, no se que decir, o de como su cabeza no paraba de pensar. Y a continuación vinieron las preguntas. Preguntas para las que no tengo respuestas, sino opiniones, y que han sido fruto de mi comida de cabeza y de mi conversación, con [Julio Salazar Ortiz](https://www.linkedin.com/in/juliosalazarortiz/), en el la fiesta del Measurecamp Madrid, a altas horas de la noche y con varias cervezas a cuestas.

La primera pregunta es: _Y ahora que vamos a hacer nosotros_ ? _Nos va a sustituir la IA_ ?

Bueno, mi opinion es que sí y que no. Sustituirá a esa gente que no cambie, que no se adapte a los nuevos tiempos, y que no comprenda las nuevas herramientas.

Esto no es nada nuevo, nuestro perfil esta en constante evolución y nunca ha tratado de si sabes la herramienta GA4 o la herramienta Tableau. Si tu eres un analista, y sabes adaptarte a los nuevos tiempos, la respuesta es _no, la IA no te va a sustituir_. [Julio Salazar Ortiz](https://www.linkedin.com/in/juliosalazarortiz/) , tan sabio él, me decía:

> _“nosotros siempre hemos sido unos traductores entre negocio y tecnología”_
Esa es la pura realidad.

Dentro de poco, habra nuevas herramientas que traducirán mejor entre negocio y tecnología, y para eso no nos necesitarán mas. Pero también serán necesarias nuevas traducciones debido a estas nuevas herramientas.

De nosotros depende si tenemos la curiosidad y ganas de aprender esas nuevas herramientas y adaptar nuestro rol o no.

Así que aparte que los analistas que se irán porque no se adaptaran a estas nuevas herramientas, habra nuevos analistas, que cubrirán a estos otros, y cubran ciertos vacíos que no existían antes.

Puede ser triste, pero prefiero asumirlo, que no negarlo y, como creo que estoy haciendo, adaptarme mientras que estamos en pleno cambio.

Otra pregunta que me hacen siempre a propósito de los MCPs es la seguridad: _Que pasa si nuestro MCP tiene datos sensibles o que no queremos que accedan ciertas personas, o LLMs_?

Esto no es ningun problema nuevo y estaba ya pensado. [Los MCP cuentan con sistemas de autenticacion](https://modelcontextprotocol.io/specification/2025-03-26/basic/authorization), asi que podran denegar el acceso a los clientes que quieran, cuando la configuración se haya realizado correctamente.

Otra cosa es, si tú, teniendo permso para acceder a un MCP, pero lo haces con un LLM que comparte sus datos con su creador (openAI, Meta, X, ….) ahi tienes un problema. En otras palabras, es como si compartieras en chatGPT un excel con la nomina de tus empleados, … y para eso, no hay que ser muy listo, para saber no lo estas haciendo bien.

Os recuerdo que hay LLMs que se pueden albergar en tu propio servidor, como tener corriendo DeepSeek o Llama con Ollama, y no dandole la información “a los chinos, ni a Zuckerberg”.

Aprende, como todos los que hemos tenido curiosidad, y de ti depende cual sera tu camino.
