# initialize service docker-compose 
docker-compose up -d polar-postgres

# run application
./gradlew bootRun

# test service
http POST :9002/orders isbn=1234567890 quantity=3