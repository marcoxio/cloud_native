# build image first step
./gradlew bootBuildImage

# run services /polar-deployment/docker second step
docker-compose up -d catalog-service order-service polar-redis

# polar-deployment/kubernetes/platform/development
## Run DB - first step
kubectl apply -f services

# run app locally - third step
## order-service | catalog-service | edge-service 
./gradlew bootRun


# Apache Benchmark example
ab -n 21 -c 1 -m POST http://localhost:9000/orders

# Apache Benchmark example
ab -n 21 -c 1 -m GET http://localhost:9000/books

# test service
http :9000/books

# run test EdgeService
./gradlew test --tests EdgeServiceApplicationTests

# start minikube profile polar with specific resources
minikube start --cpus 2 --memory 4g --driver docker --profile polar

# addons enable ingress
minikube addons enable ingress --profile polar

# componets deployed with Ingress
kubectl get all -n ingress-nginx

# deploy Postgres and Redis localy
kubectl apply -f services

# Run follow this command
gradlew bootBuildImage
minikube image load edge-service --profile polar

# obtain DNS ingress
minikube ip --profile polar

# run the command with the edge-service
kubectl apply -f k8s

# verify ingress
kubectl get ingress

# asked to input your machine’s password to authorize the tunneling to the cluster
minikube tunnel --profile polar

# test service
http 127.0.0.1/books

# stop and delete minikube
$ minikube stop --profile polar
$ minikube delete --profile polar