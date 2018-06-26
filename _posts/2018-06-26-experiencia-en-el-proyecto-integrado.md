---
layout: post
title: Proyecto integrado 2018
description: Os hablo de mi experiencia en el desarrollo del PI 2018 para el IES Polígono Sur
categories: code
author: juandels3
---

¡Hola a tod@s!

Espero que estéis teniendo un genial comienzo en esta calurosa semana de verano, al menos en Sevilla.
Hoy quiero escribiros sobre qué y cómo ha sido mi experiencia en el **proyecto integrado para el I.E.S Polígono Sur este 2018**. Prometo que no habrá sustos al respecto, así que preparen las palomitas.

El proyecto ha sido una larga travesía de 3 meses, 90 días aproximadamente y muchas horas de trabajo y aprendizaje por detrás para presentar una entrega que dejara nuestra marca en el instituto. Y sí, no me he equivocado, reitero cuando digo "nuestra", debido a que el proyecto ha sido desarrollado por 4 compañeros y por mí, realizando un trabajo y feedback casi diario para llevar a cabo tareas en cada uno de los aspectos.

## ¿Qué es?
El proyecto se trata de una aplicación web llamada "WildSports"; un portal intermediario entre empresa y usuario que realiza búsquedas de actividades al aire libre a nivel mundial, donde existen dos tipos de perfiles:

 - **Empresas**: son las que publican las actividades, detallando ciertos campos de información, entre ellos el tipo de actividad (paracaidismo, senderismo, etc...) y el precio, entre muchas otras. Cada empresa tiene un perfil que puede visualizarse por los usuarios y la competencia (otras empresas). 
 - **Usuarios**: pueden reservar dichas actividades siempre que se encuentren en el plazo adecuado, pudiendo dar una valoración y reseña por dicha actividad, editar su perfil, entre otras cosas.

> El propósito de WildSports era crear un portal que facilite las reservas de actividades al aire libre, evitando el papeleo y gasto administrativo que supone gestionar reservas entre empresa y usuario y dándole a ambos perfiles las herramientas necesarias para poderlo llevar a cabo.

![small image]({{site.baseurl}}/images/web-completa.png)

## Preparación del proyecto
La primera fase del proyecto ha sido desarrollar la lógica de negocio de la aplicación, intentando ultimar cada uno de los detalles. Tuvimos que elaborar los respectivos esquemas relacionales de la aplicación para generar la base de datos que contendría finalmente USUARIOS, EMPRESAS, RESERVAS y OFERTAS.
Después tratamos las tecnologías que ibamos a usar tanto en front-end como en back-end (teniendo que haber sido éstas estudiadas a lo largo de los dos años de enseñanza en el centro). Nos decantamos por usar **HTML5, CSS3, Javascript, JQuery y uso de componentes en front** y **PHP sin framework en back.**
Para finalizar lo que fue la primera fase, elaboramos los wireframes del portal, ubicando cada componente en su lugar y detallando al máximo posible qué y dónde irá ubicado, para una mejor maquetación llegado el momento.

![small image]({{site.baseurl}}/images/wireframe.png)

Decidimos repartirnos en distintas partes de la aplicación, **tocándome a mi la parte back-end de la aplicación**.

## Cómo ha sido el desarrollo
El workflow ha sido un constante cúmulo de comunicación entre los miembros del equipo, usando como medios principalmente una memoria, en la que se detalla qué y cuándo a realizado una tarea un miembro del equipo o si ha tenido problemas, cómo lo ha solucionado. También hemos usando una ficha de incidencias, donde se han detallado los mayores fallos de la aplicación, el miembro del equipo asignado a solucionarlo y la gravedad. Éstas dos fichas las elaboramos en Google Drive debido a la comodidad que nos daba a todos. A parte de esto, usábamos Whatsapp o incluso pequeñas reuniones para hablar de los objetivos a cumplir, que eran ni más ni menos que hitos marcados por el instituto en una fecha determinada en la que había que terminar una tarea.

## Mi trabajo en el proyecto
Como ya he dicho anteriormente, mi cometido en el proyecto ha sido la parte back-end de la aplicación. He puesto especial énfasis y esfuerzo en los filtros de búsqueda de las actividades, pudiendo filtrarlas por una lista de tipos de actividades dinámicas, que cambian conformen se añadan más tipos de actividades en la base de datos, por provincia y precio. También quiero destacar que estos filtros funcionan mediante ajax, y por tanto las peticiones a la bd se hacen sin recargar la página, dando una mejor experiencia al usuario final. También he implantado ajax en otras funcionalidades, como la validación en el inicio de sesión.
Muchas otras funcionalidades las he trabajado en conjunto con mi compañero de back.

![small image]({{site.baseurl}}/images/filtros.png)

## Resultado final
El resultado final siempre es mejorable, de hecho está en github y es público para todos los públicos curiosos. En mi opinión, han faltado mejores resultados en ciertos aspectos de diseño en algunas páginas estáticas. También hecho en falta ciertas funcionalidades que mejorarían la experiencia de usuario, como el envío de mensajes desde el formulario sin usar mailto, entre otras muchas cosas "secundarias" que faltaron por falta de tiempo.

Aún así, estoy orgulloso del resultado final de la aplicación y creo que cumple, tanto en funcionalidad como en diseño lo pedido para esta entrega de proyecto integrado. 

![small image]({{site.baseurl}}/images/quienes.png)

## Mi experiencia
Personalmente me ha encantado este proyecto y me ha ayudado muchísimo a aprender cosas básicas e intermedias de PHP, cosas que al pelearte día si y día también coges del bolsillo el día mañana y lo implementas sin apenas dificultad *(¡siempre hay errores inesperados!*) 
Mi nota en este proyecto ha sido de un 9; no tengo nada que decir en contra, debido a que un 10 era excesivo por lo comentado anteriormente.

Ha sido fantástico poder trabajar con mis 4 compañeros en esta corta pero frenética, e inolvidable experiencia que supone el proyecto integrado para el I.E.S Polígono Sur. También agradecer a nuestros profesores por ser justos a la vez que justificados con la corrección. Esto fue el primer proyecto de alta importancia, pero no el último, ¡eso está claro!
