# CopyCat: Procesos de integración

Los procesos de integración se encuentran en la máquina.

Repositorio Gitlab donde están documentados los [procesos de la máquina CopyCat](https://git.datik.io/ingenieria/masats/etl-scripts)

# Docker y crontab

Hay un Docker Registry ejectuándose en un Docker container:

```
$ docker ps

9b5aa0da7af5    registry:2     "/entrypoint.sh /etc…"   21 months ago   Up 6 months      0.0.0.0:5000->5000/tcp, :::5000->5000/tcp   registry
```

Tareas de cron ***del usuario ubuntu***:

```
$ contrab -e
```

![](doc/img/cron.png)

```console
PATH=/home/ubuntu/bin:/usr/sbin:/usr/bin:/sbin:/bin
ES_URL=https://search-predictivo-masats-5aml7qgszk3zltmyhwrwhqi4p4.eu-central-1.es.amazonaws.com

# m h     dom mon dow command
  0 */12  *   *   *  sync_consolidated.sh  >/dev/null 2>&1
 30  *    *   *   *  time sync_s3_data.sh  > ~/log/sync_s3_data.log  2>&1
 13 */6   *   *   *  forcemerge.sh >  ~/log/forcemerge.log 2>&1
 55  1    *   *   *  deletebyquery.sh > ~/log/last_delete_by_query.log 2>&1
*/2  *    *   *   *  masats_limits_query.sh > ~/log/last_limits_query.log 2>&1
 12  *    *   *   *  rm -f /home/ubuntu/percentiles_conf.json
```

En el repositorio [ETL Scripts](https://git.datik.io/ingenieria/masats/etl-scripts) se describe la finalidad de cada script del ```cron```.

## Procesos de analítica

A fecha de 05/10/2020, hay estos procesos:

***Proceso de Sincronización de los Datos***
Es el distribuidor mencionado antes, el que se encarga de mover y transformar los datos de un lado a otro (S3, ES, etc.) para su disponibilidad. Es un proceso que se ejecuta cada 4 horas.

***Proceso de Consolidación de los Datos***
Es el proceso que se encarga de agrupar todas las maniobras de un equipo en un fichero por un intervalo de tiempo (1 día), ficheros que después se guardan en el segundo bucket. Se ejecuta cada 2 horas.

***Proceso de Cálculo de percentiles***
El excel para editar los límites de distintas variables se encuentra en [Google Spreadsheet](https://docs.google.com/spreadsheets/d/1J3arIo5QfibjAuUFvGGKT1aMzIja39w9GlXiPBodhbg). Hay un programa que lee el excel periódicamente y crea las alertas de fuera de rango.
Se ejecuta cada dos minutos y su regla crontab es

```
*/2  *    *   *   *    sh /home/ubuntu/bin/masats_limits_query.sh
```

### Docker Eurecat

El proceso de detección de outliers mediante IA de Eurecat se ejecuta en dos máquinas:

1. un registro al que subir la imagen del contenedor Docker que contiene el código empaquetado con todas sus dependencias listo para ejecutarse
2. y otra máquina más potente que toma la imagen y ejecuta el código para después mostrar las alertas (máquina ***[Docker Eurecat](aws.md)***)
