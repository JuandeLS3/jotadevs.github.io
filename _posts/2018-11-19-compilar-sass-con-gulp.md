---
layout: post
title: Compilar Sass con Gulp.js
description: Aprovecha las ventajas que trae gulp.js a la hora de compilar Sass en tu proyecto
author: juandels3
categories: code
---

![small image]({{site.baseurl}}/images/gulpjs.png)

¡Buenas a tod@s!  
  
Hoy vamos a dar la vuelta a la tortilla, y nos desplazaremos un poco al lado front del desarrollo web, algo atípico en este blog, pero bueno, siempre es importante saber un poco de todo en este amplio mundillo. Para aprender a compilar Sass, primero debemos saber qué es, así que empezaremos definiendo un poco Sass.  
Según [wikipedia](https://es.wikipedia.org/wiki/Sass_%28lenguaje_de_hojas_de_estilo%29):  
  
> **Sass** (_Syntactically Awesome Stylesheets_) es un lenguaje de hoja de estilos inicialmente diseñado por Hampton Catlin y desarrollado por Nathan Weizenbaum.  
  
Y en realidad, no es más que eso, un metalenguaje de CSS, **restrictivo pero muy organizado y limpio**, donde cada bloque de código se estructura según la indentación y formas para evitar el copy & paste de código, evitando código repetido una y otra vez. La estructura de un proyecto Sass es normalmente la siguiente:  
  
- base/  
- componentes/  
- layout/  
- páginas/  
- temas/  
- utilidades/  
- proveedores/  
  
Para convertir los Sass en CSS y así reflejar los cambios en nuestro proyecto, es necesario compilar los ficheros Sass, podemos hacerlo **uno a uno** con el siguiente comando:  
  
 sass source/stylesheets/index.scss build/stylesheets/index.css  
Pero claro, para qué queremos compilar ficheros uno a uno cada vez que hagamos un cambio en el código, teniendo la opción de lanzar un script que se encarga de detectar cambios en los ficheros Sass y compilarlos automáticamente para reflejar estos cambios al instante en nuestro proyecto. Esto ya es otro nivel, hablamos de **Gulp.js**. Gulp es un sistema de construcción o *build system* cuya función es automatizar tareas al desarrollador, tales como compilación, validación, errores de sintaxis, entre otras. Está desarrollado en JavaScript y funciona sobre **Node.js**, el cual procederemos a instalar a continuación.  
  
Para instalar Node.js, haremos uso de NVM, que es el "Administrador de Versiones de Node.js". Con NVM podemos instalar varias versiones separadas e independientes de Node.js, que permitirán controlar el entorno más fácilmente. En nuestro caso, utilizaremos NVM para instalar la última versión estable de Node.js y mantenerlo actualizado. En primer lugar actualizaremos nuestros paquetes fuente:  
  
 sudo apt-get update sudo apt-get install build-essential libssl-dev   
A continuación, descargaremos con cURL el script de instalación de NVM desde [la página del proyecto en Github](https://github.com/creationix/nvm).  
  

     curl -sL https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh -o install_nvm.sh 

  
El número de la versión puede variar con el tiempo, solo tendríamos que ir a "**Releases**" y cambiar si es necesario la versión de NVM. Ahora procedemos a ejecutar el script.  
  

     bash install_nvm.sh  

NVM se instalará en un subdirectorio del directorio principal ~/.nvm. También agregará unas líneas al ~/.profile, por tanto debemos reiniciarlo para aplicar cambios.  
  

     source ~/.profile  

Ahora que se ha instalado NVM, podemos instalar versiones independientes de Node.js. Para averiguar de entre las versiones disponibles la estable, ejecutaremos el comando:  
  

     nvm ls-remote  

Esto nos imprimirá en pantalla una lista con todas las versiones disponibles. Debemos instalar la última versión **LTS Estable**, que actualmente es la v8.12.0, pero es variará con el tiempo.  
  

     nvm install v8.12.0  

Indicaremos a NVM que queremos usar esta versión.  
  

     nvm use v8.12.0  

Comprobaremos la versión de Node.js instalada.  
  

     node -v  

Ya tenemos instalado Node.js correctamente y su gestor de versiones NVM, el cual nos permitirá fácilmente actualizarlo o escoger versiones en particular. Ahora es el turno de instalar Gulp.js. En primer lugar, instalaremos gulp-cli, que es una utilidad de gulp el cual nos permite usar gulp en proyectos o de forma global.  
  

     npm install gulp-cli -g  

Indicando la opción -g le estamos diciendo que queremos instalarlo de forma global para usarlo en cualquier proyecto. Ahora instalaremos gulp.  
  

     npm install gulp  

Este proceso puede tomar su tiempo, pero una vez finalizado tendremos instalado Gulp de forma global. Antes de poder usarlo, debemos crear un nuevo fichero de configuración para gulp, denominado **gulpfile.js**.
Este fichero debe estar colocado en la raíz del proyectom y debe tener una estructura con los siguientes elementos:

-   La importación de otros módulos
-   La importación de un fichero de configuración del proyecto (opcional)
-   La definición de las tareas
-   Observadores que se ejecutan en función de ciertos cambios (opcional)
-   Una tarea (tasks) por defecto a ejecutar

Eso significa que el gulpfile.js se debe crear dependiendo de las necesidades del proyecto. La propia web oficial de gulp nos facilita un comando para comenzar el gulpfile.js:

```
npx -p touch nodetouch gulpfile.js
```

Sin embargo, aquí os dejo una configuración básica.
  
```javascript  
// Gulp.js configuration  
var  
 // modules  gulp = require('gulp'),  
 // development mode?  devBuild = (process.env.NODE_ENV !== 'production'),  
 // folders  folder = {  src: 'src/',  
  build: 'build/'  
 };  
  
```  

Si queremos crear tareas o *'tasks'*, debemos indicarlo en este fichero. En esta entrada, crearemos dos task importantes, pero puedes crear o encontrar por internet googleando muchos más con funcionalidades interesantes.

Gulp watch. Nos permite dejar "a la escucha" gulp para compilar nuestros ficheros Sass en el CSS. También detectará fallos de sintaxis.


        gulp.task('watch', function(){
        gulp.watch(srcAssets.styles + '**/*.s+(a|c)ss', ['styles:dev'])
            .on('change', function(event) {
                console.log('');
                console.log('-> File ' + event.path.magenta.bold + ' was ' + event.type.green + ', running tasks css...');
            });
    
        gulp.watch(distAssets.js + '**/*.js', ['jshint'])
            .on('change', function(event) {
                console.log('');
                console.log('-> File ' + event.path.yellow + ' was ' + event.type.green + ', running tasks js...');
            });
        gulp.watch(srcAssets.images + '**/*', ['imagemin'])
            .on('change', function(event) {
                console.log('');
                console.log('-> File ' + event.path.yellow + ' was ' + event.type.green + ', running tasks images...');
        });
    });

Gulp styles:pro. Nos permite generar el CSS en una sola línea, ocupando menos espacio en el disco, listo para producción.

        gulp.task('styles:pro', ['clean:css'], function () {
        return gulp.src([srcAssets.styles + '**/*.s+(a|c)ss'])
            .pipe(sassGlob())
            .pipe(sass({
                errLogToConsole: true,
                outputStyle: 'compressed'
            }).on('error', sass.logError))
            .pipe(postCss([
                autoprefixer({
                    browsers: ['> 1%', 'ie 8', 'last 2 versions'] }
                )
            ]))
            .pipe(gulp.dest(distAssets.styles));
	    });

Al final, acabaremos con un fichero **gulpfile.js** con tantas 'tasks' como necesitemos en el proyecto.

Para proceder a usar gulp, basta con lanzar el comando:  
  

     gulp watch 

 
Ahora quedará en "espera" hasta que haya cambios en los ficheros Sass del proyecto. Una vez los haga, irá compilando y detectando errores de sintaxis de forma automática, por lo que no hay que perder de vista la shell. Para realizar una compilación de CSS eficiente en un entorno de producción, lanzaremos el comando:  
  

     gulp styles:pro  

 
Esto compilará los cambios en el CSS en una sola línea.  
  
Con gulp listo y funcionando, notaremos notables diferencias a la hora de perder tiempo en tareas básicas y repetitivas y emplearlo en lo que realmente importa: desarrollar.