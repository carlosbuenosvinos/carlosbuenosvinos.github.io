---
title: ¿Cuál es el verdadero objetivo de TDD?
author: Carlos Buenosvinos
type: post
date: 2013-09-13T14:28:47+00:00
url: /cual-es-el-verdadero-objetivo-de-tdd/
switch_like_status:
  - "1"
categories:
  - 'Agile &amp; Craftsmanship'
tags:
  - agile
  - robert c martin
  - software development cost
  - tdd

---
A todos aquellos que habéis pensado: &#8220;¡Demostrar que mi código funciona!&#8221;, lo siento, estáis equivocados. Aunque está asociado al 2º valor principal del desarrollo de software (según Robert C. Martin).

<!--more-->

<strong style="line-height: 1.5;">Los valores del desarrollo de software (los dos se deben cumplir):</strong>

  1. Flexible (que se adapte fácilmente a los cambios del entorno, refactorización, etc.)
  2. Que haga lo que tiene que hacer (que funcione)

Robert C. Martin considera que el software sea flexible es más importante ya que el gran porcentaje de tiempo nos lo pasamos manteniendo código, leyendo más que escribiendo, desarrollando nuevas funcionalidades, arreglando bugs, etc.

> <span style="line-height: 1.5;">En comparación con otros sectores, la fase de desarrollar software (construirlo) es mucho más barato que diseñar software (pensar en cómo construirlo).</span>

Pensad en construir coches, hay que invertir millones en el diseño del coche y de su proceso de fabricación (planos, simulaciones, etc.) antes de construirlo, principalmente porque un buen diseño ahorrará millones a su fabricante.

¿Qué porcentaje de tiempo invertimos haciendo mantenimiento (bugs y nuevas funcionalidades a un proyecto existente)?

> Las estadísticas están entre el **80% y el 90%**, lo que significa que representa la gran mayoría del tiempo.

Así que necesitamos una manera o método de poder reducir los costes del tiempo asociado al mantenimiento del software. ¿Se os ocurre alguna?

El verdadero objetivo de TDD es abaratar los proyectos haciendo más barata la fase de mantenimiento. ¿Cómo lo hace?

  1. Garantizando que nuestro código funciona como debería
  2. Ayudando a que la solución y su diseño emerja de los tests (no de un trabajo previo &#8211; acordaos de la incerteza del software)
  3. Facilita un diseño desacoplado
  4. A no desarrollar más de lo estrictamente necesario para que los juegos de pruebas pasen (&#8220;baby steps&#8221; + &#8220;until you need it&#8221;)

Muchas veces he escuchado desarrolladores diciendo que &#8220;no hay tiempo&#8221; para desarrollar tests unitarios, que sale más caro, que es una pérdida de tiempo, sólo os dejo una reflexión, ¿cuánto tiempo pasáis usando el debugger? ¿cuánto tiempo pasáis resolviendo bugs que se podrían haber detectado con un test unitario?

Feliz fin de semana!