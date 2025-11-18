# Appointment Management System

A Simple, Scalable appointment management application with Spring Boot backend and modern JavaScript frontend.

**Cloud Computing Project Submission**

## Team Members

- Ishita Parkar
- Kirshnandu Gurey
- Prutha Jadhav
- Yash Adhau
- Shreeraj Salvi

## Features

- 📅 Complete CRUD operations for appointments
- 🎨 Modern, responsive web interface
- 🔄 Real-time appointment updates
- 🔍 Search and filter functionality
- 🌐 RESTful API architecture
- 💾 Dual database support (H2 for development, MySQL for production)
- ☁️ AWS RDS integration ready
- 🚀 Spring Boot DevTools for rapid development

## Tech Stack

### Backend
- **Spring Boot**: 3.5.5
- **Java**: 21
- **Database**: H2 (development) / MySQL (production)
- **JDBC Template**: Direct SQL operations
- **Maven**: Build and dependency management

### Frontend
- **HTML5, CSS3, JavaScript (ES6+)**
- **Vanilla JS**: No framework dependencies
- **Responsive Design**: Mobile-friendly interface

### Cloud & DevOps
- **AWS RDS**: MySQL database hosting
- **H2 Console**: In-memory database for local testing
- **Spring Boot DevTools**: Hot reload during development

## Quick Start

### Prerequisites
- Java 21 or higher
- Maven 3.6+
- MySQL database (for production) or H2 (included for development)

### Setup

**Clone the repository**
```bash
git clone <repository-url>
cd restapi_app
```

**Run the application**

Option A: Using Maven Wrapper (Recommended)
```bash
# Windows
mvnw.cmd spring-boot:run

# Mac/Linux
./mvnw spring-boot:run
```

Option B: Using Maven
```bash
mvn spring-boot:run
```

**Access the application**
- Frontend: http://localhost:9080
- H2 Console: http://localhost:9080/h2-console
- API Base: http://localhost:9080/appointments

## Configuration

The application uses `src/main/resources/application.properties` for configuration:

### Development (H2 Database - Default)
```properties
server.port=9080
spring.datasource.url=jdbc:h2:mem:appointmentdb
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
spring.h2.console.enabled=true
```

### Production (AWS RDS MySQL)
```properties
server.port=9080
spring.datasource.url=jdbc:mysql://[your-rds-endpoint]:3306/appointmentdb?useSSL=false&serverTimezone=UTC
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.username=[your-username]
spring.datasource.password=[your-password]
```

## Project Structure

```
restapi_app/
├── src/
│   ├── main/
│   │   ├── java/mssu/in/restapi_app/
│   │   │   ├── config/
│   │   │   │   └── WebConfig.java           # CORS configuration
│   │   │   ├── controller/
│   │   │   │   └── AppointmentController.java  # REST API endpoints
│   │   │   ├── service/
│   │   │   │   └── AppointmentService.java     # Business logic layer
│   │   │   ├── repository/
│   │   │   │   └── AppointmentRepository.java  # Data access layer
│   │   │   ├── entity/
│   │   │   │   └── Appointment.java            # Domain model
│   │   │   └── RestapiAppApplication.java      # Main application
│   │   └── resources/
│   │       ├── static/
│   │       │   ├── index.html              # Frontend UI
│   │       │   ├── app.js                  # Frontend logic
│   │       │   └── styles.css              # Styling
│   │       ├── application.properties      # Configuration
│   │       ├── schema.sql                  # Database schema
│   │       └── data.sql                    # Sample data
│   └── test/
│       └── java/mssu/in/restapi_app/
│           └── RestapiAppApplicationTests.java
├── pom.xml                                 # Maven dependencies
└── README.md
```

## Architecture

This application follows a **3-tier layered architecture**:

### 1. Presentation Layer (Controller)
- `AppointmentController.java` - Handles HTTP requests and responses
- Exposes RESTful endpoints at `/appointments`
- Returns JSON data for frontend consumption

