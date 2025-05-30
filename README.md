# Simple filesender

This filesender service consists in two microservices (camel routes). One service gets notifications from minio and downloads the file to a PVC. The second service polls the PVC for files and transfers them to destination (SFTP).

## Prerequisites
* Kafka cluster
* Minio service with one Bucket with `put` events forwarded to Kafka topic
* Kubernetes cluster with Camel-K Operator


## How-to run

### Secret Configuration

Create a secret for every microservice to contain the sensitive properties in the `application.properties` file and `sender-application.properties` file:

```
kubectl create secret generic fileservice-props --from-file application.properties

kubectl create secret generic files-sender-props --from-file sender-application.properties
```

Then create the PVC, and run the microservices.

```
kubectl apply -f fileservice-pvc.yaml

kamel run --dev --config secret:fileservice-props --volume fileservice-pvc:/files file-downloader.camel.yaml

kamel run --dev --config secret:files-sender-props --volume fileservice-pvc:/files file-sender.camel.yaml
```

## Export

First start both integrations. Then open a second terminal and run the following command for every integration changing the integration-names acordingly:

```sh
kamel promote <integration-name> --to prod -o yaml > integration-name.yaml
```

Then apply integration-name.yaml to your cluster.

## File downloader route

![filesender route](./route.png)
