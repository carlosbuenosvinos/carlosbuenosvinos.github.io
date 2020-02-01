---
title: 'Domain-Driven Design: Logging with Domain Events Webcast at @AtrapaloEng (Spanish)'
author: Carlos Buenosvinos
type: post
date: 2015-10-19T08:30:09+00:00
url: /domain-driven-design-logging-with-domain-events-webcast-at-atrapaloeng-spanish/
categories:
  - DDD
  - PHP
  - Talks
tags:
  - atrapalo
  - ddd
  - domain events
  - domain-driven design
  - kibana
  - SOLID
  - SRP

---
Cuando trabajamos con aplicaciones DDD-style (aunque para las otras también aplica), una de las preguntas clásicas es como ejecutar tareas relacionadas con infraestructura desde las zonas más internas como Value Object o Entidades, sin violar la dependencias hacia capas superiores e incluso el SRP. Un ejemplo es logar información como acciones de usuario (tipo intento de acceso al sistema, alta de usuario, baja de usuario, etc.). El pasado Viernes, orientamos la formación de <a href="https://twitter.com/AtrapaloEng" target="_blank">@AtrapaloEng</a> sobre este tema. Os dejo el video y los ejercicios. Formación práctica basada en el proyecto de <a href="https://github.com/dddinphp/last-wishes" target="_blank">LastWishes</a> con múltiples ejercicios resueltos in live coding.

<!--more-->

#### Ejercicios

  1. Logar todos los eventos de dominio (los ya existentes) a disco con Monolog.
  2. Crear un nuevo evento de dominio cuando un usuario se intenta logar.
  3. No persistir en BBDD los eventos que queremos transmitir a otros Bounded Contexts.
  4. Persistir los eventos de dominio, no sólo en disco, también en Elasticsearch.
  5. Conectar un Kibana con nuestro Elastic y pintar alguna gráfica sobre los eventos logados
  6. Agregar información adicional al logging como IP, referrer, etc.

#### Vídeo

{{< youtube V2_Jnjp8PfE >}}

#### Más material

Si queréis más info sobre eventos de dominio, podéis echarle un ojo al libro <a href="https://leanpub.com/ddd-in-php" target="_blank">&#8220;Domain-Driven Design in PHP&#8221;</a> que estamos escribiendo Christian, Keyvan y yo. Podéis también consultar mi otro post <a href="http://carlosbuenosvinos.com/domain-driven-design-domain-events-and-bc-integration-webcast-at-atrapaloeng-spanish/" target="_blank">http://carlosbuenosvinos.com/domain-driven-design-domain-events-and-bc-integration-webcast-at-atrapaloeng-spanish/</a>.

&nbsp;

&nbsp;