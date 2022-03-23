---
layout: post
title: Crear comandos en Drush
description: Cómo crear un proceso por lotes mediante un comando drush (¡2x1!)
categories: code
author: juandels3
---

![small image]({{site.baseurl}}/images/drush_logo.png)


¡Hola! Si quieres aprender Drupal, estás en un buen lugar.

En esta ocasión, quiero compartir cómo crear tu propio Drush command (comando drush) desde PHP y lanzar un proceso por lotes (batch). Tranquilo si te abrumo con tanto término nuevo, ¡al final no es para tanto!

## Qué es un drush command

Para saber qué es un drush command, primero hay que saber qué es drush.

Drush es una herramienta -LA HERRAMIENTA- de consola utilizada en Drupal, que sirve para agilizar tareas cotidianas o repetitivas; en pocas palabras, drush facilita la vida del desarrollador que trabaja en un proyecto Drupal.

Entre muchas otras cosas, drush trae por defecto scripts que nos permiten borrar cachés, crear usuarios temporales, poner el site en mantenimiento, habilitar/deshabilitar módulos, actualizar contraseñas de usuarios, y un largo etc...

¿Pero qué pasa si aún no es suficiente lo que drush te ofrece por defecto? ¡Pues que creas **tu propio drush command**!

## Casos de uso

Lanzar un proceso por lotes desde tu propio comando drush es un método correcto a la hora de tener que ejecutar cualquier modificación masiva en tu sitio web, como por ejemplo, actualizar un campo de N contenidos cuyo tipo de contenido sea X, o eliminar todos los usuarios de un determinado rol. Cualquier cosa es posible, **ya que tenemos la API de Drupal** por delante.

## Pongámoslo en práctica

Vamos a crear un drush command que nos modifique de forma automática el título de todos los contenidos de un determinado tipo de contenido.
Para empezar, vamos a definir la siguiente estructura de archivos:

    \migration_commands
			\src
				BatchService.php
				\Commands
					GeneralCommands.php
			drush.services.yml
			migration_commands.info.yml

- BatchService.php: será la clase PHP que define la lógica del proceso por lotes.
- GeneralCommands.php: será la clase PHP en la que se definen todos los comandos que se quieran crear.
- drush.services.yml: aquí se define el servicio drush.command para ser inyectado en la clase GeneralCommands.php
- migration_commands.info.yml: aquí se define la información básica de cualquier módulo o tema de Drupal.

Primero vamos a definir el **servicio** de la siguiente forma.

    services:
      migration_commands.commands:
        class: \Drupal\migration_commands\Commands\GeneralCommands
        arguments: []
        tags:
          - { name: drush.command }

