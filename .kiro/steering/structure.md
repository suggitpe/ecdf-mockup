# Project Structure

## Project Organization

This is a full-stack web application with separate backend and frontend projects.

### Overall Structure
```
career-development-assessment/
├── backend/              # Spring Boot backend
├── frontend/             # React frontend
├── .kiro/               # Kiro spec and steering files
│   ├── specs/
│   │   └── career-development-assessment/
│   └── steering/
├── docs/                # Additional documentation
└── README.md
```

## Backend Structure (Spring Boot)

```
backend/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/careerdev/
│   │   │       ├── controller/      # REST API controllers
│   │   │       │   ├── AuthController.java
│   │   │       │   ├── EmployeeController.java
│   │   │       │   ├── EvidenceController.java
│   │   │       │   ├── EvidenceScoreController.java
│   │   │       │   ├── ECDFAssessmentController.java
│   │   │       │   ├── ProgressController.java
│   │   │       │   ├── DevelopmentPlanController.java
│   │   │       │   ├── ManagerController.java
│   │   │       │   └── IndependentAssessorController.java
│   │   │       ├── service/         # Business logic
│   │   │       │   ├── AuthService.java
│   │   │       │   ├── EmployeeService.java
│   │   │       │   ├── EvidenceService.java
│   │   │       │   ├── EvidenceScoreService.java
│   │   │       │   ├── ECDFAssessmentService.java
│   │   │       │   ├── ScoreAggregationService.java
│   │   │       │   ├── PerformanceEvaluationService.java
│   │   │       │   ├── ProgressService.java
│   │   │       │   ├── DevelopmentPlanService.java
│   │   │       │   ├── ManagerService.java
│   │   │       │   └── IndependentAssessorService.java
│   │   │       ├── repository/      # Data access (Spring Data JPA)
│   │   │       │   ├── UserRepository.java
│   │   │       │   ├── EmployeeRepository.java
│   │   │       │   ├── EvidenceRepository.java
│   │   │       │   ├── EvidenceScoreRepository.java
│   │   │       │   ├── ECDFAssessmentRepository.java
│   │   │       │   ├── PillarScoreRepository.java
│   │   │       │   ├── ExpectedScoreRepository.java
│   │   │       │   ├── DevelopmentPlanRepository.java
│   │   │       │   └── DevelopmentGoalRepository.java
│   │   │       ├── model/           # JPA entities
│   │   │       │   ├── User.java
│   │   │       │   ├── Employee.java
│   │   │       │   ├── Evidence.java
│   │   │       │   ├── EvidenceScore.java
│   │   │       │   ├── ECDFAssessment.java
│   │   │       │   ├── PillarScore.java
│   │   │       │   ├── ExpectedScore.java
│   │   │       │   ├── DevelopmentPlan.java
│   │   │       │   └── DevelopmentGoal.java
│   │   │       ├── model/enums/     # Enum types
│   │   │       │   ├── UserRole.java
│   │   │       │   ├── EmployeeGrade.java
│   │   │       │   ├── PillarName.java
│   │   │       │   ├── AssessorType.java
│   │   │       │   ├── PerformanceStatus.java
│   │   │       │   └── GoalStatus.java
│   │   │       ├── dto/             # Data Transfer Objects
│   │   │       │   ├── request/
│   │   │       │   └── response/
│   │   │       ├── security/        # Security configuration
│   │   │       │   ├── JwtTokenProvider.java
│   │   │       │   ├── JwtAuthenticationFilter.java
│   │   │       │   └── SecurityConfig.java
│   │   │       ├── config/          # Application configuration
│   │   │       ├── exception/       # Custom exceptions
│   │   │       └── util/            # Utility classes
│   │   └── resources/
│   │       ├── application.properties
│   │       ├── application-dev.properties
│   │       ├── application-prod.properties
│   │       └── db/migration/        # Flyway/Liquibase migrations
│   └── test/
│       └── java/
│           └── com/careerdev/
│               ├── unit/            # Unit tests
│               │   ├── service/
│               │   ├── repository/
│               │   └── util/
│               └── integration/     # Integration tests
│                   ├── controller/
│                   └── security/
├── build.gradle
└── gradle.properties
```

## Frontend Structure (React)