### 2. Business Logic Layer (Service)
- `AppointmentService.java` - Contains business logic
- Acts as intermediary between controller and repository
- Provides transaction management and validation

### 3. Data Access Layer (Repository)
- `AppointmentRepository.java` - Direct database operations
- Uses Spring's `JdbcTemplate` for SQL execution
- Custom RowMapper for object-relational mapping

### 4. Domain Model (Entity)
- `Appointment.java` - POJO representing appointment data
- Contains getters, setters, and constructors

### 5. Configuration
- `WebConfig.java` - Enables CORS for cross-origin requests
- Allows frontend to communicate with backend API

## How It Works

### Request Flow

```
Frontend (Browser)
    ↓ HTTP Request (JSON)
AppointmentController
    ↓ Method Call
AppointmentService
    ↓ Method Call
AppointmentRepository
    ↓ SQL Query (JdbcTemplate)
Database (H2/MySQL)
    ↓ Result Set
AppointmentRepository (RowMapper)
    ↓ Appointment Object
AppointmentService
    ↓ Appointment Object
AppointmentController
    ↓ HTTP Response (JSON)
Frontend (Browser)
```

### Example: Creating an Appointment

1. User fills form in `index.html` and clicks "Add Appointment"
2. `app.js` sends POST request to `/appointments/add` with JSON data
3. `AppointmentController.addNewAppointment()` receives the request
4. Controller calls `AppointmentService.addNewAppointment()`
5. Service delegates to `AppointmentRepository.addNewAppointment()`
6. Repository executes SQL INSERT using `JdbcTemplate`
7. Database stores the record and auto-generates ID
8. Success response flows back through the layers
9. Frontend refreshes the appointment list

## API Endpoints

Base URL: `http://localhost:9080/appointments`

| Method | Endpoint | Description | Request Body |
|--------|----------|-------------|--------------|
| GET | `/get` | Get all appointments | None |
| GET | `/get/{id}` | Get appointment by ID | None |
| POST | `/add` | Create new appointment | Appointment JSON |
| PUT | `/edit` | Update appointment | Appointment JSON |
| DELETE | `/delete/{id}` | Delete appointment | None |

### Request/Response Examples

**Get All Appointments**
```bash
curl http://localhost:9080/appointments/get
```

**Get Appointment by ID**
```bash
curl http://localhost:9080/appointments/get/1
```

**Create New Appointment**
```bash
curl -X POST http://localhost:9080/appointments/add \
  -H "Content-Type: application/json" \
  -d '{
    "clientId": 101,
    "providerId": 201,
    "serviceId": 301,
    "appointmentDate": "2025-11-20",
    "appointmentTime": "10:00",
    "status": "Scheduled",
    "createdAt": "2025-11-18"
  }'
```

**Update Appointment**
```bash
curl -X PUT http://localhost:9080/appointments/edit \
  -H "Content-Type: application/json" \
  -d '{
    "appointmentId": 1,
    "clientId": 101,
    "providerId": 201,
    "serviceId": 301,
    "appointmentDate": "2025-11-21",
    "appointmentTime": "14:30",
    "status": "Confirmed",
    "createdAt": "2025-11-18"
  }'
```

**Delete Appointment**
```bash
curl -X DELETE http://localhost:9080/appointments/delete/1
```

## Database Schema

