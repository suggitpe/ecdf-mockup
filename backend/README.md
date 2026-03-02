# Career Development Assessment - Backend

Spring Boot backend for the Career Development Assessment application.

## Prerequisites

- Java 17 or higher
- Gradle 7.5+ (or use the included Gradle wrapper)
- PostgreSQL 14+ (for production) or H2 (for development)

## Project Structure

```
backend/
├── src/
│   ├── main/
│   │   ├── java/com/careerdev/
│   │   │   ├── controller/      # REST API controllers
│   │   │   ├── service/         # Business logic
│   │   │   ├── repository/      # Data access
│   │   │   ├── model/           # JPA entities
│   │   │   ├── model/enums/     # Enum types
│   │   │   ├── dto/             # Data Transfer Objects
│   │   │   ├── security/        # Security configuration
│   │   │   ├── config/          # Application configuration
│   │   │   ├── exception/       # Custom exceptions
│   │   │   └── util/            # Utility classes
│   │   └── resources/
│   │       ├── application.properties
│   │       ├── application-dev.properties
│   │       └── application-prod.properties
│   └── test/
│       └── java/com/careerdev/
├── build.gradle
└── settings.gradle
```

## Getting Started

### 1. Build the project

```bash
./gradlew build
```

### 2. Run tests

```bash
./gradlew test
```

### 3. Run the application (development mode with H2)

```bash
./gradlew bootRun
```

The application will start on `http://localhost:8080`

### 4. Run with PostgreSQL (production mode)

First, ensure PostgreSQL is running and create the database:

```bash
createdb career_dev
```

Then run with the prod profile:

```bash
./gradlew bootRun --args='--spring.profiles.active=prod'
```

## Configuration

### Development (H2)
- Database: In-memory H2
- H2 Console: http://localhost:8080/h2-console
- JDBC URL: `jdbc:h2:mem:testdb`
- Username: `sa`
- Password: (empty)

### Production (PostgreSQL)
- Database: PostgreSQL
- Configure connection in `application-prod.properties`
- Default: `jdbc:postgresql://localhost:5432/career_dev`

### Environment Variables

Set these environment variables or update the properties files:

- `JWT_SECRET`: Secret key for JWT token generation (min 256 bits)
- `JWT_EXPIRATION`: Token expiration time in milliseconds (default: 86400000 = 24 hours)
- `SPRING_DATASOURCE_URL`: Database connection URL
- `SPRING_DATASOURCE_USERNAME`: Database username
- `SPRING_DATASOURCE_PASSWORD`: Database password

## API Documentation

Once the application is running, the REST API will be available at:
- Base URL: `http://localhost:8080/api`

## Security

- JWT-based authentication
- BCrypt password hashing
- Role-based access control (Employee, Manager, Independent_Assessor)
- Currently configured to permit all requests for initial development

## Testing

Run all tests:
```bash
./gradlew test
```

Run tests with coverage:
```bash
./gradlew test jacocoTestReport
```

## Build for Production

```bash
./gradlew clean build
```

The JAR file will be created in `build/libs/`
