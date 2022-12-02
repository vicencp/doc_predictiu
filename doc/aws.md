# AWS

Cuenta AWS: 896950693547 (datik-masats)

## EC2

|      VM       |   Region     | Public IP     | Certificate to access | Purpose                  |
|---------------|--------------|---------------|-----------------------|--------------------------|
| nagios         | eu-central-1 | 3.64.165.3    | nagios-masats         | Nagios                  |
| CopyCat        | eu-central-1 | 52.29.88.23 * | CloudMasats           | Procesos de integración |
| Docker Eurecat | eu-central-1 | 3.65.139.34 * | CloudMasats           | Procesos de Eurecat     |

* IP elástica fija

## S3

predictivo-masats

![](./doc/img/s3.png)

## OpenSearch

|       Domain              | Nodos |  Tipo      | Almacenamiento | Version            |
|---------------------------|-------|------------|----------------|--------------------|
| predictivo-masats          |    1  | r5.xlarge  |     200GB      | Elasticsearch 6.8 |

## Kubernetes

Hay un componente desplegado en el Kubernetes.

***POD name***: masats-eventos
***namespace***: ms-adhoc-reports

***Registry***: git.datik.io:5005/ingenieria/masats/etl-scripts:prod-1.0.0

```console
Este componente está actualmente generando el siguiente error:

Adjuntauta daude logak baino hau da: ModuleNotFoundError: No module named 'boto3'
```


## IAM

***Users***

|           User    |      Grupos       | Politicas              |   Key                |
|-------------------|-------------------|------------------------|----------------------|
| datik-integration | DatikS3FullAccess | AmazonS3FullAccess     | AKIA5BVTNN2VY6Y4XS4U |
| masats-s3-access  |         -         | AmazonS3ReadOnlyAccess | AKIA5BVTNN2VVCIBAQ76 |
| vicenc.pio        |         -         | AdministratorAccess    |           -          |

***Rol***

|       Rol                     |  Cuenta de confianza |
|-------------------------------|----------------------|
| DatikAccessAdministratorRole   |    034152894809     |
| DatikAccessIoTRole             |    034152894809		 |
| ServiceElasticSearchFullAccess |    159146222523     |
| ServiceS3FullAccess            |    159146222523     |
