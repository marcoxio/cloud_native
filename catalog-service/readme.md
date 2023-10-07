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

# Catalog Service

This application is part of the Polar Bookshop system and provides the functionality for managing
the books in the bookshop catalog. It's part of the project built in the
[Cloud Native Spring in Action](https://www.manning.com/books/cloud-native-spring-in-action) book
by [Thomas Vitale](https://www.thomasvitale.com).

## REST API

| Endpoint	      | Method   | Req. body  | Status | Resp. body     | Description    		   	     |
|:---------------:|:--------:|:----------:|:------:|:--------------:|:-------------------------------|
| `/books`        | `GET`    |            | 200    | Book[]         | Get all the books in the catalog. |
| `/books`        | `POST`   | Book       | 201    | Book           | Add a new book to the catalog. |
|                 |          |            | 422    |                | A book with the same ISBN already exists. |
| `/books/{isbn}` | `GET`    |            | 200    | Book           | Get the book with the given ISBN. |
|                 |          |            | 404    |                | No book with the given ISBN exists. |
| `/books/{isbn}` | `PUT`    | Book       | 200    | Book           | Update the book with the given ISBN. |
|                 |          |            | 201    | Book           | Create a book with the given ISBN. |
| `/books/{isbn}` | `DELETE` |            | 204    |                | Delete the book with the given ISBN. |

## Useful Commands

| Gradle Command	         | Description                                   |
|:---------------------------|:----------------------------------------------|
| `./gradlew bootRun`        | Run the application.                          |
| `./gradlew build`          | Build the application.                        |
| `./gradlew test`           | Run tests.                                    |
| `./gradlew bootJar`        | Package the application as a JAR.             |
| `./gradlew bootBuildImage` | Package the application as a container image. |

After building the application, you can also run it from the Java CLI:

```bash
java -jar build/libs/catalog-service-0.0.1-SNAPSHOT.jar
```

## Running a PostgreSQL Database

Run PostgreSQL as a Docker container

```bash
docker run -d \
    --name polar-postgres \
    -e POSTGRES_USER=user \
    -e POSTGRES_PASSWORD=password \
    -e POSTGRES_DB=polardb_catalog \
    -p 5432:5432 \
    postgres:14.4
```

### Container Commands

| Docker Command	                     | Description       |
|:------------------------------------|:-----------------:|
| `docker stop polar-postgres`        | Stop container.   |
| `docker start polar-postgres`       | Start container.  |
| `docker remove polar-postgres`      | Remove container. |

### Database Commands

Start an interactive PSQL console:

```bash
docker exec -it polar-postgres psql -U user -d polardb_catalog
```

| PSQL Command	              | Description                                    |
|:---------------------------|:-----------------------------------------------|
| `\list`                    | List all databases.                            |
| `\connect polardb_catalog` | Connect to specific database.                  |
| `\dt`                      | List all tables.                               |
| `\d book`                  | Show the `book` table schema.                  |
| `\d flyway_schema_history` | Show the `flyway_schema_history` table schema. |
| `\quit`                    | Quit interactive psql console.                 |

From within the PSQL console, you can also fetch all the data stored in the `book` table.

```bash
select * from book;
```

The following query is to fetch all the data stored in the `flyway_schema_history` table.

```bash
select * from flyway_schema_history;
```

./gradlew test --tests BookRepositoryJdbcTests
./gradlew test --tests CatalogServiceApplicationTests

# delete image
docker rm -fv polar-postgres

# execute build image Dockerfile
docker build -t my-java-image:1.0.0 .

# view images docker
docker images my-java-image

# run container image
docker run --rm my-java-image:1.0.0

# build the JAR artefact #1
./gradlew clean bootJar

# build image Dockerfile #2
docker build -t catalog-service .

# build image using maven
docker build --build-arg JAR_FILE=target/*.jar -t catalog-service .

# delete images first #3 
docker rm -f catalog-service polar-postgres

# delete network 
docker network rm catalog-network

# create network #4
docker network create catalog-network

# execute polar-postgres #5
docker run -d --name polar-postgres --net catalog-network -e POSTGRES_USER=user -e POSTGRES_PASSWORD=password -e POSTGRES_DB=polardb_catalog -p 5432:5432 postgres:14.4

# execute catalog service #6
docker run -d --name catalog-service --net catalog-network -p 9001:9001 -e SPRING_DATASOURCE_URL=jdbc:postgresql://polar-postgres:5432/polardb_catalog -e SPRING_PROFILES_ACTIVE=testdata catalog-service

# request service by Docker
http :9001/books

# request service by Docker Toolbox
http 192.168.99.100:9001/books

# Create Cluster Kubernetes by profile polar
minikube start --cpus 2 --memory 4g --driver docker --profile polar

# Obtain list all nodes cluster
kubectl get nodes

# list all the available contexts
kubectl config get-contexts

# current contex
kubectl config current-context

# change the current context
kubectl config use-context polar

# run the following command to deploy PostgreSQL in your local cluste
kubectl apply -f services

# review logs
kubectl logs deployment/polar-postgres

# review pod
kubectl get pod

# delete services
kubectl delete -f services

# stop profile
minikube stop --profile polar

# start profile
minikube start --profile polar

# apply manifest service
kubectl apply -f k8s/service.yml

# apply manifest service
kubectl apply -f k8s/deployment.yml

# get service
kubectl get svc -l app=catalog-service

# listener de port for request
kubectl port-forward service/catalog-service 9001:80

# build image docker and load image with minukube profile polar
gradlew bootBuildImage
minikube image load catalog -service --profile polar

# apply manifest specific
kubectl apply -f k8s/deployment.yml
kubectl get pods -l app=catalog-service

# Delete manifest kubernetes
kubectl delete -f k8s
kubectl delete -f services

# validar vulnerabilidad imagen
docker run -it --rm anchore/grype .

# build image with token
./gradlew bootBuildImage \
--imageName ghcr.io/<your_github_username>/catalog-service \
--publishImage \
-PregistryUrl=ghcr.io \
-PregistryUsername=<your_github_username> \
-PregistryToken=<your_github_token>