# Technology Stack

## Build System & Package Management

- **Backend**: Gradle (Java/Spring Boot)
- **Frontend**: npm or yarn (React)

## Tech Stack

### Backend
- **Language**: Java 17+
- **Framework**: Spring Boot 3.x
- **Build Tool**: Gradle 7.5+
- **Key Libraries**:
  - Spring Web (REST APIs)
  - Spring Data JPA (Data persistence)
  - Spring Security (Authentication & authorization)
  - Spring Validation (Input validation)
  - BCrypt (Password hashing)
  - JPA/Hibernate (ORM)
- **Testing**:
  - JUnit 5 (Unit testing)
  - Mockito (Mocking dependencies)
  - Spring Boot Test (Integration testing)
  - H2 (In-memory database for testing)

### Frontend
- **Language**: JavaScript/TypeScript
- **Framework**: React 18+
- **Key Libraries**:
  - React Router (Navigation)
  - Axios (HTTP client)
  - Context API or Zustand (State management)
  - Chart.js or Recharts (Data visualization)
  - Material-UI or Ant Design (UI component library)
- **Testing**:
  - Jest (Unit testing)
  - React Testing Library (Component testing)
  - MSW (Mock Service Worker for API mocking)

### Database
- **Primary Database**: PostgreSQL 14+
- **Development**: H2 (in-memory) or PostgreSQL with Docker
- **Migration Tool**: Flyway or Liquibase

### Architecture
- RESTful API backend
- Single Page Application (SPA) frontend
- Separate backend and frontend deployments
- Three-tier architecture (Controller → Service → Repository)
- JWT-based authentication

## Development Approach

### Test-Driven Development (TDD)
- Write tests first, then implementation
- Focus on class behavior testing (NOT property-based testing)
- Use JUnit 5 for all backend tests
- Test business logic, integration points, and error conditions
- Maintain 80%+ code coverage for services and controllers

### Testing Strategy
- **Unit Tests**: Test individual classes and methods in isolation
- **Integration Tests**: Test API endpoints and database interactions
- **Repository Tests**: Test database queries with H2
- **Component Tests**: Test React components with React Testing Library

### Key Principles
- Small, incremental tasks
- Test before implementation
- Focus on class behavior
- Verify business logic correctness
- Test error conditions and edge cases

## Common Commands

### Backend (Spring Boot with Gradle)
```bash
# Install dependencies and build
./gradlew build

# Run development server
./gradlew bootRun

# Run tests
./gradlew test

# Run tests with coverage
./gradlew test jacocoTestReport

# Build for production
./gradlew clean build

# Run with specific profile (dev/prod)
./gradlew bootRun --args='--spring.profiles.active=dev'

# Run with H2 database (development)
./gradlew bootRun --args='--spring.profiles.active=dev'

# Run with PostgreSQL (production)
./gradlew bootRun --args='--spring.profiles.active=prod'
```

### Frontend (React)
```bash
# Install dependencies
npm install
# or
yarn install

# Run development server
npm start
# or
yarn start

# Run tests
npm test
# or
yarn test

# Run tests with coverage
npm test -- --coverage
# or
yarn test --coverage

# Build for production
npm run build
# or
yarn build

# Lint/format code
npm run lint
# or
yarn lint
```

### Database (PostgreSQL)
```bash
# Start PostgreSQL with Docker
docker run --name career-dev-postgres \
  -e POSTGRES_PASSWORD=password \
  -e POSTGRES_DB=career_dev \
  -p 5432:5432 \
  -d postgres:14

# Stop PostgreSQL
docker stop career-dev-postgres

# Start existing container
docker start career-dev-postgres

# Connect to PostgreSQL
psql -h localhost -U postgres -d career_dev

# Run migrations (if using Flyway)
./gradlew flywayMigrate

# View migration status
./gradlew flywayInfo
```

## Development Environment

### Required Tools
- Java Development Kit (JDK) 17 or higher
- Gradle 7.5+ (or use Gradle wrapper `./gradlew`)
- Node.js 18+ and npm/yarn
- PostgreSQL 14+ (or Docker)
- IDE: IntelliJ IDEA (recommended), VS Code, or Eclipse
- Git for version control

### Environment Variables

#### Backend
- `SPRING_DATASOURCE_URL`: PostgreSQL connection string
  - Dev: `jdbc:h2:mem:testdb` (H2 in-memory)
  - Prod: `jdbc:postgresql://localhost:5432/career_dev`
