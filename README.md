# Appointment Management REST API

A Spring Boot REST API application for managing appointments with MySQL database integration.

## Overview

This application provides a RESTful API to perform CRUD operations on appointments. It's built using Spring Boot 3.5.5 with JDBC for database connectivity and runs on AWS RDS MySQL database.

## Technology Stack

- **Java**: 21
- **Spring Boot**: 3.5.5
- **Database**: MySQL (AWS RDS)
- **Build Tool**: Maven
- **Dependencies**:
  - Spring Web
  - Spring JDBC
  - MySQL Connector/J 9.4.0
  - Spring Boot DevTools

## Project Structure

```
src/
├── main/
│   ├── java/mssu/in/restapi_app/
│   │   ├── controller/
│   │   │   └── AppointmentController.java
│   │   ├── entity/
│   │   │   └── Appointment.java
│   │   ├── repository/
│   │   │   └── AppointmentRepository.java
│   │   ├── service/
│   │   │   └── AppointmentService.java
│   │   └── RestapiAppApplication.java
│   └── resources/
│       └── application.properties
└── test/
```

## Database Schema

The application expects an `appointments` table with the following structure:

```sql
CREATE TABLE appointments (
    appointment_id INT PRIMARY KEY AUTO_INCREMENT,
    client_id INT NOT NULL,
    provider_id INT NOT NULL,
    service_id INT NOT NULL,
    appointment_date VARCHAR(255),
    appointment_time VARCHAR(255),
    status VARCHAR(255),
    created_at VARCHAR(255)
);
```

## Configuration

Update `src/main/resources/application.properties` with your database credentials:

```properties
server.port=9080
spring.datasource.url=jdbc:mysql://[your-database-host]:3306/[database-name]?useSSL=false&serverTimezone=UTC
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.username=[your-username]
spring.datasource.password=[your-password]
```

## API Endpoints

Base URL: `http://localhost:9080/appointments`

### Get All Appointments
```
GET /appointments/get
```
Returns a list of all appointments.

### Get Appointment by ID
```
GET /appointments/get/{id}
```
Returns a specific appointment by ID.

### Create New Appointment
```
POST /appointments/add
Content-Type: application/json

{
    "clientId": 1,
    "providerId": 2,
    "serviceId": 3,
    "appointmentDate": "2025-10-15",
    "appointmentTime": "10:00 AM",
    "status": "scheduled",
    "createdAt": "2025-10-11"
}
```

### Update Appointment
```
PUT /appointments/edit
Content-Type: application/json

{
    "appointmentId": 1,
    "clientId": 1,
    "providerId": 2,
    "serviceId": 3,
    "appointmentDate": "2025-10-16",
    "appointmentTime": "11:00 AM",
    "status": "confirmed",
    "createdAt": "2025-10-11"
}
```

### Delete Appointment
```
DELETE /appointments/delete/{id}
```
Deletes an appointment by ID.

## Running the Application

### Prerequisites
- Java 21 or higher
- Maven 3.6+
- MySQL database

### Build and Run

**Using Maven Wrapper (Recommended):**
```bash
# Windows
mvnw.cmd spring-boot:run

# Linux/Mac
./mvnw spring-boot:run
```

**Using Maven:**
```bash
mvn spring-boot:run
```

**Build JAR:**
```bash
mvn clean package
java -jar target/restapi_app-0.0.1-SNAPSHOT.jar
```

The application will start on `http://localhost:9080`

## Testing the API

Using curl:

```bash
# Get all appointments
curl http://localhost:9080/appointments/get

# Get appointment by ID
curl http://localhost:9080/appointments/get/1

# Create new appointment
curl -X POST http://localhost:9080/appointments/add \
  -H "Content-Type: application/json" \
  -d '{"clientId":1,"providerId":2,"serviceId":3,"appointmentDate":"2025-10-15","appointmentTime":"10:00 AM","status":"scheduled","createdAt":"2025-10-11"}'

# Update appointment
curl -X PUT http://localhost:9080/appointments/edit \
  -H "Content-Type: application/json" \
  -d '{"appointmentId":1,"clientId":1,"providerId":2,"serviceId":3,"appointmentDate":"2025-10-16","appointmentTime":"11:00 AM","status":"confirmed","createdAt":"2025-10-11"}'

# Delete appointment
curl -X DELETE http://localhost:9080/appointments/delete/1
```

## Development

The application uses Spring Boot DevTools for automatic restart during development. Any changes to the source code will trigger an automatic restart.

## License

This project is a demo application for Spring Boot.

## Author

Ishita Parkar, Kirshnandu Gurey, Prutha Jadhav and Yash Adhau 
