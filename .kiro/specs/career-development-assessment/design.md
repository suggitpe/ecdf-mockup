# Design Document: Career Development Assessment

## Overview

The Career Development Assessment system is a full-stack web application built with Spring Boot (backend) and React (frontend) that enables employees to evaluate and track their career growth through a structured 9-pillar framework. The system emphasizes evidence-based assessment with multi-role evaluation, where employees document concrete work activities and receive independent assessments from themselves, their managers, and independent assessors.

### Key Design Principles

1. **Evidence-Driven Assessment**: All pillar scores are aggregated from evidence scores across multiple evidence items
2. **Multi-Role Independent Assessment**: Employees, Managers, and Independent Assessors score evidence independently
3. **Point-in-Time Versioning**: Evidence scores and ECDF assessments are versioned snapshots that preserve historical state
4. **Temporal Growth Tracking**: Employees can create multiple ECDF assessments over time as they accumulate evidence
5. **Performance-Based Development**: System compares scores against grade expectations to identify development needs
6. **Data Integrity**: Assessment responses and evidence must be reliably persisted and retrievable
7. **Security First**: Employee data must be encrypted with role-based access controls
8. **Web-Only Interface**: Optimized for desktop and laptop browsers, no mobile app requirements

### System Boundaries

**In Scope:**
- Multi-role authentication (Employee, Manager, Independent_Assessor)
- Employee profile display with grade information
- Evidence creation with three quality factors (Impact, Complexity, Personal_Contribution)
- Multi-role evidence scoring (Employee, Manager, Independent_Assessor score independently)
- Evidence score versioning (point-in-time snapshots)
- ECDF assessment versioning (multiple assessments over time)
- Evidence score aggregation to calculate pillar scores
- Expected score configuration by employee grade
- Performance status evaluation (under-performing, at-expectation, over-performing)
- Personal development plans for under-performing pillars
- Manager views for individual and team progress tracking
- Progress visualization across multiple ECDF versions
- Draft saving and restoration for all roles
- Data encryption and role-based access control

**Out of Scope:**
- Automated scoring or AI-based recommendations
- Integration with HR systems or performance management platforms
- Multi-language support (initial version English only)
- Mobile applications (web-only interface)
- Real-time collaboration features
- Notification systems (email/SMS)

## Architecture

### High-Level Architecture

The system follows a three-tier architecture with clear separation of concerns:

```
┌──────────────────────────────────────────────────────────────┐
│                    React Frontend (SPA)                       │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐       │
│  │  Employee    │  │   Manager    │  │ Independent  │       │
│  │   Views      │  │    Views     │  │  Assessor    │       │
│  │              │  │              │  │   Views      │       │
│  │ - Evidence   │  │ - Team       │  │ - Evidence   │       │
│  │ - Assessment │  │   Dashboard  │  │   Review     │       │
│  │ - Progress   │  │ - Individual │  │ - Assessment │       │
│  │ - Dev Plans  │  │   Progress   │  │   Scoring    │       │
│  └──────────────┘  └──────────────┘  └──────────────┘       │
│           │                │                 │                │
│           └────────────────┴─────────────────┘                │
│                          │                                     │
│                   HTTP/REST (JSON)                             │
└───────────────────────────┼────────────────────────────────────┘
                            │
┌───────────────────────────┼────────────────────────────────────┐
│              Spring Boot Backend (REST API)                    │
│  ┌─────────────────────────────────────────────────────┐      │
│  │              Controllers Layer                       │      │
│  │  AuthController │ EmployeeController │               │      │
│  │  EvidenceController │ EvidenceScoreController │      │      │
│  │  ECDFAssessmentController │ ProgressController │     │      │
│  │  DevelopmentPlanController │ ManagerController       │      │
│  └─────────────────────────────────────────────────────┘      │
│                          │                                     │
│  ┌─────────────────────────────────────────────────────┐      │
│  │              Services Layer                          │      │
│  │  AuthService │ EmployeeService │                     │      │
│  │  EvidenceService │ EvidenceScoreService │            │      │
│  │  ECDFAssessmentService │ ScoreAggregationService │   │      │
│  │  ProgressService │ DevelopmentPlanService │          │      │
│  │  ManagerService │ PerformanceEvaluationService       │      │
│  └─────────────────────────────────────────────────────┘      │
│                          │                                     │
│  ┌─────────────────────────────────────────────────────┐      │
│  │          Repository Layer (Spring Data JPA)          │      │
│  │  UserRepository │ EmployeeRepository │               │      │
│  │  EvidenceRepository │ EvidenceScoreRepository │      │      │
│  │  ECDFAssessmentRepository │ PillarScoreRepository │  │      │
│  │  ExpectedScoreRepository │ DevelopmentPlanRepository │      │
│  └─────────────────────────────────────────────────────┘      │
└───────────────────────────┼────────────────────────────────────┘
                            │
┌───────────────────────────┼────────────────────────────────────┐
│                  PostgreSQL Database                           │
│         (H2 for development/testing)                           │
└────────────────────────────────────────────────────────────────┘
```

### Component Responsibilities

**Frontend (React)**
- Role-specific user interface rendering (Employee, Manager, Independent_Assessor)
- Form validation and user input handling
- State management for assessment drafts (all roles)
- API communication via Axios/Fetch
- Progress visualization using charting library (Chart.js or Recharts)
- Multi-assessor score comparison views
- Manager team dashboard and analytics

**Backend (Spring Boot)**
- RESTful API endpoints for all roles
- Business logic enforcement
- Multi-role evidence scoring logic
- Evidence score versioning and point-in-time snapshots
- ECDF assessment versioning and temporal tracking
- Score aggregation algorithms (combining Employee, Manager, Independent_Assessor scores)
- Performance status evaluation against expected scores
- Data validation and sanitization
- Authentication and role-based authorization (Spring Security)
- Database operations via JPA
- Data encryption/decryption

**Database (PostgreSQL)**
- Persistent storage for users, employees, evidence, evidence scores, ECDF assessments
- Evidence score versioning with temporal tracking
- ECDF assessment versioning with point-in-time snapshots
- Expected scores by employee grade
- Personal development plans
- Relational integrity enforcement
- Query optimization for progress tracking and manager views

### Technology Stack Details

**Backend:**
- Java 17+
- Spring Boot 3.x
- Spring Web (REST APIs)
- Spring Data JPA (persistence)
- Spring Security (authentication/authorization)
- Spring Validation (input validation)
- BCrypt (password hashing)
- JPA/Hibernate (ORM)

**Frontend:**
- React 18+
- React Router (navigation)
- Axios (HTTP client)
- Context API or Zustand (state management)
- Chart.js or Recharts (visualization)
- Material-UI or Ant Design (UI components)

