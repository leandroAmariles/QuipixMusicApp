```markdown
# Music App

This project is a music application built with Java, Spring Boot, and Maven. It consists of two main modules: `musicApp` and `musicAppPersist`.

## Prerequisites

- Java 17
- Maven 3.8.4 or higher
- Docker (for containerization)
- Kafka
- MySQL
- Keycloak (for OAuth2 authentication)

## Configuration


### Ketcloack

Create a new realm in Keycloak and add a new client with the following settings:

Go to http://localhost:8088/auth/admin/master/console/#/realms and click on `Add realm`. Enter the realm name and click on `Create`.

<img alt="img.png" src="C:\Users\amari\Desktop\Java\OpenBootcamp\TechicalTesQuipux\images\img.png"/>

Click on `Clients` and then `Create`.

<img alt="img1.png" src="\TechicalTesQuipux\images\img_1.png"/>

Config acces setings

<img alt="img2.png" src="\TechicalTesQuipux\images\img_2.png"/>

In credentials looking for the client secret

<img alt="img3.png" src="\TechicalTesQuipux\images\img_3.png"/>

Cretae a new user

<img alt="img4.png" src="\TechicalTesQuipux\images\img_4.png"/>

In credentials set a password

<img alt="img5.png" src="\TechicalTesQuipux\images\img_5.png"/>

Generate a token with postman

<img alt="img6.png" src="\TechicalTesQuipux\images\img_6.png"/>



Modify de docker-compose.yml file with the client secret

KEYCLOAK_CLIENT_SECRET:

```yml


### Application Properties

#### `musicAppPersist/src/main/resources/application.properties`

```ini
spring.application.name=musicAppPersist

################################
### Kafka Configuration
################################
spring.kafka.bootstrap-servers=${KAFKA_SERVER}
kafka.bootstrap-servers=${KAFKA_SERVER}
spring.kafka.consumer.group-id=musicApp

kafka.topic.saveReproductionList=saveReproductionList
kafka.topic.deleteReproductionList=deleteReproductionList
kafka.topic.listReproductionList=listReproductionList
kafka.topic.responseReproductionList=responseReproductionList

################################
### Database Configuration
################################
spring.datasource.url=${DB_URL}
spring.datasource.driverClassName=com.mysql.cj.jdbc.Driver
spring.datasource.username=${DB_USER}
spring.datasource.password=${DB_PASSWORD}
spring.data.jdbc.dialect=mysql
spring.jpa.generate-ddl=true
spring.jpa.hibernate.ddl-auto=update
```

#### `musicApp/src/main/resources/application.properties`

```ini
spring.application.name=musicApp
server.port=8083

################################
### Database Configuration
################################
spring.datasource.url=${DB_URL}
spring.datasource.driverClassName=com.mysql.cj.jdbc.Driver
spring.datasource.username=${DB_USER}
spring.datasource.password=${DB_PASSWORD}
spring.data.jdbc.dialect=mysql
spring.jpa.generate-ddl=true
spring.jpa.hibernate.ddl-auto=update

################################
### Kafka Configuration
################################
spring.kafka.bootstrap-servers=${KAFKA_SERVER}
spring.kafka.consumer.auto-offset-reset=earliest
spring.kafka.consumer.key-deserializer=org.apache.kafka.common.serialization.StringDeserializer
spring.kafka.consumer.value-deserializer=org.apache.kafka.common.serialization.StringDeserializer
kafka.bootstrap-servers=${KAFKA_SERVER}
spring.kafka.consumer.group-id=musicApp

kafka.topic.saveReproductionList=saveReproductionList
kafka.topic.deleteReproductionList=deleteReproductionList
kafka.topic.listReproductionList=listReproductionList
kafka.topic.responseReproductionList=responseReproductionList

##############################
### Spotify Config
##############################
spotify.client.id=ff52cf89c60345a39060fa14deb866cf
spotify.client.secret=4ea716bd8f144701a90972d89f3fb4c0
spotify.authorization.url=https://accounts.spotify.com/api/token
spotify.api.url=https://api.spotify.com/v1/browse/categories

################################
### Web Client Config
################################
webclient.connection.max.connections=100
webclient.connection.acquire.timeout=45
webclient.connection.tls.version=TLSv1.2

################################
### Keycloak Config
################################
spring.security.oauth2.resourceserver.jwt.issuer-uri=http://localhost:8088/realms/Quipux
spring.security.oauth2.resourceserver.opaque-token.client-id=QuipuxClient
spring.security.oauth2.resourceserver.opaque-token.client-secret=${KEYCLOAK_CLIENT_SECRET}
spring.security.oauth2.authorizationserver.endpoint.authorization-uri=http://localhost:8088/auth
spring.security.oauth2.authorizationserver.endpoint.oidc.user-info-uri=http://localhost:8088/auth/realms/Quipux/protocol/openid-connect/userinfo
spring.security.oauth2.authorizationserver.endpoint.token-uri=http://localhost:8088/realms/Quipux/protocol/openid-connect/token
```

## Building and Running

### Using Maven

To build the project, run:

```sh
mvn clean package -DskipTests
```

### Using Docker

To build and run the Docker container for `musicAppPersist`, use the following commands:

```sh
docker-compose up --build
```

## License

This project is licensed under the MIT License.
```