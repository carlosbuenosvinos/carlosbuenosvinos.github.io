---
title: 'Consulta sobre DDD #1: Pregunta sobre las entidades en DDD (Identidad y UUID)'
author: Carlos Buenosvinos
type: post
date: 2015-10-15T07:30:53+00:00
url: /consulta-sobre-ddd-1-entidades-identidad-uuid/
categories:
  - DDD
  - PHP
tags:
  - database
  - ddd
  - domain-driven design
  - entities
  - identity
  - php
  - sequences
  - uuid

---
A raíz del libro <a href="https://leanpub.com/ddd-in-php" target="_blank">&#8220;Domain-Driven Design in PHP&#8221;</a> y los videos que tengo en <a href="https://www.youtube.com/playlist?list=PLfgj7DYkKH3DjmXTOxIMs-5fcOgDg_Dd2" target="_blank">Youtube</a>, semanalmente, me van llegando consultas sobre temas de relacionados. Son bastante interesantes y me gustaría compartirlas así como mis respuestas. A los autores de los mails les he pedido permiso para publicar la conversión. Las iré agregando en la medida que pueda. La primera es de Álex sobre Entidades, Identidad y UUIDs.

<!--more-->

* * *

Hola Carlos,

Hace unos días he empezado a buscar información sobre DDD y me topé con tu repositorio en GitHub donde tienes un ejemplo que al parecer estas utilizando en el libro que estás haciendo. Lo primero felicitarte porque tiene buena pinta y seguro que merece mucho la pena.

Quería hacerte una pequeña pregunta sobre la identificación de entidades. En todos los sitios donde se habla sobre esto se cuenta que las entidades cuando se crean (vía constructor o vía factorías) deberían ser creadas ya con su ID y para ello hay mecanismos como los GUID o los UUID como el que muestras en el ejemplo de &#8220;last-wishes&#8221;, los cuales permiten generar ID&#8217;s a nivel de dominio.
  
Esto me ha chocado con lo que nos han estado enseñando todo este tiempo en el instituto. Siempre nos han mostrado crear bases de datos relacionales ligando tablas a través de ID&#8217;S autoincrementables y estos ID&#8217;s solamente se pueden conocer una vez que se hayan insertado en la base de datos mediante una operación Insert. Bueno, también se podría hacer una consulta a la tabla para obtener el máximo y después sumar 1 pero eso creo que podría causar problemas si se realizan varias peticiones a la vez.

Mis preguntas son: ¿En este tipo de diseño a caso se ha desplazado por completo la generación de identificación desde la base de datos al dominio? ¿Eso significa que debo de dejar de usar ID&#8217;s autoincrementables? ¿Utilizar un UUID no va a resultar en consultas más lentas al buscar por ejemplo por ID o relacionando con otras tablas? Parece que es más costoso buscar por ejemplo un post con un id así 550e8400-e29b-41d4-a716-446655440000 que con un número autoincrementable.

Perdona si me he enrollado mucho y si no ha sido el medio más apropiado para formularte la consulta.

Un saludo, Álex.

* * *

Hola Álex,

<div>
  Conocer el ID de un objecto antes de persistirlo tiene muchas ventajas. Los UUIDs se usan para esto, garantizar identidad y tener identidad antes de persistir. Recuerda que las entidades existen cuando se crean, no cuando se persisten ;). La persistencia es un detalle de implementación.
</div>

<div>
</div>

<div>
  Para aplicaciones sencillas puedes usar IDs autoincrementales. Pero si estás con temas de DDD, llegarás rápido a darte cuenta que un UUID, te será más útil. En la Uni o el instituto te enseñaron a relacionar entidades por identidad, pero no que la identidad fuera autoincremental necesariamente ;).
</div>

<div>
</div>

