# Design Document: Career Development Assessment

## Overview

The Career Development Assessment system is a full-stack web application built with Spring Boot (backend) and React (frontend) that enables employees to evaluate and track their career growth through a structured 9-pillar framework. The system emphasizes evidence-based assessment, where employees document concrete work activities and use them to support their competency ratings across nine career development pillars.

### Key Design Principles

1. **Evidence-Driven Assessment**: All pillar scores should be supported by documented evidence of actual work activities
2. **Data Integrity**: Assessment responses and evidence must be reliably persisted and retrievable
3. **Security First**: Employee data must be encrypted and access-controlled
4. **Progressive Enhancement**: Support draft saving to prevent data loss during assessment completion
5. **Responsive Design**: Provide consistent experience across desktop, tablet, and mobile devices

### System Boundaries

**In Scope:**
- Employee authentication and session management
- Evidence creation, storage, and retrieval
- Assessment completion with 9-pillar scoring
- Progress tracking and visualization across multiple assessments
- Draft saving and restoration
- Data encryption and access control

**Out of Scope:**
- Manager review or approval workflows
- Peer feedback or 360-degree assessments
- Automated scoring or AI-based recommendations
- Integration with HR systems or performance management platforms
- Multi-language support (initial version English only)

## Architecture

### High-Level Architecture

The system follows a three-tier architecture with clear separation of concerns:

```
┌─────────────────────────────────────────────────────────┐
│                    React Frontend (SPA)                  │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │ Assessment   │  │  Evidence    │  │   Progress   │  │
│  │   Views      │  │   Manager    │  │ Visualization│  │
│  └──────────────┘  └──────────────┘  └──────────────┘  │
│           │                │                 │           │
│           └────────────────┴─────────────────┘           │
│                          │                                │
│                   HTTP/REST (JSON)                        │
└───────────────────────────┼───────────────────────────────┘
                            │
┌───────────────────────────┼───────────────────────────────┐
│              Spring Boot Backend (REST API)               │
│  ┌──────────────────────────────────────────────────┐    │
│  │              Controllers Layer                    │    │
│  │  AuthController │ AssessmentController │          │    │
│  │  EvidenceController │ ProgressController          │    │
│  └──────────────────────────────────────────────────┘    │
│                          │                                │
│  ┌──────────────────────────────────────────────────┐    │
│  │              Services Layer                       │    │
│  │  AuthService │ AssessmentService │                │    │
│  │  EvidenceService │ ProgressService                │    │
│  └──────────────────────────────────────────────────┘    │
│                          │                                │
│  ┌──────────────────────────────────────────────────┐    │
│  │          Repository Layer (Spring Data JPA)       │    │
│  │  EmployeeRepository │ AssessmentRepository │      │    │
│  │  EvidenceRepository                               │    │
│  └──────────────────────────────────────────────────┘    │
└───────────────────────────┼───────────────────────────────┘
                            │
┌───────────────────────────┼───────────────────────────────┐
│                  Relational Database                      │
│         (PostgreSQL/MySQL/H2 for development)             │
└───────────────────────────────────────────────────────────┘
```

### Component Responsibilities

**Frontend (React)**
- User interface rendering and interaction
- Form validation and user input handling
- State management for assessment drafts
- API communication via Axios/Fetch
- Progress visualization using charting library (e.g., Chart.js, Recharts)

**Backend (Spring Boot)**
- RESTful API endpoints
- Business logic enforcement
- Data validation and sanitization
- Authentication and authorization (Spring Security)
- Database operations via JPA
- Data encryption/decryption

**Database**
- Persistent storage for employees, assessments, evidence, and pillar scores
- Relational integrity enforcement
- Query optimization for progress tracking

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
- `POST /api/auth/login`: Authenticate employee credentials
- `POST /api/auth/logout`: Terminate session
- `GET /api/auth/session`: Validate current session

**AuthService**
- `authenticate(username, password)`: Verify credentials and create session
- `validateSession(sessionToken)`: Check if session is active
- `terminateSession(sessionToken)`: End session

#### 2. Evidence Module

**EvidenceController**
- `POST /api/evidence`: Create new evidence item
- `GET /api/evidence`: Retrieve all evidence for authenticated employee
- `GET /api/evidence/{id}`: Retrieve specific evidence item
- `PUT /api/evidence/{id}`: Update evidence item
- `DELETE /api/evidence/{id}`: Delete evidence item

