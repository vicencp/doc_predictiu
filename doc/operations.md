# Operaciones

## Añadir una nueva flota


***Cambios sobre Grafana***

Por cada nueva flota, se debe crear un clon de los siguientes dashboards:

*  Vista Errores por Vehiculo EMT
*  Detalle de fuera de límites EMT

Para clonar un dashboard, es suficiente con entrar en el modelo a clonar, acceder a los settings del dashboard, y clickar sobre "Save as...".

A continuación, se debe editar "Errors Global View", para que en su panel "Navegación" incluya un enlace a estos nuevos dashboards.

Finalmente, se deben editar los nuevos dashboards para que filtren los vehículos que no pertenecen a la flota con la que debe trabajar el dashboard: en el campo **Regex** de la configuración de la variable ***vehiculo*** debe filtre por el prefijo con el nombre de la flota:

```
    /^EMT.*/
```

***NOTA IMPORTANTE SOBRE EL NOMBRE DE VEHICULOS***

Los vehículos deben llevar en el nombre un prefijo que identifique la flota a la que pertenecen. Este prefijo se utiliza en ```Grafana``` para filtrar vehículos pertenecientes a cada flota (si bien es cierto que también podría utilizarse el campo *indice* que viene en los datos y que contiene el nombre de la flota):

![](doc/img/vehicle_name_change_in_go.png)


***Cambios sobre telemetría***

Por cada flota se debe crear un nuevo alias sobre el índice 'main' (en Elasticsearch).

Después, en el registro de la tabla 'client_configuration' (esquema 'db_telemetry_core') correspondiente a la flota, actualizar la URL de Elasticsearch (poner la de Masats).

Los vehículos de la flota deben estar enviando datos a su índice correspondiente.

Por cada flota, en las properties del API de aumentación (ips_telemetry_lambda) se debe añadir el id de la cuenta a la lista 'clients.with.doors'.


***Cambios sobre cálculo de percentiles***

En el Spreadsheet "PR18020 - Percentiles configuración" añadir una copia de todas las variables para la nueva flota.
Para ello, es necesario saber qué tipo de puertas (y rampas) montan los vehículos de la flota, y activar únicamente los parámetros asociados a estas puertas (y rampas).

Lo más sencillo es partir de un modelo preexistente y copiar en bloque todas las variables del modelo.

***Cambios sobre el script de generación de alarmas***

El script ```alarmas_masats``` contiene referencias a los *alias* de índices donde residen los datos que se usan para la generación de alarmas.
Es necesario incluir los *alias* que contienen los datos de las nuevas flotas recién creadas.

```console
    #!/bin/sh

    curl -gsG https://search-predictivo-masats-5aml7qgszk3zltmyhwrwhqi4p4.eu-central-1.es.amazonaws.com/tmb_masats@,emt_madrid@,masats_singapur@/_search --data-urlencode size=1000
```

Debe añadirse los *alias* necesarios justo antes de la directiva *_search* de la URL de elasticsearch.
