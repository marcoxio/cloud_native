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