```
frontend/
├── src/
│   ├── components/          # Reusable UI components
│   │   ├── auth/
│   │   │   ├── LoginPage.jsx
│   │   │   ├── ProtectedRoute.jsx
│   │   │   └── RoleBasedRedirect.jsx
│   │   ├── employee/
│   │   │   ├── EmployeeProfile.jsx
│   │   │   └── EmployeeDashboard.jsx
│   │   ├── evidence/
│   │   │   ├── EvidenceList.jsx
│   │   │   ├── EvidenceForm.jsx
│   │   │   ├── EvidenceCard.jsx
│   │   │   └── EvidenceDetailView.jsx
│   │   ├── scoring/
│   │   │   ├── EvidenceScoreForm.jsx
│   │   │   ├── PillarSelector.jsx
│   │   │   ├── ScoreInput.jsx
│   │   │   ├── PillarDescription.jsx
│   │   │   ├── ScoreHistoryView.jsx
│   │   │   └── MultiAssessorComparison.jsx
│   │   ├── assessment/
│   │   │   ├── ECDFAssessmentForm.jsx
│   │   │   ├── ECDFVersionList.jsx
│   │   │   ├── ECDFVersionDetail.jsx
│   │   │   ├── ECDFComparison.jsx
│   │   │   ├── PillarScoreDisplay.jsx
│   │   │   └── DraftSaveButton.jsx
│   │   ├── performance/
│   │   │   ├── PerformanceStatusCard.jsx
│   │   │   ├── ExpectedScoreComparison.jsx
│   │   │   └── PerformanceIndicator.jsx
│   │   ├── development/
│   │   │   ├── DevelopmentPlanForm.jsx
│   │   │   ├── DevelopmentGoalCard.jsx
│   │   │   ├── GoalProgressTracker.jsx
│   │   │   └── PlanOutcomeView.jsx
│   │   ├── progress/
│   │   │   ├── ProgressDashboard.jsx
│   │   │   ├── PillarRadarChart.jsx
│   │   │   ├── PillarTrendChart.jsx
│   │   │   ├── ECDFHistoryTimeline.jsx
│   │   │   └── EvidenceGrowthChart.jsx
│   │   ├── manager/
│   │   │   ├── ManagerDashboard.jsx
│   │   │   ├── TeamMemberList.jsx
│   │   │   ├── EmployeeEvidenceReview.jsx
│   │   │   ├── EmployeeProgressView.jsx
│   │   │   ├── TeamOverviewDashboard.jsx
│   │   │   ├── TeamPerformanceHeatmap.jsx
│   │   │   ├── TeamTrendAnalysis.jsx
│   │   │   └── TeamReportExport.jsx
│   │   ├── assessor/
│   │   │   ├── AssessorDashboard.jsx
│   │   │   ├── AssignedEmployeeList.jsx
│   │   │   ├── EmployeeEvidenceAssessment.jsx
│   │   │   └── AssessmentProgressTracker.jsx
│   │   └── common/          # Shared components
│   ├── pages/               # Page-level components
│   │   ├── EmployeePage.jsx
│   │   ├── ManagerPage.jsx
│   │   └── AssessorPage.jsx
│   ├── services/            # API service layer
│   │   ├── authService.js
│   │   ├── employeeService.js
│   │   ├── evidenceService.js
│   │   ├── evidenceScoreService.js
│   │   ├── ecdfAssessmentService.js
│   │   ├── progressService.js
│   │   ├── developmentPlanService.js
│   │   └── managerService.js
│   ├── hooks/               # Custom React hooks
│   │   ├── useAuth.js
│   │   ├── useEvidence.js
│   │   └── useAssessment.js
│   ├── contexts/            # React contexts
│   │   └── AuthContext.jsx
│   ├── utils/               # Utility functions
│   │   ├── api.js
│   │   ├── validation.js
│   │   └── formatting.js
│   ├── styles/              # Global styles
│   ├── assets/              # Images, fonts, etc.
│   ├── App.jsx
│   └── index.jsx
├── public/
├── package.json
└── .env.example
```

## Naming Conventions

### Backend (Java)
- **Classes**: PascalCase (e.g., `EvidenceService`, `UserRepository`)
- **Methods**: camelCase (e.g., `createEvidence`, `getEmployeeProfile`)
- **Variables**: camelCase (e.g., `employeeId`, `pillarScore`)
- **Constants**: UPPER_SNAKE_CASE (e.g., `MAX_SCORE`, `DEFAULT_GRADE`)
- **Packages**: lowercase (e.g., `com.careerdev.service`)
- **Enums**: PascalCase with UPPER_CASE values (e.g., `UserRole.EMPLOYEE`)

### Frontend (React)
- **Components**: PascalCase (e.g., `EvidenceForm`, `ManagerDashboard`)
- **Files**: Match component name (e.g., `EvidenceForm.jsx`)
- **Functions**: camelCase (e.g., `handleSubmit`, `fetchEvidence`)
- **Variables**: camelCase (e.g., `evidenceList`, `currentUser`)
- **Constants**: UPPER_SNAKE_CASE (e.g., `API_BASE_URL`)
- **CSS classes**: kebab-case (e.g., `evidence-card`, `pillar-score`)

### Database
- **Tables**: snake_case plural (e.g., `users`, `evidence_scores`)
- **Columns**: snake_case (e.g., `employee_id`, `created_at`)
- **Indexes**: `idx_` prefix (e.g., `idx_employee_version`)
- **Foreign keys**: `fk_` prefix (e.g., `fk_evidence_employee`)

## File Organization Rules

### Backend
- Group by layer (controller, service, repository, model)
- Keep related entities in same package
- Separate DTOs by request/response
- Place enums in dedicated package
- One class per file
- Test files mirror source structure

### Frontend
- Group components by feature/domain
- Keep component, styles, and tests together
- Shared components in `common/` directory
- One component per file
- Co-locate related components

### General
- Keep related files together
- Separate concerns appropriately
- Follow framework conventions
- Maintain consistent structure across similar modules
- Use meaningful directory names that reflect purpose
