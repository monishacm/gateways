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
**POST http://localhost:8080/login**
```
{
    "username": "apiuser",
    "password": "apipass",
}
```
**Response** - On successful login returns 200(OK) and authorization token in headers.

### Signup
There is also a signup api available to add new user.
**POST http://localhost:8080/signup**
```
{
    "username": "newuser",
    "password": "newpass",
}
```
**Response** - On successful returns 200(OK).

## Gateway Service Endpoints
To access gateway services add authorization token into headers. **```Authorization: "received token after login"```**

### Get all stored gateways
**GET http://localhost:8080/gateways**<br />
**Parameters:**
```
page: page number to start
size: number of records to retrieve
```
**Response** - 200 (OK) and both with the list of gateways

### Add a new gateway
**POST http://localhost:8080/gateways**
```
{
    "serial": "string", //a unique serial number ex: abcd1234
    "name": "string", //a human-readable name ex: Gateway 1
    "ip": "string" //an IPv4 address ex: 10.0.0.1
}
```
**Response** - 200 (OK) and body with the new gateway, or status 400 (Bad Request) if a gateway exists with same serial or IP Address invalid

### get the details of a gateway by serial
**GET http://localhost:8080/gateways/{serial}**<br />
**Parameters:**
```
serial: serial of the gateway to retrieve
```
**Response** - 200 (OK) and with body the gateway, or status 404 (Not Found) if gateway not exists

### Add a device from a gateway
POST http://localhost:8080/gateways/{serial}/device<br>
body: {<br>
"vendor": "string", // ex: Vendor A<br>
"status": "online|offline" // ex: online<br>
}

## Delete a gateway:
DELETE http://localhost:8080/gateways/{serial} // ex: http://localhost:8080/gateways/AbC123

## Get a single gateway:
GET http://localhost:8080/gateways/{serial} // ex: http://localhost:8080/gateways/AbC123

## Remove a device from a gateway
DELETE http://localhost:8080/gateways/{serial}/device/{device_uid} // ex: http://localhost:8080/gateways/AbC123/device/1
