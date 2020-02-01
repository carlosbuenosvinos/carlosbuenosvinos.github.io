---
title: 'No necesitáis consultoría sobre DDD: Caso Motor de Reservas'
author: Carlos Buenosvinos
type: post
date: 2017-04-03T09:00:15+00:00
url: /no-necesitais-consultoria-o-formacion-sobre-ddd-caso-1/
featured_image: /posts/images/2017/04/rainbow_texture679.jpg
dsq_thread_id:
  - "5691445730"
categories:
  - 'Agile &amp; Craftsmanship'
  - DDD
  - Talks
tags:
  - anemic domain model
  - ddd
  - pragmatismo
  - unit testing

---
Cuando una empresa me contacta para hacer consultoría, monto un Skype para entender mejor las necesidades y la situación en la que se encuentra. Normalmente, hablo con un par de técnicos y/o el CTO que me cuentan sus dificultades. Tengo varias de éstas al mes. Algunas son empresas grandes (50 developers o más) y más pequeñas (menos de 10 developers). Como no me gano la vida haciendo consultoría, soy muy imparcial con lo que necesitan y **sobretodo con lo que no necesitan**. En sus dificultades, algunos patrones se repiten (miedos, prejuicios, presiones, modas, etc.). Me gustaría resumir cómo fue una de esas video conferencias por si podéis estar en la misma situación.

<!--more-->

<h4 style="text-align: left;">
  Conversación
</h4>

<p style="text-align: left;">
  <strong>Ellos:</strong> Hola Carlos, necesitaríamos formación o consultoría sobre DDD.
</p>

<p style="text-align: left;">
  <strong>Yo:</strong> Genial! Contadme un poco sobre el proyecto.
</p>

<p style="text-align: left;">
  <strong>Ellos:</strong> Es un sistema de reservas para el sector X que está muy pensado para la venta por un canal. Ahora tenemos que abrirnos a multi canal y tenemos miedo de liarla parda.
</p>

<p style="text-align: left;">
  <strong>Yo:</strong> Ya veo, cuantas personas trabajan en el proyecto?
</p>

<p style="text-align: left;">
  <strong>Ellos:</strong> En la empresa, 5 developers, pero en este proyecto somos nosotros dos.
</p>

<p style="text-align: left;">
  <strong>Yo:</strong> Dos. Ok. Cuántas tablas tenéis en BBDD?
</p>

<p style="text-align: left;">
  <strong>Ellos:</strong> Bueno&#8230; deben ser unas 20 como mucho.
</p>

<p style="text-align: left;">
  <strong>Yo:</strong> Ok. Y cuál es el estado de vuestro código? Tenéis tests? Subís frecuentemente a producción? Tenéis la lógica de negocio en el web controller?
</p>

<p style="text-align: left;">
  <strong>Ellos:</strong> El código está bien. Hay tests unitarios, servicios de aplicación desacoplados de Symfony y tenemos Jenkins. Sí que es cierto que nuestras entidades tienen <i>getters</i> y <i>setters </i>y la lógica de negocio está en esos servicios de aplicación. Sabemos que es un <i>modelo anémico </i>pero nos va bien así.
</p>

<p style="text-align: left;">
  <strong>Yo:</strong> Guay! Tener un <em>Anemic Domain Model</em> no es un problema cuando es lo que quieres. Con bajas complejidades suele ser lo que funciona mejor. Volviendo a vuestro proyecto, si el código está desacoplado y tenéis test unitarios, con todo el cariño, cuál es el problema?
</p>

<p style="text-align: left;">
  <strong>Ellos:</strong> Bueno&#8230; es que antes trabajaba con nosotros un talibán de &#8220;todo esto&#8221; y tenemos miedo de liarla parda.
</p>

<p style="text-align: left;">
  <strong>Yo:</strong> Entiendo. Lo primero que os diría es que no os obsesionéis. Hay que separar lo que es poco relevante de lo que puede serlo más. Por ejemplo, en qué carpeta va un fichero no va a ser determinante para el éxito de vuestro proyecto. Pero las implicaciones sobre CRUD vs. Rich Domains podría serlo. Es bueno ser práctico si entiendes los trade-offs de tus decisiones. Lo estáis haciendo bien. Segundo, parece que vuestra aplicación no tiene una complejidad muy grande. Lo de agregar el concepto de multicanal parece un problema de diseño de software normal y corriente. Discutid frente a una pizarra blanca, proponer varias opciones, elegid una e implementadla. Si no os gusta, la cambiáis.
</p>

<p style="text-align: left;">
  <strong>Ellos:</strong> Oh! Gracias Carlos. Nos quedamos mucho más tranquilos. Ha sido una charla reveladora. Hay otros productos que tendremos que integrar más adelante e igual la cosa se complica más. Te importaría que te contactáramos entonces?
</p>

<p style="text-align: left;">
  <strong>Yo:</strong> Claro, no hay problema. Gracias a vosotros. Mucha suerte y volved a contactar cuando queráis.
</p>

<h4 style="text-align: left;">
  Conclusión
</h4>

Os dejo algunas recomendaciones que a mí me funcionan:

<li style="text-align: left;">
  No os dejéis llevar por modas o presiones, podríais meteros en cosas que no necesitáis.
</li>
<li style="text-align: left;">
  CRUD (como ejemplo de Modelo de Dominio Anémico) funcionan muy bien (ir rápido y pocos bugs) con lógica de negocio sencilla.
</li>
<li style="text-align: left;">
  El 80% de los problemas de velocidad de desarrollo tienen que ver con código acoplado y no tener tests. Independientemente del estilo arquitectural.
</li>
<li style="text-align: left;">
  Si entendéis, sois realistas con los pros/cons de cada decisión y los asumís, estáis haciendo bien vuestro trabajo. Os podéis equivocar, pero con tests y código desacoplado debería ser fácil revertir una decisión.
</li>

Espero que hay sido útil. Si tenéis dudas o queréis que os eche un cable, podéis enviarme un mail a _carlos.buenosvinos@gmail.com_.

<p style="text-align: left;">