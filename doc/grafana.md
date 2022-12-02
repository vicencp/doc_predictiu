# Grafana

## Manual de usuario

la documentación general de Grafana se encuentra en su [página oficial](https://grafana.com/docs/grafana/latest/)

## Acceso a grafana

Acceso a ***[Grafana](https://grafana.datik.io/login)***

El visualizador genérico de datos utilizado es Grafana. La vista general de los datos comienza con un ranking de outliers y número de bits de error activados se encuentra en el dashboard ```Errors Global View```.

Respecto al ránking de outliers, el excel para editar los límites de distintas variables se encuentra en: [PR18020 - Percentiles configuracion](https://docs.google.com/spreadsheets/d/1J3arIo5QfibjAuUFvGGKT1aMzIja39w9GlXiPBodhbg/edit#gid=0). Hay un programa que lee el excel periódicamente y crea las alertas de fuera de rango.

En cuanto al recuento de errores, se puede ver el detalle en dashboards correspondientes a cada compañía [EMT](http://grafana.datik.io/d/kxierr-EMT/vista-errores-por-vehiculo-emt)/[TMB](http://grafana.datik.io/d/kxierr-TMB/vista-errores-por-vehiculo-tmb), y también se puede ver en una pantalla de [Percentiles](http://grafana.datik.io/d/CJL_NGDWy/percentiles) la evolución en el tiempo de la variable, ello para poder ver el outlier en contexto.

Finalmente, hay 3 dashboards de contexto. El primero es uno con [Contadores Globales](https://grafana.datik.io/d/4jxbu7aWz/contadors) (métricas generales de lo consultable en Elasticsearch). Después hay uno que muestra los [Agregados de curvas de consumo](http://grafana.datik.io/d/rlmq9hRGz/curvas-de-consumo) en un intervalo de tiempo (una especie de media entre las curvas) y otro llamado [Visualizador de curvas de consumo 1 por 1](http://grafana.datik.io/d/vizcurves1by1/viz1by1) para ver las curvas de consumo una por una.

El esquema de los dashboards en forma de árbol es el siguiente:

```
├─── Errors Global View
│    ├─── Vista de errores EMT/TMB
│    │    └─── Visualizar las curvas con bit de Error ON
│    │
│    │
│    └─── Detalle de fuera de límites EMT/TMB
│         └─── Percentiles (para ver cómo ha evolucionado la variable en cuestión)
│
├─── Contadores Globales
├─── Agregados de curvas de consumo
└─── Visualizador de curvas de consumo 1 por 1
```

*  [Errors Global View](https://grafana.datik.io/d/RUvcMdRMk/errors-global-view)
*  Vista de errores [EMT](http://grafana.datik.io/d/kxierr-EMT/vista-errores-por-vehiculo-emt)/[TMB](http://grafana.datik.io/d/kxierr-TMB/vista-errores-por-vehiculo-tmb)
*  [Visualizar las curvas con bit de Error ON](http://grafana.datik.io/d/HIBTxNzGk/vizer)
*  Detalle de fuera de límites [EMT](http://grafana.datik.io/d/olim-EMT/detalle-de-fuera-de-limites-emt)/[TMB](http://grafana.datik.io/d/olim-TMB/detalle-de-fuera-de-limites-tmb)
*  [Percentiles](http://grafana.datik.io/d/CJL_NGDWy/percentiles)
*  [Contadores Globales](https://grafana.datik.io/d/4jxbu7aWz/contadors)
*  [Agregados de curvas de consumo](http://grafana.datik.io/d/rlmq9hRGz/curvas-de-consumo)
*  [Visualizador de curvas de consumo 1 por 1](http://grafana.datik.io/d/vizcurves1by1/viz1by1)


Todos los dashboards son editables y exportables por el usuario.


## Origen de los datos (data sources) de las tablas/gráficas

Data sources:
* [ES-Masats](https://search-predictivo-masats-5aml7qgszk3zltmyhwrwhqi4p4.eu-central-1.es.amazonaws.com/). Indices:
  * main
  * thresholds <span style="color:red">(vacío)</span>
  * anomalous_consumption_arrays (no se usa)


Gráficas por dashboard:

| Dashboard   |      Table/plot      |  Data Source |  Index/table |
|:----------|:-------------|:------|:------|
| Errors global view | Red COUNT | ES-Masts | main |
| Errors global view | Fuera de límites COUNT   | ES-Masts | thresholds <br> <span style="color:red">(vacío)</span> |
| Errors global view | Yellow COUNT | ES-Masts | main |
| Errors global view | Green COUNT | ES-Masts | main |
| Vista de errores EMT/TMB | p1 red | ES-Masts | main |
| Vista de errores EMT/TMB | p2 red | ES-Masts | main |
| Vista de errores EMT/TMB | p3 red | ES-Masts | main |
| Vista de errores EMT/TMB | rt1 red | ES-Masts | main |
| Vista de errores EMT/TMB | p1 yellow | ES-Masts | main |
| Vista de errores EMT/TMB | p2 yellow | ES-Masts | main |
| Vista de errores EMT/TMB | p3 yellow | ES-Masts | main |
| Vista de errores EMT/TMB | rt1 yellow | ES-Masts | main |
| Vista de errores EMT/TMB | p1 green | ES-Masts | main |
| Vista de errores EMT/TMB | p2 green | ES-Masts | main |
| Vista de errores EMT/TMB | p3 green | ES-Masts | main |
| Vista de errores EMT/TMB | rt1 green | ES-Masts | main |
| Visualizar las curvas con bit de Error ON | Maniobras con $bit_error ON | ES-Masts | main |
| Visualizar las curvas con bit de Error ON | Consumo m1 vs sample number | ES-Masts | main |
| Visualizar las curvas con bit de Error ON | Consumo m2 vs sample number | ES-Masts | main |
| Detalle de fuera de límites EMT/TMB | p1 | ES-Masts | thresholds <br> <span style="color:red">(vacío)</span> |
| Detalle de fuera de límites EMT/TMB | p2 | ES-Masts | thresholds <br> <span style="color:red">(vacío)</span> |
| Detalle de fuera de límites EMT/TMB | p3 | ES-Masts | thresholds <br> <span style="color:red">(vacío)</span> |
| Detalle de fuera de límites EMT/TMB | re1 | ES-Masts | thresholds <br> <span style="color:red">(vacío)</span> |
| Detalle de fuera de límites EMT/TMB | rt1 | ES-Masts | thresholds <br> <span style="color:red">(vacío)</span> |
| Percentiles | Número de maniobras de ${abrir_cerrar} ${puerta_rampa} en ${vehiculo} | ES-Masts | main |
| Percentiles | NUMERO_MANIOBRAS_${abrir_cerrar} (reportado por la caja de control) | ES-Masts | main |
| Percentiles | Evolución temporal del ${variable_escalar} | ES-Masts | main |
| Percentiles | Evolución temporal del ${variable_escalar} (QUITANDO SENSIBILIZACIONES) | ES-Masts | main |
| Percentiles | RECUENTO SENSIBILIZAZIONES | ES-Masts | main |
| Percentiles | Percentiles del ${variable_escalar} | ES-Masts | main |
| Contadores Globales | Histograma NUMERO_MUESTRAS_MOTOR | ES-Masts | main |
| Contadores Globales | Último dato (${abrir_cerrar}, ${puerta_rampa}) | ES-Masts | main |
| Contadores Globales | ${abrir_cerrar}, ${puerta_rampa} | ES-Masts | main |
| Contadores Globales | Recuento de datos en el índice principal | ES-Masts | main |
| Contadores Globales | Recuento de datos en el índice principal (evolución temporal) | ES-Masts | main |
| Agregados de curvas de consumo | Consumo m1 vs t | ES-Masts | samples <br> <span style="color:red">(no existe)</span> |
| Agregados de curvas de consumo | Consumo m2 vs sample number | ES-Masts | samples <br> <span style="color:red">(no existe)</span> |
| Agregados de curvas de consumo | Consumo m1 vs t (sensibilización) | ES-Masts | samples <br> <span style="color:red">(no existe)</span> |
| Agregados de curvas de consumo | Consumo m2 vs t (sensibilización) | ES-Masts | samples <br> <span style="color:red">(no existe)</span> |
| Visualizador de curvas de consumo 1 por 1 | Lista de Maniobras para ${vehiculo} en ${puerta_rampa} ${abrir_cerrar}, $__url_time_range | ES-Masts | main |
| Visualizador de curvas de consumo 1 por 1 | Consumo m1 vs sample number | ES-Masts | main |
| Visualizador de curvas de consumo 1 por 1 | Consumo m2 vs sample number | ES-Masts | main |
