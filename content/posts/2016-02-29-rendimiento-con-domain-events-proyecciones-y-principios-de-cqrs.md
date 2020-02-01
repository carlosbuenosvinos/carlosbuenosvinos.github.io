---
title: Rendimiento con Domain Events, Proyecciones y principios de CQRS
author: Carlos Buenosvinos
type: post
date: 2016-02-29T07:00:38+00:00
url: /rendimiento-con-domain-events-proyecciones-y-principios-de-cqrs/
featured_image: /posts/images/2016/02/commands-and-domain-events.jpg
categories:
  - Databases
  - DDD
  - PHP
tags:
  - cqrs
  - ddd
  - domain events
  - domain-driven design
  - performance
  - php

---
Cuando desarrollamos una aplicación nueva, todo va muy rapidito. Hay poco tráfico, pocas queries y si hay alguna más &#8220;dura&#8221; usamos alguna cache como Memcached o Redis. Pero a medida que agregamos más funcionalidad a una página, el número de queries a base de datos u otras infraestructuras va creciendo. Hasta que sin saber cómo, haces 300 queries, y no es broma, en la ficha de algún producto.

El problema es que estamos acostumbrados a hacer muchas queries de lectura y muy pocas de escritura en estructuras bastante normalizadas. Eso escala mal en base a nueva funcionalidad. Un buen approach en busca del máximo rendimiento es la consistencia eventual, estructuras desnormalizadas y proyecciones.

Os dejo el video de la formación de @AtrapaloEng sobre cómo el uso de Eventos de Dominio y el uso de conceptos de CQRS nos pueden ayudar enormemente a mejorar el rendimiento de nuestras aplicaciones.

<!--more-->

{{< youtube 3PssQeA8cEs >}}