**Database:**
- PostgreSQL (production) or H2 (development)

## Components and Interfaces

### Backend Components

#### 1. Authentication Module

**AuthController**
- `POST /api/auth/login`: Authenticate user credentials and return role
- `POST /api/auth/logout`: Terminate session
- `GET /api/auth/session`: Validate current session and return user role

**AuthService**
- `authenticate(username, password)`: Verify credentials, determine role, create session
- `validateSession(sessionToken)`: Check if session is active and return user role
- `terminateSession(sessionToken)`: End session
- `getUserRole(userId)`: Retrieve user's role (Employee, Manager, Independent_Assessor)

#### 2. Employee Module

**EmployeeController**
- `GET /api/employees/profile`: Get authenticated employee's profile
- `GET /api/employees/{id}/profile`: Get employee profile (Manager/Assessor access)
- `GET /api/employees/{id}/grade`: Get employee grade information

**EmployeeService**
- `getEmployeeProfile(employeeId)`: Retrieve employee record with grade
- `getEmployeeGrade(employeeId)`: Get employee's current grade
- `getExpectedScores(employeeId)`: Get expected scores for employee's grade

#### 3. Evidence Module

**EvidenceController**
- `POST /api/evidence`: Create new evidence item (Employee only)
- `GET /api/evidence`: Retrieve all evidence for authenticated employee
- `GET /api/employees/{employeeId}/evidence`: Retrieve employee evidence (Manager/Assessor access)
- `GET /api/evidence/{id}`: Retrieve specific evidence item
- `PUT /api/evidence/{id}`: Update evidence item (Employee only, own evidence)
- `DELETE /api/evidence/{id}`: Delete evidence item (Employee only, own evidence)

**EvidenceService**
- `createEvidence(employeeId, evidenceData)`: Validate and persist evidence with quality factors
- `getEmployeeEvidence(employeeId, requestorId, requestorRole)`: Retrieve evidence with access control
- `getEvidenceById(evidenceId, requestorId, requestorRole)`: Retrieve specific evidence with access control
- `updateEvidence(evidenceId, employeeId, evidenceData)`: Update existing evidence
- `deleteEvidence(evidenceId, employeeId)`: Remove evidence
- `validateEvidenceQuality(evidenceData)`: Ensure Impact, Complexity, Personal_Contribution are present

#### 4. Evidence Score Module

**EvidenceScoreController**
- `POST /api/evidence/{evidenceId}/scores`: Create evidence scores for pillars (all roles)
- `GET /api/evidence/{evidenceId}/scores`: Get all scores for evidence item
- `GET /api/evidence/{evidenceId}/scores/my`: Get authenticated user's scores for evidence
- `PUT /api/evidence/{evidenceId}/scores/{scoreId}`: Update evidence score (creates new version)
- `GET /api/evidence/{evidenceId}/scores/history`: Get score version history
- `GET /api/evidence/{evidenceId}/scores/compare`: Compare scores across assessor types

**EvidenceScoreService**
- `createEvidenceScore(evidenceId, assessorId, assessorType, pillarName, score)`: Create versioned evidence score
- `getEvidenceScores(evidenceId, assessorType)`: Retrieve scores filtered by assessor type
- `getEvidenceScoresByAssessor(evidenceId, assessorId)`: Get specific assessor's scores
- `updateEvidenceScore(scoreId, assessorId, newScore)`: Create new version of evidence score
- `getScoreHistory(evidenceId, pillarName)`: Retrieve all versions for evidence-pillar combination
- `compareScoresByAssessorType(evidenceId)`: Get scores grouped by assessor type
- `validateScoreRange(score)`: Ensure score is 1-5
- `checkAssessmentIndependence(evidenceId, assessorId, assessorType)`: Prevent viewing others' scores before completing own

#### 5. ECDF Assessment Module

**ECDFAssessmentController**
- `POST /api/assessments/ecdf`: Create new ECDF assessment version (Employee only)
- `GET /api/assessments/ecdf`: Retrieve all ECDF versions for authenticated employee
- `GET /api/employees/{employeeId}/assessments/ecdf`: Get employee's ECDF versions (Manager/Assessor access)
- `GET /api/assessments/ecdf/{versionId}`: Retrieve specific ECDF version
- `GET /api/assessments/ecdf/latest`: Get most recent ECDF assessment
- `GET /api/assessments/ecdf/compare`: Compare multiple ECDF versions
- `POST /api/assessments/ecdf/draft`: Save draft ECDF assessment
- `PUT /api/assessments/ecdf/draft/{draftId}`: Update draft
- `POST /api/assessments/ecdf/draft/{draftId}/submit`: Submit draft as complete version

**ECDFAssessmentService**
- `createECDFAssessment(employeeId, isDraft)`: Create new versioned ECDF assessment
- `getEmployeeECDFVersions(employeeId, requestorId, requestorRole)`: Retrieve all versions with access control
- `getECDFVersionById(versionId, requestorId, requestorRole)`: Retrieve specific version
- `getLatestECDFVersion(employeeId)`: Get most recent assessment
- `compareECDFVersions(versionIds)`: Generate comparison data
- `saveDraft(employeeId, draftData)`: Persist draft assessment
- `updateDraft(draftId, employeeId, draftData)`: Update existing draft
- `submitDraft(draftId, employeeId)`: Convert draft to complete version
- `validateCompleteness(assessmentData)`: Ensure all 9 pillars have scores

#### 6. Score Aggregation Module

**ScoreAggregationService**
- `aggregatePillarScores(employeeId, pillarName)`: Calculate pillar score from all evidence scores
- `calculatePillarScore(evidenceScores, aggregationMethod)`: Apply aggregation algorithm
- `getAggregationMethod()`: Return configured method (average, weighted average, max)
- `aggregateAllPillars(employeeId)`: Calculate scores for all 9 pillars
- `getEvidenceContributions(employeeId, pillarName)`: Show which evidence contributed to pillar score
- `combineMultiRoleScores(employeeScores, managerScores, assessorScores)`: Merge scores from different roles

#### 7. Performance Evaluation Module

**PerformanceEvaluationService**
- `evaluatePerformanceStatus(employeeId, pillarScores)`: Compare scores to expected scores
- `getExpectedScores(employeeGrade)`: Retrieve expected scores for grade
- `calculatePerformanceStatus(pillarScore, expectedScore)`: Determine under/at/over performing
- `identifyUnderPerformingPillars(employeeId, pillarScores)`: List pillars needing development
- `generatePerformanceReport(employeeId, ecdfVersionId)`: Create comprehensive performance analysis

#### 8. Development Plan Module