- `SPRING_DATASOURCE_USERNAME`: Database username (default: `postgres`)
- `SPRING_DATASOURCE_PASSWORD`: Database password
- `JWT_SECRET`: Secret key for JWT token generation (min 256 bits)
- `JWT_EXPIRATION`: Token expiration time in milliseconds (default: 86400000 = 24 hours)
- `SERVER_PORT`: Server port (default: 8080)
- `SPRING_PROFILES_ACTIVE`: Active profile (`dev` or `prod`)

#### Frontend
- `REACT_APP_API_URL`: Backend API base URL
  - Dev: `http://localhost:8080/api`
  - Prod: `https://api.example.com/api`
- `NODE_ENV`: Environment mode (`development` or `production`)

### Local Setup

1. **Install Prerequisites**
   ```bash
   # Verify Java installation
   java -version  # Should be 17+
   
   # Verify Node.js installation
   node -version  # Should be 18+
   
   # Verify Gradle (optional, can use wrapper)
   gradle -version
   ```

2. **Clone Repository**
   ```bash
   git clone <repository-url>
   cd career-development-assessment
   ```

3. **Set Up Database**
   ```bash
   # Option 1: Use Docker
   docker run --name career-dev-postgres \
     -e POSTGRES_PASSWORD=password \
     -e POSTGRES_DB=career_dev \
     -p 5432:5432 \
     -d postgres:14
   
   # Option 2: Install PostgreSQL locally
   # Create database: career_dev
   createdb career_dev
   ```

4. **Configure Backend**
   ```bash
   cd backend
   
   # Create application-dev.properties (or use environment variables)
   # Set database connection, JWT secret, etc.
   
   # Build project
   ./gradlew build
   
   # Run tests to verify setup
   ./gradlew test
   ```

5. **Configure Frontend**
   ```bash
   cd frontend
   
   # Install dependencies
   npm install
   
   # Create .env file
   echo "REACT_APP_API_URL=http://localhost:8080/api" > .env
   
   # Run tests to verify setup
   npm test
   ```

6. **Run Application**
   ```bash
   # Terminal 1: Start backend
   cd backend
   ./gradlew bootRun
   
   # Terminal 2: Start frontend
   cd frontend
   npm start
   ```

7. **Access Application**
   - Frontend: http://localhost:3000
   - Backend API: http://localhost:8080/api
   - H2 Console (dev): http://localhost:8080/h2-console

## Project-Specific Requirements

### 9-Pillar Framework
The system is built around a 9-pillar career development framework:
1. **Thinks**: Strategic thinking and problem-solving
2. **Engages**: Stakeholder engagement and communication
3. **Influences**: Leadership and influence
4. **Achieves**: Results delivery and execution
5. **Defines**: Requirements definition and analysis
6. **Designs**: Solution design and architecture
7. **Delivers**: Implementation and delivery
8. **Controls**: Quality control and governance
9. **Operates**: Operations and maintenance

### Evidence Quality Factors
Every evidence item must include three quality factors:
1. **Impact**: Effect or value to the firm
2. **Complexity**: Difficulty, scope, or sophistication
3. **Personal Contribution**: Individual's specific role and input

### Multi-Role Assessment
- **Employee**: Creates evidence and performs self-assessment
- **Manager**: Independently reviews and rates employee evidence
- **Independent Assessor**: Provides unbiased third-party evaluation
- All three roles score evidence independently (1-5 for each pillar)

### Versioning and Temporal Tracking
- **Evidence Score Versioning**: Each score update creates a new version (point-in-time snapshot)
- **ECDF Assessment Versioning**: Multiple assessments over time to track growth
- **Immutable History**: Previous versions never modified, only new versions created
- **Temporal Queries**: Support for "as of" date queries

### Performance Evaluation
- **Employee Grades**: JUNIOR, MID_LEVEL, SENIOR, PRINCIPAL
- **Expected Scores**: Configured per grade and pillar
- **Performance Status**: UNDER_PERFORMING, AT_EXPECTATION, OVER_PERFORMING
- **Development Plans**: Created for under-performing pillars

### Security Requirements
- JWT-based authentication
- Role-based access control (RBAC)
- Data encryption at rest
- HTTPS for all communications
- Assessment independence (assessors can't see each other's scores before completing their own)

## Platform Constraints

- **Web-Only**: Desktop and laptop browsers only (no mobile app)
- **Supported Browsers**: Chrome, Firefox, Safari, Edge (latest versions)
- **Responsive Design**: Adapt to different browser window sizes
- **No Offline Support**: Requires internet connection
