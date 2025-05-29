# Simple filesender

This filesender service consists in two microservices (camel routes). One service gets notifications from minio and downloads the file to a PVC. The second service polls the PVC for files and transfers them to destination (SFTP).

## Prerequisites
* Kafka cluster
* Minio service with one Bucket with `put` events forwarded to Kafka topic
* Kubernetes cluster with Camel-K Operator


## How-to run

### Secret Configuration

For convenience, create a secret to contain the sensitive properties in the `application.properties` file:

```
kubectl create secret generic fileservice-props --from-file application.properties
```

Then create the PVC, and run the microservices.

```
kubectl apply -f fileservice-pvc.yaml

kamel run --dev --config secret:fileservice-props --volume fileservice-pvc:/files file-downloader.camel.yaml

kamel run --dev --config secret:fileservice-props --volume fileservice-pvc:/files pvc-consumer.yaml
```



### Local dev with kamel

Replace properties in `application.properties` with your values.

Run with

```sh
kamel run --dev  --property file:application.properties -t camel.runtime-provider=plain-quarkus file-sender.camel.yaml
```

## Export
First create ConfigMap and Secrets from the application.properties:

```
kubectl create secret generic file-sender-secret --from-env-file local-secrets.properties

kubectl create cm file-sender-cm --from-file local.properties
```

then run the integration with:

```sh
kamel run --config secret:file-sender-secret --config configmap:file-sender-cm -t camel.runtime-provider=plain-quarkus file-sender.camel.yaml
```

Now open a second terminal and run the following command:

```sh
kamel promote file-sender --to prod -o yaml > integration.yaml
```

Then apply integration.yaml to your cluster.

## Route

![filesender route](./route.png)