**DevelopmentPlanController**
- `POST /api/development-plans`: Create personal development plan
- `GET /api/development-plans`: Get active development plans for authenticated employee
- `GET /api/development-plans/{planId}`: Get specific development plan
- `PUT /api/development-plans/{planId}`: Update development plan
- `POST /api/development-plans/{planId}/goals`: Add development goal
- `PUT /api/development-plans/goals/{goalId}`: Update goal status
- `GET /api/development-plans/history`: Get historical plans and outcomes

**DevelopmentPlanService**
- `createDevelopmentPlan(employeeId, underPerformingPillars)`: Create plan for identified gaps
- `addDevelopmentGoal(planId, pillarName, goalData)`: Add goal to plan
- `updateGoalStatus(goalId, status)`: Mark goal as in-progress/completed
- `trackProgress(employeeId, pillarName)`: Compare scores across ECDF versions for pillar
- `linkPlanToAssessment(planId, ecdfVersionId)`: Associate plan with triggering assessment
- `evaluatePlanOutcome(planId, newEcdfVersionId)`: Check if pillar scores improved

#### 9. Progress Module

**ProgressController**
- `GET /api/progress`: Retrieve progress report for authenticated employee
- `GET /api/employees/{employeeId}/progress`: Get employee progress (Manager/Assessor access)
- `GET /api/progress/pillars/{pillarName}`: Retrieve history for specific pillar
- `GET /api/progress/trends`: Get trend analysis across all pillars
- `GET /api/progress/evidence-growth`: Track evidence accumulation over time

**ProgressService**
- `generateProgressReport(employeeId)`: Calculate trends across ECDF versions
- `getPillarHistory(employeeId, pillarName)`: Get score history for one pillar
- `calculateTrends(ecdfVersions)`: Compute growth metrics
- `getEvidenceGrowth(employeeId)`: Track evidence item count over time
- `compareVersions(versionId1, versionId2)`: Generate detailed comparison

#### 10. Manager Module

**ManagerController**
- `GET /api/manager/team`: Get list of assigned employees
- `GET /api/manager/team/overview`: Get team-level statistics and analytics
- `GET /api/manager/employees/{employeeId}/evidence/pending`: Get evidence needing manager assessment
- `GET /api/manager/employees/{employeeId}/progress`: Get individual employee progress
- `GET /api/manager/team/performance`: Get team performance distribution
- `GET /api/manager/team/trends`: Get team-level competency trends
- `POST /api/manager/team/reports/export`: Export team progress report

**ManagerService**
- `getAssignedEmployees(managerId)`: Retrieve list of managed employees
- `getTeamOverview(managerId)`: Calculate team-level statistics
- `getPendingEvidenceForEmployee(managerId, employeeId)`: Get evidence needing manager scores
- `getEmployeeProgress(managerId, employeeId)`: Retrieve individual progress with access control
- `getTeamPerformanceDistribution(managerId)`: Calculate performance status across team
- `getTeamCompetencyTrends(managerId)`: Analyze team pillar score trends
- `identifyHighPerformers(managerId)`: Find over-performing team members
- `identifyEmployeesNeedingSupport(managerId)`: Find under-performing team members
- `exportTeamReport(managerId, format)`: Generate exportable report

#### 11. Independent Assessor Module

**IndependentAssessorController**
- `GET /api/assessor/assignments`: Get list of assigned employees
- `GET /api/assessor/employees/{employeeId}/evidence/pending`: Get evidence needing assessor evaluation
- `GET /api/assessor/employees/{employeeId}/progress`: Get employee progress (read-only)

**IndependentAssessorService**
- `getAssignedEmployees(assessorId)`: Retrieve list of assigned employees
- `getPendingEvidenceForEmployee(assessorId, employeeId)`: Get evidence needing assessor scores
- `getEmployeeProgress(assessorId, employeeId)`: Retrieve progress with access control

### Frontend Components

#### 1. Authentication Components
- `LoginPage`: Login form with credential submission
- `AuthContext`: Global authentication state with role management
- `ProtectedRoute`: Route guard for authenticated pages with role-based access
- `RoleBasedRedirect`: Redirect users to appropriate dashboard based on role

#### 2. Employee Components
- `EmployeeProfile`: Display employee record with grade information
- `EmployeeDashboard`: Main dashboard for employees with evidence, assessments, and development plans

#### 3. Evidence Components (Employee)
- `EvidenceList`: Display all evidence items for employee
- `EvidenceForm`: Create/edit evidence with Impact, Complexity, Personal_Contribution
- `EvidenceCard`: Display single evidence item with quality factors
- `EvidenceDetailView`: Show evidence with all assessor scores (after all complete)

#### 4. Evidence Scoring Components (All Roles)
- `EvidenceScoreForm`: Score evidence for selected pillars (role-specific)
- `PillarSelector`: Select which pillars evidence demonstrates
- `ScoreInput`: Input score 1-5 for each pillar
- `PillarDescription`: Show pillar details and scoring guidance
- `ScoreHistoryView`: Display evidence score versions over time
- `MultiAssessorComparison`: Compare scores across Employee/Manager/Assessor

#### 5. ECDF Assessment Components (Employee)
- `ECDFAssessmentForm`: Create new ECDF assessment version
- `ECDFVersionList`: Display all ECDF versions chronologically
- `ECDFVersionDetail`: Show complete assessment with all evidence and scores
- `ECDFComparison`: Side-by-side comparison of multiple ECDF versions
- `PillarScoreDisplay`: Show aggregated pillar score with contributing evidence
- `DraftSaveButton`: Save ECDF assessment progress without submission

#### 6. Performance Status Components (Employee)
- `PerformanceStatusCard`: Display performance status for each pillar
- `ExpectedScoreComparison`: Show actual vs expected scores
- `PerformanceIndicator`: Visual indicator (color/icon) for under/at/over performing

#### 7. Development Plan Components (Employee)
- `DevelopmentPlanForm`: Create personal development plan
- `DevelopmentGoalCard`: Display individual development goal
- `GoalProgressTracker`: Track goal status and completion
- `PlanOutcomeView`: Show pillar score improvements linked to plan

#### 8. Progress Components (Employee)
- `ProgressDashboard`: Overview of career development over time
- `PillarRadarChart`: Radar chart showing all 9 pillars for selected ECDF version
- `PillarTrendChart`: Line chart for individual pillar across ECDF versions
- `ECDFHistoryTimeline`: Timeline view of all ECDF assessments
- `EvidenceGrowthChart`: Visualize evidence accumulation over time