<div>
  Si quieres tener un comportamiento al estilo DDD con autoincrementales, la implementación de los Repositorios, del nextId, tendrá que hacer una query a quien guarda la secuencia. En Oracle, SQLServer es fácil porque funcionan con secuencias, pero en MySQL necesitará emularlo: <a href="https://www.google.es/webhp?sourceid=chrome-instant&ion=1&espv=2&ie=UTF-8#q=mysql+sequences+nextval" target="_blank">https://www.google.<wbr />es/webhp?sourceid=chrome-<wbr />instant&ion=1&espv=2&ie=UTF-8#<wbr />q=mysql+sequences+nextval</a>
</div>

<div>
</div>

<div>
  Con las BBDD actuales, las búsquedas por UUID son igual de rápidas, no notarás la diferencia.
</div>

<div>
</div>

<div>
  ¿He respondido a todo?
</div>

<div>
</div>

<div>
  Puedo poner tu pregunta en mi blog de forma anónima para ayudar a otros? ;)
</div>

<div>
</div>

<div>
  Saludos,<br /> Carlos
</div>

<div>
  <hr />
  
  <p>
    Hola de nuevo
  </p>
  
  <p>
    ¡Vaya, no me esperaba una contestación tan rápida! No sabes cuanto te lo agradezco.
  </p>
  
  <p>
    Me ha quedado todo bastante claro.  Es la primera vez que leo sobre los UUID y eso unido a que soy todavía un principiante tal vez no me han hecho ver todo ese potencial que me comentas pero ya poco a poco voy identificando sus ventajas. Me ha impresionado eso de que las búsquedas sean igual (o casi) de rápidas.
  </p>
  
  <p>
    Seguramente acabe utilizándolo pero la posibilidad que me has dicho de que sí es posible usar IDs autoincrementales me abre una pregunta que ya por solo curiosidad me gustaría hacértela. Si tenemos un centenar de usuarios que acceden al mismo servicio, por ejemplo, &#8220;Crear post&#8221;, ¿el método nextId() del repositorio no devolvería en ciertos casos el mismo ID al tener a tantísimos usuarios acceciendo de manera concurrente al mismo repositorio? Nunca he utilizado secuencias y no sé si eso resolvería ese problema, o directamente si esto es un problema que me he imagino yo (el de la concurrencia me refiero) y si no se pudiese dar nunca.
  </p>
  
  <p>
    Me parece buena idea lo de añadir la pregunta al blog. He buscado mucho por internet (todo inglés porque en castellano lamentablemente hay poca información) y no he encontrado una respuesta concreta a lo que he planteado así que creo que sí estaría bien que si otros tienen la misma duda pudiese también resolverla de manera rápida. Agradecería también lo me ha dicho de que sea de forma anónima, :P porque de hecho pienso que tal vez ha sido una pregunta un poco tonta y simple.
  </p>
  
  <p>
    Bueno, creo que ya esta todo. Me ha resulto de manera muy clara todas mis dudas. Solamente esa pequeña curiosidad que me había surgido a raíz de lo de los ids autoincrementables, pero por lo demás está todo listo. :)
  </p>
  
  <p>
    Muchas gracias por su amabilidad y su tiempo :)<br /> Álex
  </p>
</div>

<div>
  <hr />
  
  <p>
    Hoy en día búsquedas exactas en BBDD son muy rápidas, así que no te preocupes. Sobre el tema de las secuencias, son seguras, si lo haces bien, dos invocaciones a la secuencia no deben devolver el mismo valor, por lo menos, las de Oracle así funcionan. Las únicas &#8220;quejas&#8221; son que requieres un acceso para una entidad que igual luego no se persiste, y que en caso de no persistir siempre, te quedan &#8220;huecos&#8221; en las secuentas y hay gente a la que le molesta.
  </p>
</div>

<div>
  <hr />
</div>

<div>
  Ajá, todo entendido! Gracias una vez más! <img class="CToWUd" src="https://ci6.googleusercontent.com/proxy/uZLFH0xFxu1BDuAesWdAoMwebWf5yJe-yuXoxIgzbQUtuN6H0cOMQDpJ99zuyf3xZE5js1qi=s0-d-e1-ft#https://a.gfx.ms/Emoji_1F44D.png" alt="Emoji" /><br /> ¡Seguiré de cerca el libro de DDD en php!
</div>