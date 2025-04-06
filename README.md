# Nextgen filesender

This route gets notifications from minio, downloads the file and sends it to destination.

## How-to run

### Local dev with kamel

Replace properties in `application.properties` with your values.

Run with

```sh
kamel run --dev  --property file:application.properties -t camel.runtime-provider=plain-quarkus file-sender.camel.yaml
```

## Export
Create ConfigMap and Secrets first from the application.properties, then export with:

```sh
kamel run --config secret:mysecret --config configmap:myconfigmap -t camel.runtime-provider=plain-quarkus file-sender.camel.yaml -o yaml > integration.yaml
```

## Route

![filesender route](./route.png)