#### 9. Manager Components
- `ManagerDashboard`: Main dashboard with team overview
- `TeamMemberList`: List of assigned employees with assessment status
- `EmployeeEvidenceReview`: View and score employee evidence
- `EmployeeProgressView`: View individual employee progress over time
- `TeamOverviewDashboard`: Team-level statistics and analytics
- `TeamPerformanceHeatmap`: Visualize team competency distribution
- `TeamTrendAnalysis`: Track team pillar score trends
- `TeamReportExport`: Export team progress reports

#### 10. Independent Assessor Components
- `AssessorDashboard`: Main dashboard with assigned employees
- `AssignedEmployeeList`: List of employees requiring assessment
- `EmployeeEvidenceAssessment`: View and score employee evidence
- `AssessmentProgressTracker`: Track completion status of assessments

### API Contracts

#### Authentication

**POST /api/auth/login**
```json
Request:
{
  "username": "string",
  "password": "string"
}

Response (200):
{
  "sessionToken": "string",
  "userId": "long",
  "role": "EMPLOYEE | MANAGER | INDEPENDENT_ASSESSOR",
  "employeeId": "long (if role is EMPLOYEE)",
  "expiresAt": "ISO8601 timestamp"
}

Response (401):
{
  "error": "Invalid credentials"
}
```

#### Employee Profile

**GET /api/employees/profile**
```json
Response (200):
{
  "id": "long",
  "name": "string",
  "email": "string",
  "jobTitle": "string",
  "department": "string",
  "grade": "JUNIOR | MID_LEVEL | SENIOR | PRINCIPAL",
  "hireDate": "ISO8601 date"
}
```

#### Evidence

**POST /api/evidence**
```json
Request:
{
  "description": "string",
  "impact": "string",
  "complexity": "string",
  "personalContribution": "string"
}

Response (201):
{
  "id": "long",
  "employeeId": "long",
  "description": "string",
  "impact": "string",
  "complexity": "string",
  "personalContribution": "string",
  "createdAt": "ISO8601 timestamp"
}
```

**GET /api/employees/{employeeId}/evidence**
```json
Response (200):
[
  {
    "id": "long",
    "employeeId": "long",
    "description": "string",
    "impact": "string",
    "complexity": "string",
    "personalContribution": "string",
    "createdAt": "ISO8601 timestamp",
    "hasEmployeeScore": "boolean",
    "hasManagerScore": "boolean",
    "hasAssessorScore": "boolean"
  }
]
```

#### Evidence Scores

**POST /api/evidence/{evidenceId}/scores**
```json
Request:
{
  "scores": [
    {
      "pillarName": "THINKS",
      "score": 3
    },
    {
      "pillarName": "DESIGNS",
      "score": 4
    }
  ]
}

Response (201):
{
  "evidenceId": "long",
  "assessorId": "long",
  "assessorType": "EMPLOYEE | MANAGER | INDEPENDENT_ASSESSOR",
  "versionId": "string",
  "timestamp": "ISO8601 timestamp",
  "scores": [
    {
      "id": "long",
      "pillarName": "THINKS",
      "score": 3
    }
  ]
}
```

**GET /api/evidence/{evidenceId}/scores**
```json
Response (200):
{
  "evidenceId": "long",
  "scoresByAssessorType": {
    "EMPLOYEE": [
      {
        "id": "long",
        "pillarName": "THINKS",
        "score": 3,
        "versionId": "string",
        "timestamp": "ISO8601 timestamp"
      }
    ],
    "MANAGER": [ ... ],
    "INDEPENDENT_ASSESSOR": [ ... ]
  }
}
```

**GET /api/evidence/{evidenceId}/scores/history**
```json
Response (200):
{
  "evidenceId": "long",
  "pillarName": "THINKS",
  "versions": [
    {
      "versionId": "string",
      "assessorType": "EMPLOYEE",
      "assessorId": "long",
      "score": 3,
      "timestamp": "ISO8601 timestamp"
    },
    {
      "versionId": "string",
      "assessorType": "EMPLOYEE",
      "assessorId": "long",
      "score": 4,
      "timestamp": "ISO8601 timestamp"
    }
  ]
}
```

#### ECDF Assessment

**POST /api/assessments/ecdf**
```json
Request:
{
  "isDraft": "boolean"
}

Response (201):
{
  "id": "long",
  "employeeId": "long",
  "versionId": "string",
  "versionNumber": "int",
  "isDraft": "boolean",
  "createdAt": "ISO8601 timestamp",
  "submittedAt": "ISO8601 timestamp or null",
  "pillarScores": {
    "THINKS": 3.5,
    "ENGAGES": 4.0,
    ...
  },
  "performanceStatus": {
    "THINKS": "UNDER_PERFORMING | AT_EXPECTATION | OVER_PERFORMING",
    ...
  },
  "evidenceCount": "int"
}
```

**GET /api/assessments/ecdf**
```json
Response (200):
[
  {
    "id": "long",
    "versionId": "string",
    "versionNumber": "int",
    "createdAt": "ISO8601 timestamp",
    "submittedAt": "ISO8601 timestamp",
    "pillarScores": { ... },
    "performanceStatus": { ... },
    "evidenceCount": "int"
  }
]
```

**GET /api/assessments/ecdf/{versionId}**
```json
Response (200):
{
  "id": "long",
  "employeeId": "long",
  "versionId": "string",
  "versionNumber": "int",
  "createdAt": "ISO8601 timestamp",
  "submittedAt": "ISO8601 timestamp",
  "pillarScores": {
    "THINKS": 3.5,
    ...
  },
  "performanceStatus": {
    "THINKS": "UNDER_PERFORMING",
    ...
  },
  "expectedScores": {
    "THINKS": 4.0,
    ...
  },
  "evidenceContributions": {
    "THINKS": [
      {
        "evidenceId": "long",
        "description": "string",
        "aggregatedScore": 3.5,
        "scoresByAssessor": {
          "EMPLOYEE": 3,
          "MANAGER": 4,
          "INDEPENDENT_ASSESSOR": 3
        }
      }
    ],
    ...
  }
}
```

**GET /api/assessments/ecdf/compare?versionIds=1,2,3**
```json
Response (200):
{
  "employeeId": "long",
  "versions": [
    {
      "versionId": "string",
      "versionNumber": "int",
      "date": "ISO8601 timestamp",
      "pillarScores": { ... },
      "evidenceCount": "int"
    }
  ],
  "pillarTrends": {
    "THINKS": [
      {"versionNumber": 1, "score": 3.0},
      {"versionNumber": 2, "score": 3.5},
      {"versionNumber": 3, "score": 4.0}
    ],
    ...
  },
  "improvements": ["THINKS", "DESIGNS"],
  "declines": [],
  "stable": ["ENGAGES", ...]
}
```

#### Development Plans