```sql
CREATE TABLE IF NOT EXISTS appointments (
    appointment_id INT AUTO_INCREMENT PRIMARY KEY,
    client_id INT NOT NULL,
    provider_id INT NOT NULL,
    service_id INT NOT NULL,
    appointment_date VARCHAR(50) NOT NULL,
    appointment_time VARCHAR(50) NOT NULL,
    status VARCHAR(50) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Sample Data

The application comes with pre-loaded sample data:

| ID | Client ID | Provider ID | Service ID | Date | Time | Status |
|----|-----------|-------------|------------|------|------|--------|
| 1 | 101 | 201 | 301 | 2025-11-20 | 10:00 | Scheduled |
| 2 | 102 | 202 | 302 | 2025-11-21 | 14:30 | Confirmed |
| 3 | 103 | 203 | 303 | 2025-11-22 | 09:00 | Completed |

## Development

### Running Tests
```bash
mvn test
```

### Building JAR
```bash
mvn clean package
java -jar target/restapi_app-0.0.1-SNAPSHOT.jar
```

### Hot Reload
The application uses Spring Boot DevTools for automatic restart when code changes are detected. Simply save your files and the application will restart automatically.

### Accessing H2 Console
1. Navigate to http://localhost:9080/h2-console
2. Use these settings:
   - JDBC URL: `jdbc:h2:mem:appointmentdb`
   - Username: `sa`
   - Password: (leave empty)

## Deployment

### AWS RDS Setup

1. **Create RDS MySQL Instance**
   - Engine: MySQL 8.0+
   - Instance class: db.t3.micro (free tier)
   - Storage: 20 GB
   - Enable public access for testing

2. **Update application.properties**
   ```properties
   spring.datasource.url=jdbc:mysql://your-rds-endpoint:3306/appointmentdb?useSSL=false&serverTimezone=UTC
   spring.datasource.username=admin
   spring.datasource.password=your-password
   ```

3. **Run schema.sql on RDS**
   ```bash
   mysql -h your-rds-endpoint -u admin -p appointmentdb < src/main/resources/schema.sql
   ```

4. **Deploy application**
   ```bash
   mvn clean package
   java -jar target/restapi_app-0.0.1-SNAPSHOT.jar
   ```

## Troubleshooting

### Application won't start
```bash
# Check Java version
java -version  # Should be 21+

# Check port availability
netstat -ano | findstr :9080  # Windows
lsof -i :9080                 # Mac/Linux

# Clean and rebuild
mvn clean install
```

### Database connection errors
```bash
# For H2: Check console at http://localhost:9080/h2-console
# For MySQL: Test connection
mysql -h your-host -u your-user -p
```

### Frontend can't connect to backend
- Verify backend is running: `curl http://localhost:9080/appointments/get`
- Check browser console for CORS errors
- Clear browser cache (Ctrl+Shift+Delete)

## Common Commands

```bash
# Run application
mvn spring-boot:run

# Run tests
mvn test

# Build JAR
mvn clean package

# Run JAR
java -jar target/restapi_app-0.0.1-SNAPSHOT.jar

# Clean build
mvn clean

# Skip tests during build
mvn clean package -DskipTests
```

## Security

- CORS enabled for all origins (configure in `WebConfig.java` for production)
- SQL injection prevention through PreparedStatements (JdbcTemplate)
- Environment-based configuration support
- Credentials should be externalized for production

## Future Enhancements

- [ ] Add authentication and authorization
- [ ] Implement pagination for large datasets
- [ ] Add input validation and error handling
- [ ] Implement search and filter functionality
- [ ] Add email notifications for appointments
- [ ] Docker containerization
- [ ] CI/CD pipeline with Jenkins
- [ ] Comprehensive unit and integration tests

## Project Submission

This project demonstrates:
- RESTful API design principles
- Layered architecture pattern
- Spring Boot framework capabilities
- Database integration (H2 and MySQL)
- Cloud deployment readiness (AWS RDS)
- Full-stack development skills

## Contributing

1. Fork the repository
2. Create feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open Pull Request

## License

This project is created for educational purposes as part of a Cloud Computing course.

## Support

For issues and questions:
- Check the troubleshooting section
- Review the API documentation
- Contact team members

---

**Developed by:** Ishita Parkar, Kirshnandu Gurey, Prutha Jadhav, Shreeraj Salvi and Yash Adhau  
**Course:** Cloud Computing  
**Year:** 2025 
