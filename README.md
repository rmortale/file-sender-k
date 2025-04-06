# Nextgen filesender

This route gets notifications from minio, downloads the file and sends it to destination.

## How-to run

### Local dev with kamel

Run with

```sh
kamel run --dev  --property file:application.properties -t camel.runtime-provider=plain-quarkus file-sender.camel.yaml
```

Export with

```sh
kamel run --property file:application.properties -t camel.runtime-provider=plain-quarkus file-sender.camel.yaml -o yaml > integration.yaml
```

## Route

![filesender route](./route.png)