**POST /api/development-plans**
```json
Request:
{
  "ecdfVersionId": "long",
  "underPerformingPillars": ["THINKS", "DESIGNS"],
  "goals": [
    {
      "pillarName": "THINKS",
      "description": "string",
      "targetDate": "ISO8601 date",
      "plannedActions": "string"
    }
  ]
}

Response (201):
{
  "id": "long",
  "employeeId": "long",
  "ecdfVersionId": "long",
  "createdAt": "ISO8601 timestamp",
  "goals": [
    {
      "id": "long",
      "pillarName": "THINKS",
      "description": "string",
      "targetDate": "ISO8601 date",
      "plannedActions": "string",
      "status": "NOT_STARTED | IN_PROGRESS | COMPLETED",
      "currentScore": 3.0,
      "expectedScore": 4.0
    }
  ]
}
```

#### Progress

**GET /api/progress**
```json
Response (200):
{
  "employeeId": "long",
  "ecdfVersionCount": "int",
  "dateRange": {
    "first": "ISO8601 timestamp",
    "latest": "ISO8601 timestamp"
  },
  "pillarTrends": {
    "THINKS": [
      {"date": "ISO8601", "score": 3.0, "versionNumber": 1},
      {"date": "ISO8601", "score": 3.5, "versionNumber": 2}
    ],
    ...
  },
  "latestScores": {
    "THINKS": 4.0,
    ...
  },
  "evidenceGrowth": [
    {"date": "ISO8601", "count": 5},
    {"date": "ISO8601", "count": 12}
  ]
}
```

#### Manager Endpoints

**GET /api/manager/team**
```json
Response (200):
[
  {
    "employeeId": "long",
    "name": "string",
    "jobTitle": "string",
    "grade": "SENIOR",
    "pendingEvidenceCount": "int",
    "latestEcdfDate": "ISO8601 timestamp",
    "ecdfVersionCount": "int"
  }
]
```

**GET /api/manager/team/overview**
```json
Response (200):
{
  "managerId": "long",
  "teamSize": "int",
  "averagePillarScores": {
    "THINKS": 3.8,
    ...
  },
  "performanceDistribution": {
    "THINKS": {
      "UNDER_PERFORMING": 2,
      "AT_EXPECTATION": 5,
      "OVER_PERFORMING": 3
    },
    ...
  },
  "recentImprovements": [
    {
      "employeeId": "long",
      "name": "string",
      "pillar": "THINKS",
      "improvement": 1.5
    }
  ],
  "needsAttention": [
    {
      "employeeId": "long",
      "name": "string",
      "reason": "No recent assessment | Declining scores"
    }
  ]
}
```

**GET /api/manager/employees/{employeeId}/evidence/pending**
```json
Response (200):
[
  {
    "evidenceId": "long",
    "description": "string",
    "createdAt": "ISO8601 timestamp",
    "hasManagerScore": "boolean"
  }
]
```

## Data Models

### Entity Relationship Diagram

```
┌─────────────────┐
│      User       │
├─────────────────┤
│ id (PK)         │
│ username        │
│ passwordHash    │
│ email           │
│ role (ENUM)     │
│ createdAt       │
└────────┬────────┘
         │
         │ 1:1 (if role=EMPLOYEE)
         │
┌────────┴────────┐
│    Employee     │
├─────────────────┤
│ id (PK)         │
│ userId (FK)     │
│ name            │
│ jobTitle        │
│ department      │
│ grade (ENUM)    │
│ managerId (FK)  │
│ hireDate        │
└────────┬────────┘
         │
         │ 1:N
         │
┌────────┴────────┐
│    Evidence     │
├─────────────────┤
│ id (PK)         │
│ employeeId (FK) │
│ description     │
│ impact          │
│ complexity      │
│ contribution    │
│ createdAt       │
└────────┬────────┘
         │
         │ 1:N
         │
┌────────┴──────────────┐
│   EvidenceScore       │
├───────────────────────┤
│ id (PK)               │
│ evidenceId (FK)       │
│ pillarName (ENUM)     │
│ score (1-5)           │
│ assessorId (FK)       │
│ assessorType (ENUM)   │
│ versionId (UUID)      │
│ timestamp             │
└───────────────────────┘

┌─────────────────┐
│ ECDFAssessment  │
├─────────────────┤
│ id (PK)         │
│ employeeId (FK) │
│ versionId (UUID)│
│ versionNumber   │
│ isDraft         │
│ createdAt       │
│ submittedAt     │
└────────┬────────┘
         │
         │ 1:N
         │
┌────────┴────────────┐
│   PillarScore       │
├─────────────────────┤
│ id (PK)             │
│ assessmentId (FK)   │
│ pillarName (ENUM)   │
│ aggregatedScore     │
└─────────────────────┘

┌─────────────────────┐
│   ExpectedScore     │
├─────────────────────┤
│ id (PK)             │
│ grade (ENUM)        │
│ pillarName (ENUM)   │
│ expectedScore       │
└─────────────────────┘

┌─────────────────────┐
│  DevelopmentPlan    │
├─────────────────────┤
│ id (PK)             │
│ employeeId (FK)     │
│ ecdfVersionId (FK)  │
│ createdAt           │
└────────┬────────────┘
         │
         │ 1:N
         │
┌────────┴────────────┐
│  DevelopmentGoal    │
├─────────────────────┤
│ id (PK)             │
│ planId (FK)         │
│ pillarName (ENUM)   │
│ description         │
│ targetDate          │
│ plannedActions      │
│ status (ENUM)       │
└─────────────────────┘

┌─────────────────────┐
│ ManagerAssignment   │
├─────────────────────┤
│ id (PK)             │
│ managerId (FK)      │
│ employeeId (FK)     │
│ assignedAt          │
└─────────────────────┘

┌─────────────────────┐
│ AssessorAssignment  │
├─────────────────────┤
│ id (PK)             │
│ assessorId (FK)     │
│ employeeId (FK)     │
│ assignedAt          │
└─────────────────────┘
```

### JPA Entity Definitions

#### User Entity
```java
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(unique = true, nullable = false)
    private String username;
    
    @Column(nullable = false)
    private String passwordHash;
    
    @Column(unique = true, nullable = false)
    private String email;
    
    @Enumerated(EnumType.STRING)
    @Column(nullable = false)
    private UserRole role;
    
    @Column(nullable = false)
    private LocalDateTime createdAt;
    
    @OneToOne(mappedBy = "user", cascade = CascadeType.ALL)
    private Employee employee;
}
```