**EvidenceService**
- `createEvidence(employeeId, evidenceData)`: Validate and persist evidence
- `getEmployeeEvidence(employeeId)`: Retrieve all evidence for employee
- `getEvidenceById(evidenceId, employeeId)`: Retrieve specific evidence with access control
- `updateEvidence(evidenceId, employeeId, evidenceData)`: Update existing evidence
- `deleteEvidence(evidenceId, employeeId)`: Remove evidence

#### 3. Assessment Module

**AssessmentController**
- `POST /api/assessments`: Create new assessment (draft or complete)
- `GET /api/assessments`: Retrieve all assessments for authenticated employee
- `GET /api/assessments/{id}`: Retrieve specific assessment
- `PUT /api/assessments/{id}`: Update assessment (draft only)
- `POST /api/assessments/{id}/submit`: Submit draft as complete

**AssessmentService**
- `createAssessment(employeeId, assessmentData, isDraft)`: Create assessment response
- `getEmployeeAssessments(employeeId)`: Retrieve assessment history
- `getAssessmentById(assessmentId, employeeId)`: Retrieve specific assessment
- `updateDraft(assessmentId, employeeId, assessmentData)`: Update draft assessment
- `submitAssessment(assessmentId, employeeId)`: Mark draft as complete
- `validatePillarScores(scores)`: Ensure all 9 pillars scored 1-5

#### 4. Progress Module

**ProgressController**
- `GET /api/progress`: Retrieve progress report for authenticated employee
- `GET /api/progress/pillars/{pillarName}`: Retrieve history for specific pillar

**ProgressService**
- `generateProgressReport(employeeId)`: Calculate trends across assessments
- `getPillarHistory(employeeId, pillarName)`: Get score history for one pillar
- `calculateTrends(assessments)`: Compute growth metrics

### Frontend Components

#### 1. Authentication Components
- `LoginPage`: Login form and credential submission
- `AuthContext`: Global authentication state management
- `ProtectedRoute`: Route guard for authenticated pages

#### 2. Evidence Components
- `EvidenceList`: Display all evidence items
- `EvidenceForm`: Create/edit evidence with quality factors
- `EvidenceCard`: Display single evidence item with pillars

#### 3. Assessment Components
- `AssessmentForm`: Main assessment interface with 9 pillars
- `PillarScoreInput`: Individual pillar scoring with evidence display
- `PillarDescription`: Show pillar details and scoring guidance
- `DraftSaveButton`: Save progress without submission

#### 4. Progress Components
- `ProgressDashboard`: Overview of career development
- `PillarRadarChart`: Radar chart showing all 9 pillars
- `PillarTrendChart`: Line chart for individual pillar over time
- `AssessmentHistory`: List of completed assessments

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
  "employeeId": "long",
  "expiresAt": "ISO8601 timestamp"
}

Response (401):
{
  "error": "Invalid credentials"
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
  "personalContribution": "string",
  "associatedPillars": ["THINKS", "DESIGNS"]
}

Response (201):
{
  "id": "long",
  "employeeId": "long",
  "description": "string",
  "impact": "string",
  "complexity": "string",
  "personalContribution": "string",
  "associatedPillars": ["THINKS", "DESIGNS"],
  "createdAt": "ISO8601 timestamp"
}
```

**GET /api/evidence**
```json
Response (200):
[
  {
    "id": "long",
    "description": "string",
    "impact": "string",
    "complexity": "string",
    "personalContribution": "string",
    "associatedPillars": ["THINKS", "DESIGNS"],
    "createdAt": "ISO8601 timestamp"
  }
]
```

#### Assessment

**POST /api/assessments**
```json
Request:
{
  "isDraft": "boolean",
  "pillarScores": {
    "THINKS": 3,
    "ENGAGES": 4,
    "INFLUENCES": 3,
    "ACHIEVES": 4,
    "DEFINES": 3,
    "DESIGNS": 4,
    "DELIVERS": 5,
    "CONTROLS": 3,
    "OPERATES": 4
  },
  "evidenceReferences": {
    "THINKS": [1, 2],
    "DESIGNS": [2, 3]
  }
}

