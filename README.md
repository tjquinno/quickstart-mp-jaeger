# Helidon 1.4.8 Quickstart MP Example with Jaeger Tracing

This example implements a simple Hello World REST service using MicroProfile, using Jaeger tracing.
Note that this variation imposes a specific release of Apache Thrift to work around an API 
change from 0.12 to 0.13.

The `application.yaml` file specifies Jaeger tracing and causes Helidon to log each transmission 
of a span to Jaeger.

## Build and run

With JDK8+
```bash
mvn package
java -jar target/quickstart-mp-jaeger.jar
```

## Exercise the application

```
curl -X GET http://localhost:8080/greet
{"message":"Hello World!"}

curl -X GET http://localhost:8080/greet/Joe
{"message":"Hello Joe!"}

curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://localhost:8080/greet/greeting

curl -X GET http://localhost:8080/greet/Jose
{"message":"Hola Jose!"}
```

## Try health and metrics

```
curl -s -X GET http://localhost:8080/health
{"outcome":"UP",...
. . .

# Prometheus Format
curl -s -X GET http://localhost:8080/metrics
# TYPE base:gc_g1_young_generation_count gauge
. . .

# JSON Format
curl -H 'Accept: application/json' -X GET http://localhost:8080/metrics
{"base":...
. . .

```

## Build the Docker Image

```
docker build -t quickstart-mp-jaeger .
```

## Start the application with Docker

```
docker run --rm -p 8080:8080 quickstart-mp-jaeger:latest
```

Exercise the application as described above

## Deploy the application to Kubernetes

```
kubectl cluster-info                         # Verify which cluster
kubectl get pods                             # Verify connectivity to cluster
kubectl create -f app.yaml               # Deploy application
kubectl get service quickstart-mp-jaeger  # Verify deployed service
```