#### Employee Entity
```java
@Entity
@Table(name = "employees")
public class Employee {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @OneToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "user_id", nullable = false, unique = true)
    private User user;
    
    @Column(nullable = false)
    private String name;
    
    @Column(nullable = false)
    private String jobTitle;
    
    @Column(nullable = false)
    private String department;
    
    @Enumerated(EnumType.STRING)
    @Column(nullable = false)
    private EmployeeGrade grade;
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "manager_id")
    private User manager;
    
    @Column(nullable = false)
    private LocalDate hireDate;
    
    @OneToMany(mappedBy = "employee", cascade = CascadeType.ALL)
    private List<Evidence> evidenceList;
    
    @OneToMany(mappedBy = "employee", cascade = CascadeType.ALL)
    private List<ECDFAssessment> ecdfAssessments;
    
    @OneToMany(mappedBy = "employee", cascade = CascadeType.ALL)
    private List<DevelopmentPlan> developmentPlans;
}
```

#### Evidence Entity
```java
@Entity
@Table(name = "evidence")
public class Evidence {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "employee_id", nullable = false)
    private Employee employee;
    
    @Column(columnDefinition = "TEXT", nullable = false)
    private String description;
    
    @Column(columnDefinition = "TEXT", nullable = false)
    private String impact;
    
    @Column(columnDefinition = "TEXT", nullable = false)
    private String complexity;
    
    @Column(name = "personal_contribution", columnDefinition = "TEXT", nullable = false)
    private String personalContribution;
    
    @Column(nullable = false)
    private LocalDateTime createdAt;
    
    @OneToMany(mappedBy = "evidence", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<EvidenceScore> evidenceScores;
}
```

#### EvidenceScore Entity
```java
@Entity
@Table(name = "evidence_scores",
       indexes = {
           @Index(name = "idx_evidence_assessor", columnList = "evidence_id,assessor_id,assessor_type"),
           @Index(name = "idx_version", columnList = "version_id")
       })
public class EvidenceScore {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "evidence_id", nullable = false)
    private Evidence evidence;
    
    @Enumerated(EnumType.STRING)
    @Column(name = "pillar_name", nullable = false)
    private PillarName pillarName;
    
    @Column(nullable = false)
    @Min(1)
    @Max(5)
    private Integer score;
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "assessor_id", nullable = false)
    private User assessor;
    
    @Enumerated(EnumType.STRING)
    @Column(name = "assessor_type", nullable = false)
    private AssessorType assessorType;
    
    @Column(name = "version_id", nullable = false)
    private String versionId; // UUID
    
    @Column(nullable = false)
    private LocalDateTime timestamp;
}
```

#### ECDFAssessment Entity
```java
@Entity
@Table(name = "ecdf_assessments",
       indexes = {
           @Index(name = "idx_employee_version", columnList = "employee_id,version_number"),
           @Index(name = "idx_version_id", columnList = "version_id", unique = true)
       })
public class ECDFAssessment {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "employee_id", nullable = false)
    private Employee employee;
    
    @Column(name = "version_id", nullable = false, unique = true)
    private String versionId; // UUID
    
    @Column(name = "version_number", nullable = false)
    private Integer versionNumber;
    
    @Column(name = "is_draft", nullable = false)
    private Boolean isDraft;
    
    @Column(nullable = false)
    private LocalDateTime createdAt;
    
    @Column
    private LocalDateTime submittedAt;
    
    @OneToMany(mappedBy = "assessment", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<PillarScore> pillarScores;
}
```

#### PillarScore Entity
```java
@Entity
@Table(name = "pillar_scores",
       uniqueConstraints = @UniqueConstraint(columnNames = {"assessment_id", "pillar_name"}))
public class PillarScore {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "assessment_id", nullable = false)
    private ECDFAssessment assessment;
    
    @Enumerated(EnumType.STRING)
    @Column(name = "pillar_name", nullable = false)
    private PillarName pillarName;
    
    @Column(name = "aggregated_score", nullable = false)
    private Double aggregatedScore;
    
    @Enumerated(EnumType.STRING)
    @Column(name = "performance_status")
    private PerformanceStatus performanceStatus;
}
```

#### ExpectedScore Entity
```java
@Entity
@Table(name = "expected_scores",
       uniqueConstraints = @UniqueConstraint(columnNames = {"grade", "pillar_name"}))
public class ExpectedScore {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Enumerated(EnumType.STRING)
    @Column(nullable = false)
    private EmployeeGrade grade;
    
    @Enumerated(EnumType.STRING)
    @Column(name = "pillar_name", nullable = false)
    private PillarName pillarName;
    
    @Column(name = "expected_score", nullable = false)
    @Min(1)
    @Max(5)
    private Double expectedScore;
}
```

#### DevelopmentPlan Entity
```java
@Entity
@Table(name = "development_plans")
public class DevelopmentPlan {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "employee_id", nullable = false)
    private Employee employee;
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "ecdf_version_id", nullable = false)
    private ECDFAssessment triggeringAssessment;
    
    @Column(nullable = false)
    private LocalDateTime createdAt;
    
    @OneToMany(mappedBy = "plan", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<DevelopmentGoal> goals;
}
```

#### DevelopmentGoal Entity
```java
@Entity
@Table(name = "development_goals")
public class DevelopmentGoal {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "plan_id", nullable = false)
    private DevelopmentPlan plan;
    
    @Enumerated(EnumType.STRING)
    @Column(name = "pillar_name", nullable = false)
    private PillarName pillarName;
    
    @Column(columnDefinition = "TEXT", nullable = false)
    private String description;
    
    @Column(name = "target_date", nullable = false)
    private LocalDate targetDate;
    
    @Column(name = "planned_actions", columnDefinition = "TEXT", nullable = false)
    private String plannedActions;
    
    @Enumerated(EnumType.STRING)
    @Column(nullable = false)
    private GoalStatus status;
}
```

#### Enum Definitions

```java
public enum UserRole {
    EMPLOYEE,
    MANAGER,
    INDEPENDENT_ASSESSOR
}

public enum EmployeeGrade {
    JUNIOR,
    MID_LEVEL,
    SENIOR,
    PRINCIPAL
}

public enum PillarName {
    THINKS,
    ENGAGES,
    INFLUENCES,
    ACHIEVES,
    DEFINES,
    DESIGNS,
    DELIVERS,
    CONTROLS,
    OPERATES
}

public enum AssessorType {
    EMPLOYEE,
    MANAGER,
    INDEPENDENT_ASSESSOR
}

public enum PerformanceStatus {
    UNDER_PERFORMING,
    AT_EXPECTATION,
    OVER_PERFORMING
}

public enum GoalStatus {
    NOT_STARTED,
    IN_PROGRESS,
    COMPLETED
}
```

### Database Schema Considerations