Response (201):
{
  "id": "long",
  "employeeId": "long",
  "isDraft": "boolean",
  "pillarScores": { ... },
  "evidenceReferences": { ... },
  "createdAt": "ISO8601 timestamp",
  "submittedAt": "ISO8601 timestamp or null"
}
```

#### Progress

**GET /api/progress**
```json
Response (200):
{
  "employeeId": "long",
  "assessmentCount": "int",
  "pillarTrends": {
    "THINKS": [
      {"assessmentDate": "ISO8601", "score": 3},
      {"assessmentDate": "ISO8601", "score": 4}
    ],
    "ENGAGES": [ ... ],
    ...
  },
  "latestScores": {
    "THINKS": 4,
    "ENGAGES": 4,
    ...
  }
}
```

## Data Models

### Entity Relationship Diagram

```
┌─────────────────┐
│    Employee     │
├─────────────────┤
│ id (PK)         │
│ username        │
│ passwordHash    │
│ email           │
│ createdAt       │
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
         │ N:M
         │
┌────────┴────────────┐
│ EvidencePillar      │
├─────────────────────┤
│ evidenceId (FK)     │
│ pillarName          │
└─────────────────────┘

┌─────────────────┐
│   Assessment    │
├─────────────────┤
│ id (PK)         │
│ employeeId (FK) │
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
│ pillarName          │
│ score               │
└─────────────────────┘
         │
         │ N:M
         │
┌────────┴────────────────┐
│ AssessmentEvidence      │
├─────────────────────────┤
│ assessmentId (FK)       │
│ pillarName              │
│ evidenceId (FK)         │
└─────────────────────────┘
```

### JPA Entity Definitions

#### Employee Entity
```java
@Entity
@Table(name = "employees")
public class Employee {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(unique = true, nullable = false)
    private String username;
    
    @Column(nullable = false)
    private String passwordHash;
    
    @Column(unique = true, nullable = false)
    private String email;
    
    @Column(nullable = false)
    private LocalDateTime createdAt;
    
    @OneToMany(mappedBy = "employee", cascade = CascadeType.ALL)
    private List<Evidence> evidenceList;
    
    @OneToMany(mappedBy = "employee", cascade = CascadeType.ALL)
    private List<Assessment> assessments;
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
    
    @ElementCollection
    @CollectionTable(name = "evidence_pillars", 
                     joinColumns = @JoinColumn(name = "evidence_id"))
    @Column(name = "pillar_name")
    @Enumerated(EnumType.STRING)
    private Set<PillarName> associatedPillars;
}
```

#### Assessment Entity
```java
@Entity
@Table(name = "assessments")
public class Assessment {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "employee_id", nullable = false)
    private Employee employee;
    
    @Column(nullable = false)
    private Boolean isDraft;
    
    @Column(nullable = false)
    private LocalDateTime createdAt;
    
    @Column
    private LocalDateTime submittedAt;
    
    @OneToMany(mappedBy = "assessment", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<PillarScore> pillarScores;
    
    @OneToMany(mappedBy = "assessment", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<AssessmentEvidence> evidenceReferences;
}
```

#### PillarScore Entity
```java
@Entity
@Table(name = "pillar_scores")
public class PillarScore {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "assessment_id", nullable = false)
    private Assessment assessment;
    
    @Enumerated(EnumType.STRING)
    @Column(nullable = false)
    private PillarName pillarName;
    
    @Column(nullable = false)
    @Min(1)
    @Max(5)
    private Integer score;
}
```

#### AssessmentEvidence Entity
```java
@Entity
@Table(name = "assessment_evidence")
public class AssessmentEvidence {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "assessment_id", nullable = false)
    private Assessment assessment;
    
    @Enumerated(EnumType.STRING)
    @Column(nullable = false)
    private PillarName pillarName;
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "evidence_id", nullable = false)
    private Evidence evidence;
}
```

#### PillarName Enum
```java
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
```

### Database Schema Considerations

**Indexing Strategy:**
- Index on `employee_id` in Evidence, Assessment tables for fast employee-specific queries
- Index on `assessment_id` in PillarScore and AssessmentEvidence for join performance
- Index on `createdAt` and `submittedAt` in Assessment for chronological queries
- Composite index on `(employee_id, isDraft)` for draft retrieval

**Data Encryption:**
- Encrypt sensitive text fields (description, impact, complexity, personalContribution) at application layer before persistence
- Use AES-256 encryption with employee-specific keys derived from master key
- Store encryption metadata separately for key rotation support

**Constraints:**
- Unique constraint on `(assessment_id, pillar_name)` in PillarScore to prevent duplicate pillar scores
- Check constraint on score field: `score >= 1 AND score <= 5`
- Foreign key constraints with CASCADE DELETE for assessment-related entities

