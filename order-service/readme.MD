# initialize service docker-compose 
docker-compose up -d polar-postgres

# run application
./gradlew bootRun

# run service
http POST :9002/orders isbn=1234567890 quantity=3

# run test
./gradlew test --tests BookClientTests

# run r2dbc
./gradlew test --tests OrderRepositoryR2dbcTests

# run webFlux
./gradlew test --tests OrderControllerWebFluxTests

# /kubernetes/platform/development
kubectl apply -f services