**Indexing Strategy:**
- Index on `employee_id` in Evidence, ECDFAssessment, DevelopmentPlan tables for fast employee-specific queries
- Composite index on `(evidence_id, assessor_id, assessor_type)` in EvidenceScore for multi-role score retrieval
- Index on `version_id` in EvidenceScore and ECDFAssessment for version lookups
- Composite index on `(employee_id, version_number)` in ECDFAssessment for chronological queries
- Index on `timestamp` in EvidenceScore for temporal queries
- Composite index on `(grade, pillar_name)` in ExpectedScore for performance evaluation
- Index on `assessor_id` in EvidenceScore for manager/assessor workload queries

**Data Encryption:**
- Encrypt sensitive text fields (description, impact, complexity, personalContribution) at application layer before persistence
- Use AES-256 encryption with employee-specific keys derived from master key
- Store encryption metadata separately for key rotation support
- Encrypt development plan goals and actions

**Constraints:**
- Unique constraint on `(assessment_id, pillar_name)` in PillarScore to prevent duplicate pillar scores
- Unique constraint on `(grade, pillar_name)` in ExpectedScore to ensure one expected score per grade-pillar combination
- Check constraint on score fields: `score >= 1 AND score <= 5`
- Check constraint on aggregatedScore: `aggregated_score >= 1.0 AND aggregated_score <= 5.0`
- Foreign key constraints with CASCADE DELETE for assessment-related entities
- Foreign key constraints with RESTRICT for User references to prevent accidental deletion

**Versioning Strategy:**
- EvidenceScore uses UUID `versionId` to track point-in-time snapshots
- Each score update creates a new row with new `versionId` and `timestamp`
- ECDFAssessment uses UUID `versionId` and sequential `versionNumber` for tracking
- Historical versions are immutable (no updates, only inserts)
- Soft delete pattern for Evidence (mark as deleted rather than removing) to preserve historical references

**Performance Considerations:**
- Partition EvidenceScore table by timestamp for large datasets
- Use database views for common aggregation queries (e.g., latest scores by assessor type)
- Implement caching layer (Redis) for frequently accessed expected scores
- Use read replicas for manager dashboard queries to reduce load on primary database



## Correctness Properties

A property is a characteristic or behavior that should hold true across all valid executions of a system-essentially, a formal statement about what the system should do. Properties serve as the bridge between human-readable specifications and machine-verifiable correctness guarantees.

### Property 1: Valid Credentials Create Authenticated Session

For any valid username and password combination, authenticating with those credentials should create an active session with the correct user role assigned.

**Validates: Requirements 1.1**

### Property 2: Invalid Credentials Are Rejected

For any invalid username or password (non-existent user, wrong password, malformed input), authentication attempts should be rejected and no session should be created.

**Validates: Requirements 1.2**

### Property 3: Active Sessions Maintain State

For any active session, querying the session should return the authenticated user's identity and role without requiring re-authentication.

**Validates: Requirements 1.3**

### Property 4: Terminated Sessions Require Re-authentication

For any session that has been explicitly logged out or has expired, attempts to access protected resources should be rejected and require re-authentication.

**Validates: Requirements 1.4**

### Property 5: Evidence Score Range Validation

For any evidence score submission, the score value for each pillar must be an integer between 1 and 5 inclusive, and submissions with scores outside this range should be rejected.

**Validates: Requirements 3.3, 5.4**

### Property 6: Evidence Quality Factor Completeness

For any evidence submission, the evidence must include non-empty values for all three quality factors (Impact, Complexity, Personal_Contribution), and submissions missing any factor should be rejected.

**Validates: Requirements 4.5**

### Property 7: Evidence Score Ownership Protection

For any evidence score, only the assessor who created that score can modify it, and attempts by other users to modify the score should be rejected.

**Validates: Requirements 8.6, 18.4**

### Property 8: Pillar Score Aggregation Consistency

For any set of evidence scores for a pillar, applying the configured aggregation method (average, weighted average, or maximum) should produce a consistent result when given the same input scores.

**Validates: Requirements 9.3**

### Property 9: Aggregated Score Range Invariant

For any aggregated pillar score calculated from evidence scores, the resulting score must be between 1.0 and 5.0 inclusive.

**Validates: Requirements 9.4**

### Property 10: Performance Status Calculation

For any pillar score and expected score combination, the performance status should be calculated as: under-performing when pillar score < expected score, at-expectation when pillar score = expected score, and over-performing when pillar score > expected score.

**Validates: Requirements 21.2, 21.3, 21.4, 21.5**

### Property 11: Evidence Score Versioning Creates New Records

For any evidence score update for the same evidence item and pillar by the same assessor, a new version should be created with a new version ID and timestamp, and the previous version should remain unchanged in the data store.

**Validates: Requirements 24.2, 24.3**

### Property 12: ECDF Assessment Versioning Preserves History

For any employee creating multiple ECDF assessments over time, each new assessment should create a new version with a unique version ID and incremented version number, and previous assessment versions should remain unchanged and retrievable.

**Validates: Requirements 25.2, 25.4**

### Property 13: Employee Record Access Control

For any employee record access request, the system should only return the employee record if the requestor is the employee themselves, their assigned manager, or an assigned independent assessor.

**Validates: Requirements 2.4**

### Property 14: Evidence Employee Association

For any evidence item created by an employee, the evidence should be stored with a reference to that employee's ID, and retrieving evidence for that employee should include the created evidence item.

**Validates: Requirements 4.6**

### Property 15: Assessment Persistence Completeness

For any submitted ECDF assessment, the persisted data should include all evidence scores from all assessor types and all aggregated pillar scores, and retrieving the assessment should return complete data.

**Validates: Requirements 12.1**

### Property 16: Role-Based Evidence Access Control

For any evidence access request, employees should only access their own evidence, managers should only access evidence for their assigned employees, and independent assessors should only access evidence for their assigned employees.

**Validates: Requirements 17.2**

### Property 17: Assessment Independence Protection

For any evidence item being assessed by multiple assessor types, managers and independent assessors should not be able to view each other's scores until they have completed their own assessment for that evidence item.

**Validates: Requirements 17.4**

### Property 18: Expected Score Coverage

For any employee grade and pillar combination, there should exist exactly one expected score value in the system.

**Validates: Requirements 20.1**

### Property 19: Development Plan Employee Association

For any personal development plan created for an employee, the plan should be stored with a reference to that employee's ID and the triggering ECDF assessment version, and retrieving plans for that employee should include the created plan.

**Validates: Requirements 22.5**

### Property 20: Evidence Addition Independence

For any employee adding new evidence items after creating an ECDF assessment, the new evidence should not modify or appear in the previous ECDF assessment version, and the previous assessment should remain unchanged.