Aquí es importante introducir la ruta correcta de la clase en la que vamos a crear los comandos; en este caso, GeneralCommands. En la directiva 'tags' introducimos el alias que usaremos en la inyección de dependencias, que será 'drush.command'. Puedes leer más sobre las etiquetas de servicio aquí: [https://www.drupal.org/docs/8/api/services-and-dependency-injection/service-tags](https://www.drupal.org/docs/8/api/services-and-dependency-injection/service-tags)

Ahora vamos a crear la clase GeneralCommands. Aquí vamos a crear todos los comandos drush que queramos, pero no será donde esté la lógica del comando en sí.

    class GeneralCommands extends DrushCommands
    {
    
    /**  
     * Change node titles of content type. 
     *  
     * @command general-commands:change-node-titles 
     *  
     * @usage drush general-commands:change-node-titles
     * 
     * @aliases gc-change-titles
     */
		public function changeNodeTitles() {
		
			// Inicializar variables.
			$operations = [];
			$num_operations = 0;
			$batch_id = 1;
			$content_type = 'news';
		
			// Aquí obtenemos el listado de nodos que vamos a procesar.
			$nodes = \Drupal::entityQuery('node')->condition('type', $content_type, 'IN')->execute();
			
			foreach ($nodes as $nid) {
				// Preparamos la operación. Por cada nodo obtenido en $nodes, creamos una operación.
				$operations[] = [
				'\Drupal\migration_commands\BatchService::processChangeNodeTitles',
				[
					$batch_id,
					$nid,
					t('Procesando nodo con id @nid, ['@nid' => $nid]),
					],
				];

				$num_operations++;
				$batch_id++;
			}
			
			// Crear el batch.
			$batch = [
				'title' => t('Comprobando @num items', ['@num' => $num_operations]),
				'operations' => $operations,
				'finished' => '\Drupal\migration_commands\BatchService::processChangeNodeTitlesFinished',
			];

			// Seteamos el batch.
			batch_set($batch);

			// Lanzamos el batch.
			drush_backend_batch_process();

			// Logs de información desde consola.
			$this->output()->writeln("Batch terminado: " . $batch_id);
			$this->output()->writeln("Proceso finalizado.");
		}

La parte de comentarios arriba de la función es donde se definen, mediante anotaciones, el nombre y alias del comando que vamos a crear.
A partir del foreach el proceso siempre es el mismo: crear una operación por cada nodo obtenido en la query anteriormente lanzada ($nodes). Cada operación debe tener la clase y función donde se va a procesar el nodo X, el nid del mismo y otros argumentos que tu quieras enviar.
Una vez creadas todas las operaciones, se setea el batch y después se ejecuta, con "drush_backend_batch_process()".

Ahora vamos a crear la clase BatchService, que es donde crearemos la lógica a ejecutar por cada nodo que nos llegue en cada operación (por cada operación del array $operations).

    class BatchService
    {

		public function processChangeNodeTitles($id, $nid, $operation_details, &$context) {
			$context['message'] = t('Ejecutando batch con id @id', ['@id' => $id]);

			// Cargamos el nodo gracias al $nid que nos llega como argumento.
			$node = Node::load($nid);
			
			// Cambiamos el título del nodo cargado.
			$node->setTitle($node->getTitle() . ' ' . random_int(0, 20));
			$node->save();

			$context['results][] = $nid;
			$context['message'] = t('Finalizando el batch @id del nodo @nid', [
				'@id' => $id,
				'@nid' => $nid,
			]);
		}

		public function processChangeNodeTitlesFinished($success, array $results, array $operations) {
			$messenger = \Drupal::messenger();
			if ($success) {
				$messenger->addMessage(t('@count nodos modificados.', ['@count' => count($results)]));
			}
			else {
				$messenger->addMessage(t('Se han cometido errores...'));
			}
	}

En el método "processChangeNodeTitles()" es donde crearemos la lógica de cada nodo que nos llegue a través del argumento $nid, que será previamente cargado como entidad y será sometido a un cambio del campo título a través del método "setTitle". Guardamos el nodo, un par de líneas de logs para informar, ¡y listo!
El método "processChangeNodeTitlesFinished" será lanzado cuando el array $operations haya procesado todos los nodos, y servirá para indicarnos la cantidad de nodos procesados por el BatchService o para avisarnos se que ha ocurrido algún error.

Para probar este comando, es tan sencillo como instalar el módulo custom que hemos creado y escribir "drush".
En el listado, podemos ver en la categoría "_global" nuestro comando, si todo ha salido bien.

## Conclusión

¿Te ha parecido fácil? ¿Difícil? Crear comandos drush puede ser muy útil en tareas masivas, e incluso en algunos procesos de migración de contenidos, recibiendo los datos de distintas fuentes como un csv o un fichero de texto plano.

Si te ha gustado el artículo compártelo con tus panas drupaleros, y si quieres saber más sobre este tema o sobre Drupal en general, echa un vistazo a mi [Curso Completo de Drupal](https://www.udemy.com/course/curso-completo-drupal-2021/?couponCode=46C39E25F0CAE602731D), donde encontrarás mucho más contenido.

*Código promocional secreto: 46C39E25F0CAE602731D*
