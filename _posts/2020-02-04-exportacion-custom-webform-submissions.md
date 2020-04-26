---
layout: post
title: Exportación custom de submissions (Webform)
description: Genera un link custom que descarga todas las submissions de un webform asociado
categories: code
author: juandels3
---

![small image]({{site.baseurl}}/images/webform_post.jpg)

¡Hola a tod@s! Después de unos meses de inactividad, me apetecía escribir y compartir algo de Drupal con todos ustedes. En este artículo hablaremos un poco -sin extendernos- de Webform para Drupal 8 y un caso específico: cómo generar una exportación de webform submissions en un campo custom.

Pero como siempre, vayamos por partes...

## Qué es Webform

Webform es un [módulo contribuido](https://www.drupal.org/project/webform), de hecho es uno de los módulos más completos y flexibles a la hora de crear formularios en Drupal. Todo esto junto con una interfaz amigable de cara al usuario y una amplia gama de field types (textfield, textarea, list, radio groups... etc.) Webform tiene una version estable tanto para Drupal 7 como para Drupal 8 y es usado en más de **478 mil sitios web Drupal**.

## Exportación custom de Webform submissions

Una de las necesidades que un proyecto requería, era facilitar al usuario administrador, un botón o link que le permitiera descargar todas las submissions. Las Webform submissions son **los envíos que un usuario hace en un webform**, es decir, cuando un usuario completa un formulario y hace el 'submit'. Las submissions se recogen en la interfaz de administración de Drupal, concretamente en la sección 'Webform' del menú 'Estructura'.
Ahora bien, pensarás:

> ¿Por qué crear un enlace o botón, si las submissions ya tienen su lugar en el menú de administración?

Con esto conseguirás un link custom que podrás colocar en una vista o en un bloque de tu portal, dando al usuario mucha flexibilidad y organización a la hora de descargar el reporte de envíos de un webform. Además tienes una ventaja extra: ¡al ser codificado podrás elegir el formato de salida (normalmente se usa .csv) y otras muchas opciones que veremos más adelante! Por ejemplo, puedes colocar este link en una vista que esté filtrando algún tipo de contenido relacionado con el webform, de modo que el usuario X pueda obtener este reporte de una manera muy amigable.
Una vez hecho una pequeña explicación a los casos de uso, ¡vamos al tema!

En primer lugar, tenemos que crear un field plugin para usar las vistas. No quiero extenderme demasiado en esto, ya que lo interesante viene luego. Crearemos un plugin básico que genere un link. Recuerda que para esto es necesario crear un módulo custom, y crear este plugin en **/test_results/src/Plugin/views/field**.

    <?php
     namespace Drupal\test_results\Plugin\views\field;
    
    use Drupal\Core\Url;
    use Drupal\views\Plugin\views\field\FieldPluginBase;
    use Drupal\views\ResultRow;
    
    /**
     * Field handler to download results.
     *
     * @ingroup views_field_handlers
     *
     * @ViewsField("download_results")
     */
        class DownloadResults extends FieldPluginBase {
      /**
       * @{inheritdoc}
       */
      public function render(ResultRow $values) {
        $node = $values->_entity;
        $webform_id = $values->_entity->get('webform')[0]->target_id;
        $params = ['wid' => $webform_id];
    
        $output['results_link'] = [
          '#title' => t('Download CSV'),
          '#type' => 'link',
          '#url' => Url::fromRoute('test_results.export_webform_results', $params)
        ];
    
        return $output;
      }
    }

Este plugin '**DownloadResults**' no es más que "el transporte" a la ruta 'test_results.export_webform_results', que se encargará de pasar los parámetros necesarios -*webform_id en este caso*- al controlador. Además del plugin anterior, es necesario añadir lo siguiente en nuestro 'test_results.views.inc' para que el campo custom sea detectado y reconocido por las vistas, ya que sino no nos saldrá en el listado de campos disponibles:

      $data['node']['download_results'] = [
        'title' => t('Download CSV link'),
        'field' => [
          'title' => t('Download CSV link'),
          'help' => t('Download Webform submissions results with a CSV file.'),
          'id' => 'download_results',
        ],
      ];

El siguiente paso es configurar el **test.routing.yml**:

    test_results.export_webform_results:
      path: '/results/{wid}'
      defaults:
        _controller: '\Drupal\test_results\Controller\WebformExportResults::export'
      requirements:
        _permission: 'administer site configuration'

Esta ruta se encargará de pasar al controlador el parámetro necesario, que es el webform_id en este caso.
Y por último vamos a crear el controlador, que se encargará de que el link genere una salida con todos los webforms submissions.

    <?php
    namespace Drupal\test_results\Controller;
    
    use Drupal\Core\Controller\ControllerBase;
    use Drupal\webform\Entity\Webform;
    use Symfony\Component\HttpFoundation\BinaryFileResponse;
    use Symfony\Component\HttpFoundation\Request;
    
    
    class WebformExportResults extends ControllerBase {
    
      /**
       * Generate a CSV file.
       *
       * @param \Symfony\Component\HttpFoundation\Request $request
       *   The current request.
       * @param string $wid
       *   The webform_id parameter.
       * @param bool $download
       *   Download the generated CSV file. Default to TRUE.
       *
       * @return \Symfony\Component\HttpFoundation\Response
       *   A response object containing the CSV file.
       */
    	public function export(Request $request, $wid = NULL, $download = TRUE) {
    	  // Load the webform and the export configuration, then modify it and generate a binary file response.
        $webform = Webform::load($wid);
        $submission_exporter = \Drupal::service('webform_submission.exporter');
        $export_options = $submission_exporter->getDefaultExportOptions();
        $submission_exporter->setWebform($webform);
        $submission_exporter->setExporter($export_options);
        $submission_exporter->generate();
        $file_path = $submission_exporter->getExportFilePath();
        $file = file_get_contents($file_path);   
        $response = new BinaryFileResponse($file_path, 200, [], FALSE, $download
          ? 'attachment' : 'inline');
        $response->deleteFileAfterSend(TRUE);
    
        return $response;
    	}
    }

Con esto tendríamos un campo preparado para colocarse en una vista, el cual imprimiría un link que descargaría al usuario un .csv con todos los submissions del webform asociado. Pero aquí hay mucho más, ya que podemos cambiar a nuestro gusto las opciones de exportación ($export_options ), excluyendo campos los campos que deseemos a la hora de exportar los datos.