**Validates: Requirements 26.1**

### Property 21: ECDF Version Evidence Referential Integrity

For any ECDF assessment version, the evidence items referenced in the assessment should be exactly the evidence items that existed for that employee at the time the assessment was created, and retrieving the assessment should return references to those specific evidence items.

**Validates: Requirements 26.7**


## Error Handling

### Error Categories

**1. Authentication Errors**
- Invalid credentials: Return 401 Unauthorized with clear error message
- Expired session: Return 401 Unauthorized and require re-authentication
- Missing authentication token: Return 401 Unauthorized

**2. Authorization Errors**
- Insufficient permissions: Return 403 Forbidden with explanation of required role
- Access to other employee's data: Return 403 Forbidden
- Attempt to modify others' scores: Return 403 Forbidden

**3. Validation Errors**
- Invalid evidence score (not 1-5): Return 400 Bad Request with validation details
- Missing quality factors: Return 400 Bad Request listing missing fields
- Invalid pillar name: Return 400 Bad Request with valid pillar list
- Incomplete ECDF assessment: Return 400 Bad Request indicating missing pillars

**4. Business Logic Errors**
- Viewing others' scores before completing own: Return 409 Conflict with explanation
- Attempting to modify immutable version: Return 409 Conflict
- Creating evidence for another employee: Return 403 Forbidden

**5. Data Persistence Errors**
- Database connection failure: Return 503 Service Unavailable and retry
- Constraint violation: Return 409 Conflict with details
- Transaction timeout: Return 504 Gateway Timeout and allow retry

**6. Not Found Errors**
- Non-existent evidence ID: Return 404 Not Found
- Non-existent assessment version: Return 404 Not Found
- Non-existent employee: Return 404 Not Found

### Error Response Format

All error responses follow consistent JSON structure:

```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "Human-readable error message",
    "details": {
      "field": "specific field with error",
      "reason": "detailed explanation"
    },
    "timestamp": "ISO8601 timestamp"
  }
}
```

### Error Handling Strategy

**Backend:**
- Use Spring's `@ControllerAdvice` for global exception handling
- Create custom exception classes for domain-specific errors
- Log all errors with appropriate severity levels
- Never expose internal implementation details in error messages
- Sanitize error messages to prevent information leakage

**Frontend:**
- Display user-friendly error messages
- Provide actionable guidance for resolving errors
- Implement retry logic for transient failures
- Show validation errors inline on forms
- Log errors to monitoring service for debugging

## Testing Strategy

### Test-Driven Development (TDD) Approach

The system will be developed using Test-Driven Development with a focus on class behavior testing:

**TDD Workflow:**
1. Write test for class behavior first
2. Run test and watch it fail
3. Write minimal code to make test pass
4. Refactor while keeping tests green
5. Repeat for next behavior

**Testing Focus:**
- Test class behavior and interactions
- Verify business logic correctness
- Test integration points between components
- Validate error conditions and exception handling
- Test specific examples and edge cases
- Test database queries and data access logic

**Note on Property-Based Testing:**
While the design includes correctness properties for documentation purposes, the implementation will focus on traditional unit testing and integration testing rather than property-based testing frameworks. The properties serve as a guide for what behaviors to test.

### Testing Framework Selection

**Backend (Java/Spring Boot):**
- JUnit 5 for unit testing
- Mockito for mocking dependencies
- Spring Boot Test for integration testing
- H2 in-memory database for repository tests
- TestContainers for PostgreSQL integration tests (optional)

**Frontend (React):**
- Jest for unit testing
- React Testing Library for component testing
- MSW (Mock Service Worker) for API mocking

### Test Organization

**Backend Test Structure:**
```
src/test/java/
  com/careerdev/
    unit/
      service/          # Service layer unit tests
      repository/       # Repository tests with H2
      util/            # Utility class tests
    integration/
      controller/       # API endpoint integration tests
      security/        # Authentication/authorization tests
```

**Frontend Test Structure:**
```
src/
  components/
    __tests__/        # Component unit tests
  services/
    __tests__/        # Service layer tests
  hooks/
    __tests__/        # Custom hooks tests
```

### Key Test Scenarios

**Authentication & Authorization:**
- Valid credentials create authenticated session
- Invalid credentials are rejected
- Active sessions maintain state
- Terminated sessions require re-authentication
- Role-based access control for all endpoints
- Evidence access restricted by role

**Evidence Management:**
- Evidence creation with all quality factors
- Evidence creation fails without quality factors
- Evidence associated with correct employee
- Evidence retrieval filtered by role
- Evidence update only by owner
- Evidence deletion only by owner

**Evidence Scoring:**
- Score range validation (1-5)
- Score creation by different assessor types
- Score ownership protection
- Score versioning creates new records
- Assessment independence (assessors can't see each other's scores)
- Score history retrieval

**ECDF Assessment:**
- Assessment creation with all pillars
- Assessment versioning preserves history
- Pillar score aggregation from evidence scores
- Aggregated score range invariant (1.0-5.0)
- Draft saving and restoration
- Assessment submission validation

**Performance Evaluation:**
- Performance status calculation (under/at/over)
- Expected score retrieval by grade
- Performance status for all pillars
- Identification of under-performing pillars

**Development Plans:**
- Plan creation for under-performing pillars
- Goal association with pillars
- Goal status tracking
- Plan linkage to triggering assessment
- Progress tracking across assessments

**Manager Features:**
- Team member list retrieval
- Pending evidence identification
- Individual employee progress access
- Team overview statistics
- Team performance distribution
- Team trend analysis

**Versioning & History:**
- Evidence score versioning
- ECDF assessment versioning
- Historical comparison
- Evidence addition independence
- Referential integrity across versions

### Test Data Management

**Test Data Builders:**
- Create builder classes for domain objects (UserBuilder, EmployeeBuilder, EvidenceBuilder, etc.)
- Use builders to create test data with sensible defaults
- Allow overriding specific fields for test scenarios
- Implement fluent API for readability

**Database Test Data:**
- Use Flyway or Liquibase for test database migrations
- Create seed data scripts for integration tests
- Reset database state between tests using @Transactional with rollback
- Use @BeforeEach to set up common test data
- Use @AfterEach to clean up when needed

### Continuous Integration

- Run all unit tests on every commit
- Run integration tests on pull request merge
- Maintain minimum 80% code coverage for services and controllers
- Fail build on any test failure
- Generate test reports and coverage reports
- Run tests in parallel where possible

### Test Maintenance

- Review and update tests when requirements change
- Refactor tests to reduce duplication
- Keep test execution time under 5 minutes for unit tests
- Document complex test scenarios
- Use descriptive test names that explain the behavior being tested
