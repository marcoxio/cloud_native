# run test gradle
./gradlew test

# run application gradle
./gradlew bootRun

# build image with intellijei
./gradlew bootBuildImage

# review images docker
docker images catalog-service:0.0.1-SNAPSHOT

# execute image docker
docker run --rm --name catalog-service -p 9001:9001 catalog-service:0.0.1-SNAPSHOT

# start minikube
minikube start

# load image .jar
minikube image load catalog-service:0.0.1-SNAPSHOT

# create a Deployment from a container image
kubectl create deployment catalog-service --image=catalog-service:0.0.1-SNAPSHOT

# Obtain deployment
kubectl get deployment

# Obtain pod
kubectl get pod

# expose cluster network port 9001
kubectl expose deployment catalog-service --name=catalog-service --port=9001

# Expose services
kubectl get service catalog-service

#  forward the traffic from a local port on your computer (for example, 8000) to the port exposed by the Service inside the cluster (9001).
kubectl port-forward service/catalog-service 8000:9001

# build .jar
./gradlew bootJar

# execute .jar
java -jar build/libs/catalog-service-0.0.1-SNAPSHOT.jar

# request 201
curl -X POST -H "Content-Type: application/json" -d "{\"author\":\"Lyra Silverstar\",\"title\":\"Northern Lights\",\"isbn\":\"1234567891\",\"price\":9.90}" http://localhost:9001/books

# get request
curl -X GET http://localhost:9001/books/1234567891 | jq

# request 422
curl -X POST -H "Content-Type: application/json" -d "{\"author\":\"Jon Snow\",\"title\":\"\",\"isbn\":\"123ABC456Z\",\"price\":9.90}" http://localhost:9001/books

# run the tests with the following command
./gradlew test --tests BookValidationTests
./gradlew test --tests CatalogServiceApplicationTests
./gradlew test --tests BookControllerMvcTests
./gradlew test --tests BookJsonTests

githubActions
grype .
