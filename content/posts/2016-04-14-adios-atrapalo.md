---
title: Adiós Atrápalo
author: Carlos Buenosvinos
type: post
date: 2016-04-14T15:30:43+00:00
url: /adios-atrapalo/
featured_image: /posts/images/2016/04/10256834_309920905799307_2027411388540523420_n.jpg
dsq_thread_id:
  - "4746852827"
categories:
  - 'Agile &amp; Craftsmanship'
  - DevOps
  - Scrum
tags:
  - atrapalo
  - devops

---
Después de más de 2 años trabajando en [Atrápalo][1], ha llegado el momento de partir a nuevos horizontes. Los que ya me conocéis un poco sabéis cómo soy: llegar, simplificar y marchar. Mis objetivos se han cumplido y es hora de ayudar a otros equipos. Después de estos dos años, Atrápalo es una compañía respetada técnicamente por la comunidad de Barcelona. En 2015, consiguió se le <a href="http://awards.symfony.com/business" target="_blank">premiara por esa evolución</a>.

Cada integrante del equipo técnico ha hecho un trabajo increíble adoptando las nuevas dinámicas y prácticas de trabajo, tanto en desarrollo, UX y Sistemas. Casi siempre, un equipo grande es una desventaja, normalmente es lento, pero me ha sorprendido cómo un equipo de casi 100 personas, aplicando Scrum, eXtreme Programming y otras buenas prácticas ha conseguido reducir deuda técnica muy rápidamente. Lo que me lleva a la frase que repito entre amiguetes: &#8220;There is no legacy code, just legacy teams&#8221;.

<!--more-->

Aún recuerdo el primer test unitario (falta de formación, de hábito, poca cobertura, dudas sobre su utilidad, etc.), hoy la web tiene más de 9500 test unitarios, sin contar todas las nuevas APIs, más de 6, que han nacido y desarrollado con los beneficios del testing unitario en vena.

Así como <a href="https://carlosbuenosvinos.com/atrapalo-tech-2014-figures/" target="_blank">2014</a>, se centró en mejorar el proceso de desarrollar código, 2015 ha estado centrado en la infraestructura física. El equipo de sistemas y helpdesk han hecho un trabajo increíble para acabar con una infraestructura simple, estándar, fácil de administrar y mantener, sin parches.

<span style="font-weight: 400;">Como resumen y curiosidad: unas 400 Virtual Machines corriendo con VMware sobre unos 15 ESX. 2 HAProxy v1.6 balanceando todos los servicios. 4 Nginx v1.9 haciendo de Reverse Proxy Cache configurados para HTTP2. 20 PHP-FPM (7.0.5 + Opcache) para la web (16 cores, 16 Gb), 2 RabbitMQ servers y 15 máquinas para workers (8 cores, 32 RAM Gb). 1 Master y 5 Slaves Percona 5.6 servers, 35 Redis v3.0.7 (8 GB cada uno, 300 Gb en total persistiendo clave-valor y datos estructurados) balanceados usando 2 twemproxy. 9 Elastics (2 en modo cliente, 3 masters y 4 data. Un total de 300 millones de documentos). Para logs y análisis en tiempo real, 7 Elastics (2 client mode, 3 master and 2 data) y 1 Kibana. Para testing automatizado, 5 máquinas con selenium. Todo gestionado con Ansible.</span>

Me gustaría agradecer el cariño de todos aquellos que han ayudado a hacer posible la mejora del equipo, habéis sido la mayoría. También a los que han sido críticos porque han ayudado a tomar mejores decisiones. Por último, a los que han sido tóxicos y destructivos porque sin ellos no sería lo mismo. Agradecer a la dirección la oportunidad de participar en un proyecto tan fantástico. Por último, agradecer especialmente a Álvaro y Christian su fé, rigor y energía.

Para los que estuvisteis teletrabajando, de vacaciones o no pudisteis estar por cualquier motivo, os dejo los 15 minutos de despedida del día que lo anuncié públicamente al equipo. Gracias de nuevo.

{{< youtube 7jxbkWK4bac >}}

 [1]: https://www.atrapalo.com