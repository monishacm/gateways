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
There is also a signup api available to add new user if required.
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

### Add a new device to the "serial" gateway
**POST http://localhost:8080/gateways/{serial}/devices**<br />
```
{
    "uid": "Long", // A unique id of the device
    "vendor": "string", // Vendor name, ex: Vendor A
    "status": "online|offline"
}
```
**Response** - 200 (OK) and body with the updated gateway, or status 404 (Not Found) if gateway not exists or 400 (Bad Request) if 10 devices already added to gateway

### Delete the "serial" gateway.
**DELETE http://localhost:8080/gateways/{serial}**<br />
**Response** - 200 (OK) or status 404 (Not Found) if gateway not exists

### Delete the "uid" device from the "serial" gateway
**DELETE http://localhost:8080/gateways/{serial}/devices/{uid}**<br />
**Response** - 200 (OK) and body with updated gateway details or status 404 (Not Found) if gateway or device not exists
