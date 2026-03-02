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

### Frontend
- **Language**: JavaScript/TypeScript
- **Framework**: React 18+
- **Key Libraries**:
  - React Router (Navigation)
  - Axios or Fetch API (HTTP client)
  - State management (Context API, Redux, or Zustand)
  - UI component library (Material-UI, Ant Design, or similar)

### Database
- **Primary Database**: PostgreSQL 14+
- **Development**: PostgreSQL (can use Docker for local setup)

### Architecture
- RESTful API backend
- Single Page Application (SPA) frontend
- Separate backend and frontend deployments

## Common Commands

### Backend (Spring Boot with Gradle)
```bash
# Install dependencies and build
./gradlew build

# Run development server
./gradlew bootRun

# Run tests
./gradlew test

# Build for production
./gradlew clean build

# Run with specific profile
./gradlew bootRun --args='--spring.profiles.active=dev'
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
docker run --name career-dev-postgres -e POSTGRES_PASSWORD=password -e POSTGRES_DB=career_dev -p 5432:5432 -d postgres:14

# Connect to PostgreSQL
psql -h localhost -U postgres -d career_dev
```

## Development Environment

### Required Tools
- Java Development Kit (JDK) 17 or higher
- Gradle 7.5+ (or use Gradle wrapper)
- Node.js 18+ and npm/yarn
- PostgreSQL 14+ (or Docker)
- IDE: IntelliJ IDEA, VS Code, or Eclipse

### Environment Variables
- Backend: 
  - `SPRING_DATASOURCE_URL`: PostgreSQL connection string (e.g., jdbc:postgresql://localhost:5432/career_dev)
  - `SPRING_DATASOURCE_USERNAME`: Database username
  - `SPRING_DATASOURCE_PASSWORD`: Database password
  - `JWT_SECRET`: Secret key for JWT token generation
  - `SERVER_PORT`: Server port (default 8080)
- Frontend: 
  - `REACT_APP_API_URL`: Backend API base URL
  - `NODE_ENV`: Environment mode (development/production)

### Local Setup
1. Install JDK 17+
2. Install Node.js 18+
3. Install PostgreSQL 14+ or Docker
4. Clone repository
5. Set up database: Create PostgreSQL database `career_dev`
6. Configure environment variables (create `.env` files or use IDE configuration)
7. Set up backend: `cd backend && ./gradlew build`
8. Set up frontend: `cd frontend && npm install`
9. Run backend: `./gradlew bootRun`
10. Run frontend: `npm start`
