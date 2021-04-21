# Managing Gateways
REST service (JSON/HTTP) for storing information about gateways and their associated peripheral devices.
- Programming language: Java
- Framework: Spring Boot
- Database: In-memory
- Automated build: Apache Maven

## Steps to Setup

- To build open a console and execute ```mvn clean compile install -DskipTests```
- To run open a console and execute ```mvn spring-boot:run```. The app will start running at http://localhost:8080

## Authentication
Gateway services are protected from unauthoried access. First login using username and password to get authorization token and pass this token in subsequent api calls.

### Login
```POST http://localhost:8080/login
body: {
    "username": "apiuser",
    "password": "apipass",
}
Response - On successful login returns 200(OK) and authorization token in headers.```

### Signup
There is also a signup api available to add new user.
POST http://localhost:8080/signup
body: {
    "username": "newuser",
    "password": "newpass",
}
Response - On successful returns 200(OK).

## Gateway Service Endpoints
To access gateway services add authorization token into headers. ```Authorization: received token after login```

### Get all stored gateways
GET http://localhost:8080/gateways
Parameters: page - page number to start
            size - number of records to retrieve
Response - 200 (OK) and the list of gateways in body

## Create a gateway:
POST http://localhost:8080/gateways<br>
body: {<br>
"serial": "string", //a unique serial number ex: AbC123<br>
"name": "string", //a human-readable name ex: Gateway A<br>
"ip": "string" //an IPv4 address ex: 10.0.0.1<br>
}

## Delete a gateway:
DELETE http://localhost:8080/gateways/{serial} // ex: http://localhost:8080/gateways/AbC123

## Get all stored gateways:
GET http://localhost:8080/gateways

## Get a single gateway:
GET http://localhost:8080/gateways/{serial} // ex: http://localhost:8080/gateways/AbC123

## Add a device from a gateway
POST http://localhost:8080/gateways/{serial}/device<br>
body: {<br>
"vendor": "string", // ex: Vendor A<br>
"status": "online|offline" // ex: online<br>
}

## Remove a device from a gateway
DELETE http://localhost:8080/gateways/{serial}/device/{device_uid} // ex: http://localhost:8080/gateways/AbC123/device/1
