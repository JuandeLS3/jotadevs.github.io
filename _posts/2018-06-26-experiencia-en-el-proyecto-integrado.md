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
También he implementado funciones a usar a la hora de realizar notificaciones al usuario, haciendo uso de la siguiente función en js:

{% highlight javascript %}
// Función que obtiene un parámetro get.
var getUrlParameter = function getUrlParameter(sParam) {
    var sPageURL = decodeURIComponent(window.location.search.substring(1)),
        sURLVariables = sPageURL.split('&'),
        sParameterName,
        i;

    for (i = 0; i < sURLVariables.length; i++) {
        sParameterName = sURLVariables[i].split('=');

        if (sParameterName[0] === sParam) {
            return sParameterName[1] === undefined ? true : sParameterName[1];
        }
    }
};
{% endhighlight %}

El código que he usado para realizar peticiones ajax ha sido el siguiente:

{% highlight javascript %}
$.post("php/filter.php", {tipo_actividad: tipo_actividad}, function (data) { // Le pasamos el tipo de actividad, que es lo que se procesará en servidor
        $(".tabla").empty();
        data = $.parseJSON(data);
        /* Bloque de comprobación de disponibilidad de ofertas */
        if (data.length === 0) {
            $(".tabla").append("<div id='titulo_filtro'><h3>Actividades de " + tipo_actividad + "</h3><hr style='width: 90%'/>");
            $("#no_offers").remove();
            $(".offers").append("<div id='no_offers'><span class='alert alert-info'><span class='glyphicon glyphicon-info-sign'></span> No hay ofertas disponibles con el filtro seleccionado</span></div>");
            $("#cargar").hide();
        } else {
            $("#cargar").show();
            $("#no_offers").remove();
            $(".tabla").append("<div id='titulo_filtro'><h3>Actividades de " + tipo_actividad + "</h3><hr style='width: 90%'/>");
            for (var i = 0; i <= data.length-1; i++) {
                $(".tabla").append(
                    "<div class=\"snip1208 col-md-4 col-xs-12 col-lg-3 col-sm-6\">" +
                    "<div class=\"aaa\">"+
                    "<img src='img/oferta/"+data[i].imagen_oferta+"' alt='sample66'/>"+
                    "<figcaption>"+
                    "<h3 id='nombre'>"+data[i].nombre+"</h3>"+
                    "<p id='actividad'>Actividad: " + data[i].tipo_actividad + "</p>" +
                    "<p id='provincia'>Provincia: " + data[i].provincia + "</p>" +
                    "<p id='dificultad'>Dificultad: " + data[i].dificultad + "</p>" +
                    "<p id='precio'>Precio " + data[i].precio + "€</p>"+
                    "<button>Ver actividad</button>"+
                    "</figcaption><a href='content/oferta.php?id="+data[i].id+"'></a>"+
                    "</div></div>");
            }
        }
    });
{% endhighlight %}

El código anterior, recibe de un radio un tipo de actividad, para enviarsela a través de $.post por ajax a filter.php, el cual tiene el siguiente código:

{% highlight php %}
/* Si se recibe solo una actividad... */
if (isset($_POST['tipo_actividad']) && empty($_POST['precio'])) {
    $actividad = $_POST['tipo_actividad']; // Actividad a filtrar por el usuario
    $res=[];
    $sql = "select * from oferta where tipo_actividad = "."'$actividad'" . " ORDER BY fecha_inicio DESC LIMIT 50";

    $resultado = $conexion->query($sql);

    while($row = $resultado->fetch_object()){
        $fila=array(
            "id"=>$row->id,
            "nombre"=>$row->nombre,
            "tipo_actividad"=>$row->tipo_actividad,
            "provincia"=>$row->provincia,
            "dificultad"=>$row->dificultad,
            "precio"=>$row->precio,
            "imagen_oferta"=>$row->imagen_oferta
        );
        array_push($res, $fila);
    }
    echo json_encode($res);
    $resultado->free();
    $conexion->close();
}
{% endhighlight %}

El código anterior recibe el tipo de actividad y ejecuta la query en la base de datos, para finalmente enviarla de nuevo al js, que dibujará el html con los datos recibidos de la bd.

![small image]({{site.baseurl}}/images/filtros.png)

## Resultado final
El resultado final siempre es mejorable, de hecho está en github y es público para todos los públicos curiosos. En mi opinión, han faltado mejores resultados en ciertos aspectos de diseño en algunas páginas estáticas. También hecho en falta ciertas funcionalidades que mejorarían la experiencia de usuario, como el envío de mensajes desde el formulario sin usar mailto, entre otras muchas cosas "secundarias" que faltaron por falta de tiempo.

Aún así, estoy orgulloso del resultado final de la aplicación y creo que cumple, tanto en funcionalidad como en diseño lo pedido para esta entrega de proyecto integrado. 

![small image]({{site.baseurl}}/images/quienes.png)

## Mi experiencia
Personalmente me ha encantado este proyecto y me ha ayudado muchísimo a aprender cosas básicas e intermedias de PHP, cosas que al pelearte día si y día también coges del bolsillo el día mañana y lo implementas sin apenas dificultad *(¡siempre hay errores inesperados!*) 

Ha sido fantástico poder trabajar con mis 4 compañeros en esta corta pero frenética, e inolvidable experiencia que supone el proyecto integrado para el I.E.S Polígono Sur. También agradecer a nuestros profesores por ser justos a la vez que justificados con la corrección.
