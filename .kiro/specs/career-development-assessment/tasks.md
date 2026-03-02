# Implementation Tasks: Career Development Assessment

## Overview

This implementation plan follows Test-Driven Development (TDD) principles with small, incremental tasks. Each task builds on previous work, with tests written before implementation code. The system supports multi-role assessment (Employee, Manager, Independent_Assessor), evidence-based scoring with versioning, ECDF assessments with point-in-time snapshots, performance evaluation, and personal development plans.

## Tasks

### Phase 1: Project Setup and Foundation

- [x] 1. Set up backend project structure
  - Create Spring Boot project with Gradle
  - Configure build.gradle with dependencies: Spring Web, Data JPA, Security, Validation, H2, PostgreSQL driver
  - Set up application.properties for H2 (dev) and PostgreSQL (prod) profiles
  - Create package structure: controller, service, repository, model, config, security, dto, exception
  - Configure basic Spring Security (initially permit all for development)
  - _Requirements: Foundation for all backend features_

- [x] 2. Create basic Spring Boot application test
  - Write test that verifies Spring application context loads successfully
  - Use @SpringBootTest annotation
  - Test should pass with minimal configuration
  - This enables continuous testing as features are added
  - _Requirements: Foundation for continuous testing_

- [ ] 3. Set up frontend project structure
  - Create React project with Create React App
  - Install dependencies: React Router, Axios, Chart.js or Recharts, Material-UI or Ant Design
  - Set up folder structure: components, pages, services, hooks, utils, contexts
  - Configure environment variables for API URL (.env files)
  - Set up basic routing with React Router
  - _Requirements: Foundation for all frontend features_

- [ ] 4. Set up database migration tool
  - Configure Flyway or Liquibase in build.gradle
  - Create initial migration directory structure
  - Test migration execution with H2
  - _Requirements: Foundation for database schema management_

### Phase 2: Core Domain Models and Enums

- [ ] 5. Create enum classes with tests
  - [ ] 5.1 Write test for UserRole enum values
    - Test enum contains EMPLOYEE, MANAGER, INDEPENDENT_ASSESSOR
    - _Requirements: 1.5_
  
  - [ ] 5.2 Implement UserRole enum
    - Create enum with three values
    - _Requirements: 1.5_
  
  - [ ] 5.3 Write test for EmployeeGrade enum values
    - Test enum contains JUNIOR, MID_LEVEL, SENIOR, PRINCIPAL
    - _Requirements: 2.2, 20.3_
  
  - [ ] 5.4 Implement EmployeeGrade enum
    - Create enum with four grade levels
    - _Requirements: 2.2, 20.3_
  
  - [ ] 5.5 Write test for PillarName enum values
    - Test enum contains all 9 pillars: THINKS, ENGAGES, INFLUENCES, ACHIEVES, DEFINES, DESIGNS, DELIVERS, CONTROLS, OPERATES
    - _Requirements: 3.1_
  
  - [ ] 5.6 Implement PillarName enum
    - Create enum with 9 pillar values
    - _Requirements: 3.1_
  
  - [ ] 5.7 Write test for AssessorType enum values
    - Test enum contains EMPLOYEE, MANAGER, INDEPENDENT_ASSESSOR
    - _Requirements: 8.1_
  
  - [ ] 5.8 Implement AssessorType enum
    - Create enum with three assessor types
    - _Requirements: 8.1_
  
  - [ ] 5.9 Write test for PerformanceStatus enum values
    - Test enum contains UNDER_PERFORMING, AT_EXPECTATION, OVER_PERFORMING
    - _Requirements: 21.2, 21.3, 21.4, 21.5_
  
  - [ ] 5.10 Implement PerformanceStatus enum
    - Create enum with three performance status values
    - _Requirements: 21.2, 21.3, 21.4, 21.5_
  
  - [ ] 5.11 Write test for GoalStatus enum values
    - Test enum contains NOT_STARTED, IN_PROGRESS, COMPLETED
    - _Requirements: 22.3_
  
  - [ ] 5.12 Implement GoalStatus enum
    - Create enum with three goal status values
    - _Requirements: 22.3_


- [ ] 6. Create User entity with tests
  - [ ] 6.1 Write test for User entity creation
    - Test User can be created with username, passwordHash, email, role
    - Test createdAt is automatically set
    - _Requirements: 1.1, 1.5_
  
  - [ ] 6.2 Implement User entity
    - Create JPA entity with id, username, passwordHash, email, role, createdAt
    - Add @Entity, @Table, @Id, @GeneratedValue annotations
    - _Requirements: 1.1, 1.5_
  
  - [ ] 6.3 Write test for User validation constraints
    - Test username uniqueness constraint
    - Test email uniqueness constraint
    - Test required fields are not null
    - _Requirements: 1.1_
  
  - [ ] 6.4 Add validation annotations to User entity
    - Add @Column(unique=true, nullable=false) for username and email
    - Add @Column(nullable=false) for required fields
    - _Requirements: 1.1_
  
  - [ ] 6.5 Create database migration for users table
    - Write Flyway/Liquibase migration script
    - Include unique constraints and indexes
    - _Requirements: 1.1_

- [ ] 7. Create Employee entity with tests
  - [ ] 7.1 Write test for Employee entity creation
    - Test Employee can be created with name, jobTitle, department, grade, hireDate
    - Test Employee has reference to User
    - _Requirements: 2.1, 2.2_
  
  - [ ] 7.2 Implement Employee entity
    - Create JPA entity with id, user (OneToOne), name, jobTitle, department, grade, hireDate
    - Add proper JPA annotations
    - _Requirements: 2.1, 2.2_
  
  - [ ] 7.3 Write test for Employee-Manager relationship
    - Test Employee can have manager reference (User with MANAGER role)
    - Test manager can be null
    - _Requirements: 10.1, 28.2_
  
  - [ ] 7.4 Add manager relationship to Employee entity
    - Add @ManyToOne manager field referencing User
    - _Requirements: 10.1, 28.2_
  
  - [ ] 7.5 Create database migration for employees table
    - Write migration script with foreign keys to users table
    - Include manager_id foreign key
    - _Requirements: 2.1, 2.2_

- [ ] 8. Create Evidence entity with tests
  - [ ] 8.1 Write test for Evidence entity creation
    - Test Evidence can be created with description, impact, complexity, personalContribution
    - Test Evidence has reference to Employee
    - Test createdAt is automatically set
    - _Requirements: 4.1, 4.2, 4.3, 4.4_
  
  - [ ] 8.2 Implement Evidence entity
    - Create JPA entity with id, employee (ManyToOne), description, impact, complexity, personalContribution, createdAt
    - Use TEXT column type for text fields
    - _Requirements: 4.1, 4.2, 4.3, 4.4_
  
  - [ ] 8.3 Write test for Evidence validation
    - Test all quality factors (impact, complexity, personalContribution) are required
    - Test description is required
    - _Requirements: 4.5_
  
  - [ ] 8.4 Add validation annotations to Evidence entity
    - Add @Column(nullable=false) for all required fields
    - Add @NotBlank validation annotations
    - _Requirements: 4.5_
  
  - [ ] 8.5 Create database migration for evidence table
    - Write migration script with foreign key to employees table
    - Include index on employee_id
    - _Requirements: 4.6_

- [ ] 9. Create EvidenceScore entity with tests
  - [ ] 9.1 Write test for EvidenceScore entity creation
    - Test EvidenceScore can be created with evidence, pillarName, score, assessor, assessorType, versionId, timestamp
    - Test versionId is UUID string
    - _Requirements: 5.3, 5.5, 8.1, 8.2, 8.3, 24.1_
  
  - [ ] 9.2 Implement EvidenceScore entity
    - Create JPA entity with id, evidence (ManyToOne), pillarName, score, assessor (ManyToOne to User), assessorType, versionId, timestamp
    - _Requirements: 5.3, 5.5, 8.1, 8.2, 8.3, 24.1_
  
  - [ ] 9.3 Write test for score range validation
    - Test score must be between 1 and 5 inclusive
    - Test scores outside range are rejected
    - _Requirements: 3.3, 5.4_
  
  - [ ] 9.4 Add validation annotations for score range
    - Add @Min(1) and @Max(5) to score field
    - _Requirements: 3.3, 5.4_
  
  - [ ] 9.5 Create database migration for evidence_scores table
    - Write migration script with foreign keys to evidence and users tables
    - Include composite index on (evidence_id, assessor_id, assessor_type)
    - Include index on version_id
    - _Requirements: 5.5, 8.2, 24.1_


- [ ] 10. Create ECDFAssessment entity with tests
  - [ ] 10.1 Write test for ECDFAssessment entity creation
    - Test ECDFAssessment can be created with employee, versionId, versionNumber, isDraft, createdAt
    - Test versionId is unique UUID string
    - Test versionNumber is sequential per employee
    - _Requirements: 25.1, 25.2_
  
  - [ ] 10.2 Implement ECDFAssessment entity
    - Create JPA entity with id, employee (ManyToOne), versionId, versionNumber, isDraft, createdAt, submittedAt
    - Add unique constraint on versionId
    - _Requirements: 25.1, 25.2_
  
  - [ ] 10.3 Create database migration for ecdf_assessments table
    - Write migration script with foreign key to employees table
    - Include unique index on version_id
    - Include composite index on (employee_id, version_number)
    - _Requirements: 25.2_

- [ ] 11. Create PillarScore entity with tests
  - [ ] 11.1 Write test for PillarScore entity creation
    - Test PillarScore can be created with assessment, pillarName, aggregatedScore, performanceStatus
    - Test aggregatedScore is between 1.0 and 5.0
    - _Requirements: 9.4, 21.2_
  
  - [ ] 11.2 Implement PillarScore entity
    - Create JPA entity with id, assessment (ManyToOne), pillarName, aggregatedScore, performanceStatus
    - Add unique constraint on (assessment_id, pillar_name)
    - _Requirements: 9.4, 21.2_
  
  - [ ] 11.3 Create database migration for pillar_scores table
    - Write migration script with foreign key to ecdf_assessments table
    - Include unique constraint on (assessment_id, pillar_name)
    - _Requirements: 9.7_

- [ ] 12. Create ExpectedScore entity with tests
  - [ ] 12.1 Write test for ExpectedScore entity creation
    - Test ExpectedScore can be created with grade, pillarName, expectedScore
    - Test expectedScore is between 1.0 and 5.0
    - Test unique constraint on (grade, pillarName)
    - _Requirements: 20.1, 20.2_
  
  - [ ] 12.2 Implement ExpectedScore entity
    - Create JPA entity with id, grade, pillarName, expectedScore
    - Add unique constraint on (grade, pillar_name)
    - _Requirements: 20.1, 20.2_
  
  - [ ] 12.3 Create database migration for expected_scores table
    - Write migration script
    - Include unique constraint on (grade, pillar_name)
    - _Requirements: 20.1_
  
  - [ ] 12.4 Create seed data migration for expected scores
    - Populate expected scores for all grade-pillar combinations
    - Use reasonable default values (e.g., JUNIOR: 2.0-3.0, SENIOR: 3.5-4.5)
    - _Requirements: 20.1, 20.2_

- [ ] 13. Create DevelopmentPlan and DevelopmentGoal entities with tests
  - [ ] 13.1 Write test for DevelopmentPlan entity creation
    - Test DevelopmentPlan can be created with employee, triggeringAssessment, createdAt
    - _Requirements: 22.5_
  
  - [ ] 13.2 Implement DevelopmentPlan entity
    - Create JPA entity with id, employee (ManyToOne), triggeringAssessment (ManyToOne to ECDFAssessment), createdAt
    - _Requirements: 22.5_
  
  - [ ] 13.3 Write test for DevelopmentGoal entity creation
    - Test DevelopmentGoal can be created with plan, pillarName, description, targetDate, plannedActions, status
    - _Requirements: 22.3_
  
  - [ ] 13.4 Implement DevelopmentGoal entity
    - Create JPA entity with id, plan (ManyToOne), pillarName, description, targetDate, plannedActions, status
    - _Requirements: 22.3_
  
  - [ ] 13.5 Create database migrations for development_plans and development_goals tables
    - Write migration scripts with proper foreign keys
    - _Requirements: 22.5_

- [ ] 14. Checkpoint - Verify all entities and migrations
  - Run all tests to ensure entities are correctly defined
  - Execute migrations on H2 database
  - Verify database schema matches entity definitions
  - Ask user if questions arise

### Phase 3: Repository Layer

- [ ] 14. Create UserRepository with tests
  - [ ] 14.1 Write test for findByUsername
    - Test finding user by username returns correct user
    - Test finding non-existent username returns empty
    - _Requirements: 1.1_
  
  - [ ] 14.2 Implement UserRepository interface
    - Extend JpaRepository<User, Long>
    - Add findByUsername(String username) method
    - _Requirements: 1.1_
  
  - [ ] 14.3 Write test for findByEmail
    - Test finding user by email returns correct user
    - _Requirements: 1.1_
  
  - [ ] 14.4 Add findByEmail method to UserRepository
    - Add Optional<User> findByEmail(String email)
    - _Requirements: 1.1_

- [ ] 15. Create EmployeeRepository with tests
  - [ ] 15.1 Write test for findByUserId
    - Test finding employee by user ID returns correct employee
    - _Requirements: 2.1_
  
  - [ ] 15.2 Implement EmployeeRepository interface
    - Extend JpaRepository<Employee, Long>
    - Add findByUserId(Long userId) method
    - _Requirements: 2.1_
  
  - [ ] 15.3 Write test for findByManagerId
    - Test finding all employees for a manager
    - _Requirements: 10.1, 28.2_
  
  - [ ] 15.4 Add findByManagerId method to EmployeeRepository
    - Add List<Employee> findByManagerId(Long managerId)
    - _Requirements: 10.1, 28.2_

- [ ] 16. Create EvidenceRepository with tests
  - [ ] 16.1 Write test for findByEmployeeId
    - Test finding all evidence for an employee
    - Test ordering by createdAt descending
    - _Requirements: 4.6, 13.1_
  
  - [ ] 16.2 Implement EvidenceRepository interface
    - Extend JpaRepository<Evidence, Long>
    - Add findByEmployeeIdOrderByCreatedAtDesc(Long employeeId) method
    - _Requirements: 4.6, 13.1_
  
  - [ ] 16.3 Write test for findByEmployeeIdAndCreatedAtBefore
    - Test finding evidence created before a specific date
    - _Requirements: 26.2, 26.7_
  
  - [ ] 16.4 Add findByEmployeeIdAndCreatedAtBefore method
    - Add method for temporal evidence queries
    - _Requirements: 26.2, 26.7_

- [ ] 17. Create EvidenceScoreRepository with tests
  - [ ] 17.1 Write test for findByEvidenceId
    - Test finding all scores for an evidence item
    - _Requirements: 8.5, 13.2_
  
  - [ ] 17.2 Implement EvidenceScoreRepository interface
    - Extend JpaRepository<EvidenceScore, Long>
    - Add findByEvidenceId(Long evidenceId) method
    - _Requirements: 8.5, 13.2_
  
  - [ ] 17.3 Write test for findByEvidenceIdAndAssessorType
    - Test filtering scores by assessor type
    - _Requirements: 8.5, 13.3_
  
  - [ ] 17.4 Add findByEvidenceIdAndAssessorType method
    - Add List<EvidenceScore> findByEvidenceIdAndAssessorType(Long evidenceId, AssessorType assessorType)
    - _Requirements: 8.5, 13.3_
  
  - [ ] 17.5 Write test for findByEvidenceIdAndPillarNameOrderByTimestampDesc
    - Test finding score history for evidence-pillar combination
    - _Requirements: 24.5_
  
  - [ ] 17.6 Add findByEvidenceIdAndPillarNameOrderByTimestampDesc method
    - Add method for score version history
    - _Requirements: 24.5_


- [ ] 18. Create ECDFAssessmentRepository with tests
  - [ ] 18.1 Write test for findByEmployeeIdOrderByVersionNumberDesc
    - Test finding all ECDF versions for employee in reverse chronological order
    - _Requirements: 25.5, 27.1_
  
  - [ ] 18.2 Implement ECDFAssessmentRepository interface
    - Extend JpaRepository<ECDFAssessment, Long>
    - Add findByEmployeeIdOrderByVersionNumberDesc(Long employeeId) method
    - _Requirements: 25.5, 27.1_
  
  - [ ] 18.3 Write test for findByVersionId
    - Test finding assessment by version ID
    - _Requirements: 25.6_
  
  - [ ] 18.4 Add findByVersionId method
    - Add Optional<ECDFAssessment> findByVersionId(String versionId)
    - _Requirements: 25.6_
  
  - [ ] 18.5 Write test for findTopByEmployeeIdOrderByVersionNumberDesc
    - Test finding latest ECDF version for employee
    - _Requirements: 25.6_
  
  - [ ] 18.6 Add findTopByEmployeeIdOrderByVersionNumberDesc method
    - Add method to get most recent assessment
    - _Requirements: 25.6_

- [ ] 19. Create remaining repositories with tests
  - [ ] 19.1 Create PillarScoreRepository
    - Extend JpaRepository<PillarScore, Long>
    - Add findByAssessmentId(Long assessmentId) method
    - _Requirements: 9.7_
  
  - [ ] 19.2 Create ExpectedScoreRepository
    - Extend JpaRepository<ExpectedScore, Long>
    - Add findByGradeAndPillarName(EmployeeGrade grade, PillarName pillarName) method
    - Add findByGrade(EmployeeGrade grade) method
    - _Requirements: 20.1_
  
  - [ ] 19.3 Create DevelopmentPlanRepository
    - Extend JpaRepository<DevelopmentPlan, Long>
    - Add findByEmployeeIdOrderByCreatedAtDesc(Long employeeId) method
    - _Requirements: 22.5, 23.1_
  
  - [ ] 19.4 Create DevelopmentGoalRepository
    - Extend JpaRepository<DevelopmentGoal, Long>
    - Add findByPlanId(Long planId) method
    - _Requirements: 22.3_

- [ ] 20. Checkpoint - Verify all repositories
  - Run all repository tests
  - Verify queries execute correctly against H2
  - Check test coverage for repository layer
  - Ask user if questions arise

### Phase 4: Authentication and Authorization

- [ ] 21. Create authentication service with tests
  - [ ] 21.1 Write test for successful authentication
    - Test valid credentials create session with correct role
    - _Requirements: 1.1_
  
  - [ ] 21.2 Implement AuthService.authenticate method
    - Verify username exists
    - Verify password hash matches (use BCrypt)
    - Create session token (JWT or UUID)
    - Return session with user ID and role
    - _Requirements: 1.1_
  
  - [ ] 21.3 Write test for invalid credentials rejection
    - Test non-existent username is rejected
    - Test wrong password is rejected
    - _Requirements: 1.2_
  
  - [ ] 21.4 Add credential validation to AuthService
    - Return appropriate error for invalid credentials
    - _Requirements: 1.2_
  
  - [ ] 21.5 Write test for session validation
    - Test active session returns user info and role
    - _Requirements: 1.3_
  
  - [ ] 21.6 Implement AuthService.validateSession method
    - Verify session token is valid
    - Return user ID and role
    - _Requirements: 1.3_
  
  - [ ] 21.7 Write test for session termination
    - Test logout invalidates session
    - _Requirements: 1.4_
  
  - [ ] 21.8 Implement AuthService.terminateSession method
    - Invalidate session token
    - _Requirements: 1.4_

- [ ] 22. Create authentication controller with tests
  - [ ] 22.1 Write test for POST /api/auth/login endpoint
    - Test successful login returns session token and role
    - Test invalid credentials return 401
    - _Requirements: 1.1, 1.2_
  
  - [ ] 22.2 Implement AuthController.login endpoint
    - Accept username and password
    - Call AuthService.authenticate
    - Return session token, user ID, role, expiration
    - _Requirements: 1.1, 1.2_
  
  - [ ] 22.3 Write test for POST /api/auth/logout endpoint
    - Test logout terminates session
    - _Requirements: 1.4_
  
  - [ ] 22.4 Implement AuthController.logout endpoint
    - Accept session token
    - Call AuthService.terminateSession
    - _Requirements: 1.4_
  
  - [ ] 22.5 Write test for GET /api/auth/session endpoint
    - Test returns current user info and role
    - _Requirements: 1.3_
  
  - [ ] 22.6 Implement AuthController.session endpoint
    - Validate session token
    - Return user ID and role
    - _Requirements: 1.3_

- [ ] 23. Configure Spring Security with tests
  - [ ] 23.1 Write test for JWT token generation and validation
    - Test token contains user ID and role
    - Test token expiration
    - _Requirements: 1.1, 1.3_
  
  - [ ] 23.2 Implement JwtTokenProvider utility
    - Generate JWT tokens with user ID and role claims
    - Validate JWT tokens
    - Extract user ID and role from token
    - _Requirements: 1.1, 1.3_
  
  - [ ] 23.3 Configure Spring Security filter chain
    - Create JwtAuthenticationFilter
    - Configure public endpoints (/api/auth/login)
    - Configure protected endpoints (all others)
    - _Requirements: 1.1, 1.3, 1.4_
  
  - [ ] 23.4 Write integration test for authentication flow
    - Test login, access protected endpoint, logout
    - _Requirements: 1.1, 1.3, 1.4_

- [ ] 24. Checkpoint - Verify authentication system
  - Run all authentication tests
  - Test login flow end-to-end
  - Verify JWT tokens work correctly
  - Ask user if questions arise

### Phase 5: Employee Profile and Evidence Management

- [ ] 25. Create EmployeeService with tests
  - [ ] 25.1 Write test for getEmployeeProfile
    - Test employee can retrieve own profile
    - Test profile includes name, jobTitle, department, grade, hireDate
    - _Requirements: 2.1, 2.2_
  
  - [ ] 25.2 Implement EmployeeService.getEmployeeProfile method
    - Retrieve employee by user ID
    - Return employee profile data
    - _Requirements: 2.1, 2.2_
  
  - [ ] 25.3 Write test for getEmployeeProfile access control
    - Test manager can access assigned employee profile
    - Test manager cannot access unassigned employee profile
    - Test independent assessor can access assigned employee profile
    - _Requirements: 17.2_
  
  - [ ] 25.4 Add access control to getEmployeeProfile
    - Check requestor role and permissions
    - Throw exception for unauthorized access
    - _Requirements: 17.2_

- [ ] 26. Create EmployeeController with tests
  - [ ] 26.1 Write test for GET /api/employees/profile endpoint
    - Test authenticated employee retrieves own profile
    - _Requirements: 2.1, 2.2_
  
  - [ ] 26.2 Implement EmployeeController.getProfile endpoint
    - Extract user ID from JWT token
    - Call EmployeeService.getEmployeeProfile
    - Return employee profile DTO
    - _Requirements: 2.1, 2.2_
  
  - [ ] 26.3 Write test for GET /api/employees/{id}/profile endpoint
    - Test manager can access assigned employee profile
    - Test returns 403 for unauthorized access
    - _Requirements: 17.2_
  
  - [ ] 26.4 Implement EmployeeController.getEmployeeProfile endpoint
    - Verify requestor has permission
    - Return employee profile
    - _Requirements: 17.2_


- [ ] 27. Create EvidenceService with tests
  - [ ] 27.1 Write test for createEvidence
    - Test employee can create evidence with all quality factors
    - Test evidence is associated with employee
    - _Requirements: 4.1, 4.2, 4.3, 4.4, 4.6_
  
  - [ ] 27.2 Implement EvidenceService.createEvidence method
    - Validate all quality factors are present
    - Create Evidence entity
    - Save to repository
    - _Requirements: 4.1, 4.2, 4.3, 4.4, 4.6_
  
  - [ ] 27.3 Write test for evidence quality factor validation
    - Test missing impact is rejected
    - Test missing complexity is rejected
    - Test missing personalContribution is rejected
    - _Requirements: 4.5_
  
  - [ ] 27.4 Add validation to createEvidence
    - Check all quality factors are non-empty
    - Throw exception if validation fails
    - _Requirements: 4.5_
  
  - [ ] 27.5 Write test for getEmployeeEvidence with access control
    - Test employee can retrieve own evidence
    - Test manager can retrieve assigned employee evidence
    - Test independent assessor can retrieve assigned employee evidence
    - Test unauthorized access is rejected
    - _Requirements: 4.7, 17.2_
  
  - [ ] 27.6 Implement EvidenceService.getEmployeeEvidence method
    - Check requestor permissions
    - Retrieve evidence for employee
    - _Requirements: 4.7, 17.2_
  
  - [ ] 27.7 Write test for updateEvidence
    - Test employee can update own evidence
    - Test employee cannot update others' evidence
    - _Requirements: 18.4_
  
  - [ ] 27.8 Implement EvidenceService.updateEvidence method
    - Verify ownership
    - Update evidence fields
    - _Requirements: 18.4_
  
  - [ ] 27.9 Write test for deleteEvidence
    - Test employee can delete own evidence
    - Test employee cannot delete others' evidence
    - _Requirements: 18.4_
  
  - [ ] 27.10 Implement EvidenceService.deleteEvidence method
    - Verify ownership
    - Delete evidence (or soft delete)
    - _Requirements: 18.4_

- [ ] 28. Create EvidenceController with tests
  - [ ] 28.1 Write test for POST /api/evidence endpoint
    - Test employee can create evidence
    - Test returns 400 if quality factors missing
    - Test returns 403 if non-employee attempts creation
    - _Requirements: 4.1, 4.2, 4.3, 4.4, 4.5, 4.7_
  
  - [ ] 28.2 Implement EvidenceController.createEvidence endpoint
    - Accept evidence DTO with quality factors
    - Call EvidenceService.createEvidence
    - Return created evidence with ID
    - _Requirements: 4.1, 4.2, 4.3, 4.4, 4.5, 4.7_
  
  - [ ] 28.3 Write test for GET /api/evidence endpoint
    - Test employee retrieves own evidence
    - _Requirements: 13.1_
  
  - [ ] 28.4 Implement EvidenceController.getMyEvidence endpoint
    - Extract employee ID from JWT
    - Call EvidenceService.getEmployeeEvidence
    - Return evidence list
    - _Requirements: 13.1_
  
  - [ ] 28.5 Write test for GET /api/employees/{employeeId}/evidence endpoint
    - Test manager can retrieve assigned employee evidence
    - Test returns 403 for unauthorized access
    - _Requirements: 10.3, 11.3, 17.2_
  
  - [ ] 28.6 Implement EvidenceController.getEmployeeEvidence endpoint
    - Verify requestor permissions
    - Return employee evidence
    - _Requirements: 10.3, 11.3, 17.2_
  
  - [ ] 28.7 Write test for PUT /api/evidence/{id} endpoint
    - Test employee can update own evidence
    - Test returns 403 for others' evidence
    - _Requirements: 18.4_
  
  - [ ] 28.8 Implement EvidenceController.updateEvidence endpoint
    - Call EvidenceService.updateEvidence
    - _Requirements: 18.4_
  
  - [ ] 28.9 Write test for DELETE /api/evidence/{id} endpoint
    - Test employee can delete own evidence
    - _Requirements: 18.4_
  
  - [ ] 28.10 Implement EvidenceController.deleteEvidence endpoint
    - Call EvidenceService.deleteEvidence
    - _Requirements: 18.4_

- [ ] 29. Checkpoint - Verify employee and evidence features
  - Run all employee and evidence tests
  - Test evidence creation and retrieval end-to-end
  - Verify access control works correctly
  - Ask user if questions arise

### Phase 6: Evidence Scoring (Multi-Role)

- [ ] 30. Create EvidenceScoreService with tests
  - [ ] 30.1 Write test for createEvidenceScore
    - Test employee can score own evidence
    - Test manager can score assigned employee evidence
    - Test independent assessor can score assigned employee evidence
    - Test score is stored with correct assessorType
    - Test versionId is generated (UUID)
    - _Requirements: 5.3, 5.5, 6.3, 6.5, 7.3, 7.5, 8.1, 8.2, 24.1_
  
  - [ ] 30.2 Implement EvidenceScoreService.createEvidenceScore method
    - Validate score range (1-5)
    - Generate UUID for versionId
    - Set timestamp
    - Create EvidenceScore entity with assessorType
    - Save to repository
    - _Requirements: 5.3, 5.5, 6.3, 6.5, 7.3, 7.5, 8.1, 8.2, 24.1_
  
  - [ ] 30.3 Write test for score range validation
    - Test score < 1 is rejected
    - Test score > 5 is rejected
    - Test score 1-5 is accepted
    - _Requirements: 3.3, 5.4, 6.3, 7.3_
  
  - [ ] 30.4 Add score validation to createEvidenceScore
    - Throw exception for invalid scores
    - _Requirements: 3.3, 5.4, 6.3, 7.3_
  
  - [ ] 30.5 Write test for evidence score versioning
    - Test updating score creates new version
    - Test previous version remains unchanged
    - Test new version has new versionId and timestamp
    - _Requirements: 24.2, 24.3_
  
  - [ ] 30.6 Implement EvidenceScoreService.updateEvidenceScore method
    - Create new EvidenceScore record (don't update existing)
    - Generate new versionId
    - Set new timestamp
    - _Requirements: 24.2, 24.3_
  
  - [ ] 30.7 Write test for getEvidenceScores
    - Test retrieves all scores for evidence
    - Test groups scores by assessorType
    - _Requirements: 8.5, 13.2, 13.3_
  
  - [ ] 30.8 Implement EvidenceScoreService.getEvidenceScores method
    - Retrieve scores from repository
    - Group by assessorType
    - _Requirements: 8.5, 13.2, 13.3_
  
  - [ ] 30.9 Write test for assessment independence
    - Test manager cannot view independent assessor scores before completing own
    - Test independent assessor cannot view manager scores before completing own
    - Test employee can view all scores after all assessors complete
    - _Requirements: 17.4_
  
  - [ ] 30.10 Implement assessment independence check
    - Check if requestor has completed their assessment
    - Block access to other assessor scores if not
    - _Requirements: 17.4_
  
  - [ ] 30.11 Write test for getScoreHistory
    - Test retrieves all versions for evidence-pillar combination
    - Test versions ordered by timestamp descending
    - _Requirements: 24.5, 24.7_
  
  - [ ] 30.12 Implement EvidenceScoreService.getScoreHistory method
    - Query repository for all versions
    - Order by timestamp
    - _Requirements: 24.5, 24.7_


- [ ] 31. Create EvidenceScoreController with tests
  - [ ] 31.1 Write test for POST /api/evidence/{evidenceId}/scores endpoint
    - Test employee can score own evidence
    - Test manager can score assigned employee evidence
    - Test independent assessor can score assigned employee evidence
    - Test returns 400 for invalid score range
    - Test returns 403 for unauthorized access
    - _Requirements: 5.1, 5.2, 5.3, 5.4, 6.1, 6.2, 6.3, 7.1, 7.2, 7.3_
  
  - [ ] 31.2 Implement EvidenceScoreController.createEvidenceScores endpoint
    - Accept list of pillar-score pairs
    - Extract assessor ID and type from JWT
    - Call EvidenceScoreService.createEvidenceScore for each pillar
    - Return created scores with versionId
    - _Requirements: 5.1, 5.2, 5.3, 5.4, 6.1, 6.2, 6.3, 7.1, 7.2, 7.3_
  
  - [ ] 31.3 Write test for GET /api/evidence/{evidenceId}/scores endpoint
    - Test retrieves scores grouped by assessorType
    - Test respects assessment independence rules
    - _Requirements: 8.4, 13.2, 13.3, 17.4_
  
  - [ ] 31.4 Implement EvidenceScoreController.getEvidenceScores endpoint
    - Call EvidenceScoreService.getEvidenceScores
    - Return scores grouped by assessorType
    - _Requirements: 8.4, 13.2, 13.3, 17.4_
  
  - [ ] 31.5 Write test for PUT /api/evidence/{evidenceId}/scores/{scoreId} endpoint
    - Test assessor can update own score (creates new version)
    - Test returns 403 for others' scores
    - _Requirements: 8.6, 24.3_
  
  - [ ] 31.6 Implement EvidenceScoreController.updateEvidenceScore endpoint
    - Verify ownership
    - Call EvidenceScoreService.updateEvidenceScore
    - Return new version
    - _Requirements: 8.6, 24.3_
  
  - [ ] 31.7 Write test for GET /api/evidence/{evidenceId}/scores/history endpoint
    - Test retrieves all versions for evidence-pillar
    - _Requirements: 24.5, 24.7_
  
  - [ ] 31.8 Implement EvidenceScoreController.getScoreHistory endpoint
    - Accept pillarName query parameter
    - Call EvidenceScoreService.getScoreHistory
    - _Requirements: 24.5, 24.7_

- [ ] 32. Checkpoint - Verify evidence scoring features
  - Run all evidence scoring tests
  - Test multi-role scoring end-to-end
  - Verify versioning works correctly
  - Verify assessment independence rules
  - Ask user if questions arise

### Phase 7: Score Aggregation and ECDF Assessment

- [ ] 33. Create ScoreAggregationService with tests
  - [ ] 33.1 Write test for aggregatePillarScores
    - Test aggregates scores from all evidence items for a pillar
    - Test combines scores from Employee, Manager, Independent_Assessor
    - Test uses configured aggregation method (average)
    - _Requirements: 9.1, 9.2, 9.3_
  
  - [ ] 33.2 Implement ScoreAggregationService.aggregatePillarScores method
    - Retrieve all evidence for employee
    - Get latest scores for pillar from each evidence item
    - Combine scores from all assessor types
    - Apply aggregation method (average)
    - _Requirements: 9.1, 9.2, 9.3_
  
  - [ ] 33.3 Write test for aggregated score range invariant
    - Test aggregated score is always between 1.0 and 5.0
    - Test with various input combinations
    - _Requirements: 9.4_
  
  - [ ] 33.4 Add validation to aggregatePillarScores
    - Ensure result is in valid range
    - _Requirements: 9.4_
  
  - [ ] 33.5 Write test for aggregateAllPillars
    - Test calculates scores for all 9 pillars
    - Test handles pillars with no evidence
    - _Requirements: 9.1, 9.5_
  
  - [ ] 33.6 Implement ScoreAggregationService.aggregateAllPillars method
    - Call aggregatePillarScores for each pillar
    - Handle pillars with no evidence
    - Return map of pillar to score
    - _Requirements: 9.1, 9.5_
  
  - [ ] 33.7 Write test for getEvidenceContributions
    - Test returns which evidence items contributed to pillar score
    - Test includes scores from all assessor types
    - _Requirements: 9.6, 9.8_
  
  - [ ] 33.8 Implement ScoreAggregationService.getEvidenceContributions method
    - Retrieve evidence with scores for pillar
    - Group scores by evidence and assessor type
    - _Requirements: 9.6, 9.8_

- [ ] 34. Create ECDFAssessmentService with tests
  - [ ] 34.1 Write test for createECDFAssessment
    - Test creates assessment with unique versionId
    - Test assigns sequential versionNumber
    - Test aggregates pillar scores from all evidence
    - _Requirements: 9.1, 9.7, 25.1, 25.2_
  
  - [ ] 34.2 Implement ECDFAssessmentService.createECDFAssessment method
    - Generate UUID for versionId
    - Calculate next versionNumber for employee
    - Call ScoreAggregationService.aggregateAllPillars
    - Create ECDFAssessment entity
    - Create PillarScore entities for all 9 pillars
    - Save to repository
    - _Requirements: 9.1, 9.7, 25.1, 25.2_
  
  - [ ] 34.3 Write test for ECDF assessment versioning
    - Test multiple assessments create separate versions
    - Test previous versions remain unchanged
    - Test versionNumber increments
    - _Requirements: 25.2, 25.4, 25.5_
  
  - [ ] 34.4 Verify versioning in createECDFAssessment
    - Ensure immutability of previous versions
    - _Requirements: 25.2, 25.4, 25.5_
  
  - [ ] 34.5 Write test for getEmployeeECDFVersions
    - Test retrieves all versions for employee
    - Test ordered by versionNumber descending
    - _Requirements: 25.5, 27.1_
  
  - [ ] 34.6 Implement ECDFAssessmentService.getEmployeeECDFVersions method
    - Query repository for all versions
    - Apply access control
    - _Requirements: 25.5, 27.1_
  
  - [ ] 34.7 Write test for getECDFVersionById
    - Test retrieves specific version with all details
    - Test includes pillar scores and evidence contributions
    - _Requirements: 25.6, 25.8_
  
  - [ ] 34.8 Implement ECDFAssessmentService.getECDFVersionById method
    - Retrieve assessment by versionId
    - Load pillar scores
    - Get evidence contributions for each pillar
    - _Requirements: 25.6, 25.8_
  
  - [ ] 34.9 Write test for draft saving
    - Test can save incomplete assessment as draft
    - Test draft can be retrieved and updated
    - Test draft can be submitted as complete version
    - _Requirements: 15.1, 15.2, 15.3, 15.5_
  
  - [ ] 34.10 Implement draft functionality
    - Add saveDraft, updateDraft, submitDraft methods
    - Set isDraft flag appropriately
    - _Requirements: 15.1, 15.2, 15.3, 15.5_


- [ ] 35. Create ECDFAssessmentController with tests
  - [ ] 35.1 Write test for POST /api/assessments/ecdf endpoint
    - Test employee can create ECDF assessment
    - Test assessment includes all 9 pillar scores
    - Test returns versionId and versionNumber
    - _Requirements: 9.1, 9.7, 25.1, 25.2_
  
  - [ ] 35.2 Implement ECDFAssessmentController.createECDFAssessment endpoint
    - Extract employee ID from JWT
    - Call ECDFAssessmentService.createECDFAssessment
    - Return assessment with pillar scores
    - _Requirements: 9.1, 9.7, 25.1, 25.2_
  
  - [ ] 35.3 Write test for GET /api/assessments/ecdf endpoint
    - Test employee retrieves all own ECDF versions
    - _Requirements: 25.5_
  
  - [ ] 35.4 Implement ECDFAssessmentController.getMyECDFVersions endpoint
    - Extract employee ID from JWT
    - Call ECDFAssessmentService.getEmployeeECDFVersions
    - _Requirements: 25.5_
  
  - [ ] 35.5 Write test for GET /api/assessments/ecdf/{versionId} endpoint
    - Test retrieves specific version with all details
    - Test includes evidence contributions
    - _Requirements: 25.6, 25.8_
  
  - [ ] 35.6 Implement ECDFAssessmentController.getECDFVersion endpoint
    - Call ECDFAssessmentService.getECDFVersionById
    - Return complete assessment details
    - _Requirements: 25.6, 25.8_
  
  - [ ] 35.7 Write test for draft endpoints
    - Test POST /api/assessments/ecdf/draft
    - Test PUT /api/assessments/ecdf/draft/{draftId}
    - Test POST /api/assessments/ecdf/draft/{draftId}/submit
    - _Requirements: 15.1, 15.2, 15.3_
  
  - [ ] 35.8 Implement draft endpoints
    - saveDraft, updateDraft, submitDraft
    - _Requirements: 15.1, 15.2, 15.3_

- [ ] 36. Checkpoint - Verify ECDF assessment features
  - Run all ECDF assessment tests
  - Test assessment creation end-to-end
  - Verify score aggregation works correctly
  - Verify versioning and immutability
  - Ask user if questions arise

### Phase 8: Performance Evaluation and Development Plans

- [ ] 37. Create PerformanceEvaluationService with tests
  - [ ] 37.1 Write test for evaluatePerformanceStatus
    - Test compares pillar scores to expected scores
    - Test calculates correct status for each pillar
    - _Requirements: 21.1, 21.2_
  
  - [ ] 37.2 Implement PerformanceEvaluationService.evaluatePerformanceStatus method
    - Get employee grade
    - Get expected scores for grade
    - Compare each pillar score to expected score
    - _Requirements: 21.1, 21.2_
  
  - [ ] 37.3 Write test for performance status calculation
    - Test pillarScore < expectedScore = UNDER_PERFORMING
    - Test pillarScore = expectedScore = AT_EXPECTATION
    - Test pillarScore > expectedScore = OVER_PERFORMING
    - _Requirements: 21.3, 21.4, 21.5_
  
  - [ ] 37.4 Implement calculatePerformanceStatus method
    - Apply comparison logic
    - Return PerformanceStatus enum
    - _Requirements: 21.3, 21.4, 21.5_
  
  - [ ] 37.5 Write test for identifyUnderPerformingPillars
    - Test returns list of pillars with UNDER_PERFORMING status
    - _Requirements: 21.6, 22.1_
  
  - [ ] 37.6 Implement identifyUnderPerformingPillars method
    - Filter pillars by UNDER_PERFORMING status
    - _Requirements: 21.6, 22.1_

- [ ] 38. Update ECDFAssessmentService to include performance status
  - [ ] 38.1 Write test for performance status in ECDF assessment
    - Test assessment includes performance status for each pillar
    - Test expected scores are included
    - _Requirements: 21.6, 21.7, 21.8_
  
  - [ ] 38.2 Modify createECDFAssessment to calculate performance status
    - Call PerformanceEvaluationService.evaluatePerformanceStatus
    - Store performance status in PillarScore entities
    - _Requirements: 21.6, 21.7, 21.8_

- [ ] 39. Create DevelopmentPlanService with tests
  - [ ] 39.1 Write test for createDevelopmentPlan
    - Test creates plan for under-performing pillars
    - Test links plan to triggering ECDF assessment
    - _Requirements: 22.1, 22.2, 22.5_
  
  - [ ] 39.2 Implement DevelopmentPlanService.createDevelopmentPlan method
    - Create DevelopmentPlan entity
    - Link to employee and ECDF assessment
    - _Requirements: 22.1, 22.2, 22.5_
  
  - [ ] 39.3 Write test for addDevelopmentGoal
    - Test creates goal for specific pillar
    - Test includes description, targetDate, plannedActions
    - _Requirements: 22.3, 22.4_
  
  - [ ] 39.4 Implement DevelopmentPlanService.addDevelopmentGoal method
    - Create DevelopmentGoal entity
    - Link to plan and pillar
    - _Requirements: 22.3, 22.4_
  
  - [ ] 39.5 Write test for updateGoalStatus
    - Test can mark goal as IN_PROGRESS or COMPLETED
    - _Requirements: 23.3_
  
  - [ ] 39.6 Implement DevelopmentPlanService.updateGoalStatus method
    - Update goal status
    - _Requirements: 23.3_
  
  - [ ] 39.7 Write test for trackProgress
    - Test compares pillar scores across ECDF versions
    - Test shows improvement for pillar with development goal
    - _Requirements: 23.4, 23.5_
  
  - [ ] 39.8 Implement DevelopmentPlanService.trackProgress method
    - Retrieve ECDF versions
    - Compare pillar scores
    - Calculate improvement
    - _Requirements: 23.4, 23.5_
  
  - [ ] 39.9 Write test for plan outcome evaluation
    - Test notifies when pillar reaches expected score
    - _Requirements: 23.6_
  
  - [ ] 39.10 Implement evaluatePlanOutcome method
    - Check if pillar score meets expected score
    - _Requirements: 23.6_

- [ ] 40. Create DevelopmentPlanController with tests
  - [ ] 40.1 Write test for POST /api/development-plans endpoint
    - Test employee can create development plan
    - Test plan includes goals for under-performing pillars
    - _Requirements: 22.1, 22.2, 22.3_
  
  - [ ] 40.2 Implement DevelopmentPlanController.createDevelopmentPlan endpoint
    - Accept plan with goals
    - Call DevelopmentPlanService.createDevelopmentPlan
    - _Requirements: 22.1, 22.2, 22.3_
  
  - [ ] 40.3 Write test for GET /api/development-plans endpoint
    - Test retrieves active plans for employee
    - _Requirements: 23.1_
  
  - [ ] 40.4 Implement DevelopmentPlanController.getMyDevelopmentPlans endpoint
    - Extract employee ID from JWT
    - Return active plans with goals
    - _Requirements: 23.1_
  
  - [ ] 40.5 Write test for PUT /api/development-plans/goals/{goalId} endpoint
    - Test can update goal status
    - _Requirements: 23.3_
  
  - [ ] 40.6 Implement DevelopmentPlanController.updateGoalStatus endpoint
    - Call DevelopmentPlanService.updateGoalStatus
    - _Requirements: 23.3_

- [ ] 41. Checkpoint - Verify performance evaluation and development plans
  - Run all performance evaluation tests
  - Test development plan creation end-to-end
  - Verify progress tracking works
  - Ask user if questions arise

### Phase 9: Progress Tracking and Historical Comparison

- [ ] 42. Create ProgressService with tests
  - [ ] 42.1 Write test for generateProgressReport
    - Test calculates trends across ECDF versions
    - Test includes pillar score history
    - _Requirements: 14.1, 14.2_
  
  - [ ] 42.2 Implement ProgressService.generateProgressReport method
    - Retrieve all ECDF versions for employee
    - Extract pillar scores from each version
    - Calculate trends
    - _Requirements: 14.1, 14.2_
  
  - [ ] 42.3 Write test for getPillarHistory
    - Test retrieves score history for specific pillar
    - Test ordered chronologically
    - _Requirements: 14.2_
  
  - [ ] 42.4 Implement ProgressService.getPillarHistory method
    - Query ECDF versions
    - Extract pillar scores
    - _Requirements: 14.2_
  
  - [ ] 42.5 Write test for compareVersions
    - Test compares two ECDF versions
    - Test calculates score changes
    - Test identifies improvements and declines
    - _Requirements: 27.2, 27.3, 27.4_
  
  - [ ] 42.6 Implement ProgressService.compareVersions method
    - Retrieve two versions
    - Compare pillar scores
    - Calculate deltas
    - _Requirements: 27.2, 27.3, 27.4_
  
  - [ ] 42.7 Write test for evidence growth tracking
    - Test tracks evidence count over time
    - Test shows new evidence between versions
    - _Requirements: 27.5, 27.6_
  
  - [ ] 42.8 Implement getEvidenceGrowth method
    - Track evidence creation dates
    - Count evidence per ECDF version
    - _Requirements: 27.5, 27.6_


- [ ] 43. Create ProgressController with tests
  - [ ] 43.1 Write test for GET /api/progress endpoint
    - Test employee retrieves own progress report
    - Test includes pillar trends and evidence growth
    - _Requirements: 14.1, 14.2_
  
  - [ ] 43.2 Implement ProgressController.getMyProgress endpoint
    - Extract employee ID from JWT
    - Call ProgressService.generateProgressReport
    - _Requirements: 14.1, 14.2_
  
  - [ ] 43.3 Write test for GET /api/assessments/ecdf/compare endpoint
    - Test compares multiple ECDF versions
    - Test returns score changes and trends
    - _Requirements: 27.1, 27.2, 27.3, 27.4, 27.8_
  
  - [ ] 43.4 Implement ECDFAssessmentController.compareVersions endpoint
    - Accept list of versionIds
    - Call ProgressService.compareVersions
    - Return comparison data
    - _Requirements: 27.1, 27.2, 27.3, 27.4, 27.8_

- [ ] 44. Checkpoint - Verify progress tracking features
  - Run all progress tracking tests
  - Test historical comparison end-to-end
  - Verify trend calculations are correct
  - Ask user if questions arise

### Phase 10: Manager Features

- [ ] 45. Create ManagerService with tests
  - [ ] 45.1 Write test for getAssignedEmployees
    - Test manager retrieves list of assigned employees
    - _Requirements: 10.1, 28.2_
  
  - [ ] 45.2 Implement ManagerService.getAssignedEmployees method
    - Query employees by manager ID
    - _Requirements: 10.1, 28.2_
  
  - [ ] 45.3 Write test for getPendingEvidenceForEmployee
    - Test retrieves evidence needing manager assessment
    - Test filters by hasManagerScore flag
    - _Requirements: 10.2, 10.3, 10.5_
  
  - [ ] 45.4 Implement ManagerService.getPendingEvidenceForEmployee method
    - Get employee evidence
    - Filter for evidence without manager scores
    - _Requirements: 10.2, 10.3, 10.5_
  
  - [ ] 45.5 Write test for getTeamOverview
    - Test calculates team-level statistics
    - Test includes average pillar scores
    - Test includes performance distribution
    - _Requirements: 29.1, 29.2, 29.3_
  
  - [ ] 45.6 Implement ManagerService.getTeamOverview method
    - Get all assigned employees
    - Get latest ECDF assessment for each
    - Calculate averages and distributions
    - _Requirements: 29.1, 29.2, 29.3_
  
  - [ ] 45.7 Write test for getEmployeeProgress
    - Test manager can view employee progress over time
    - Test includes all ECDF versions
    - _Requirements: 28.4, 28.5, 28.6, 28.7_
  
  - [ ] 45.8 Implement ManagerService.getEmployeeProgress method
    - Verify manager-employee relationship
    - Call ProgressService.generateProgressReport
    - _Requirements: 28.4, 28.5, 28.6, 28.7_
  
  - [ ] 45.9 Write test for identifyHighPerformers
    - Test finds employees with over-performing status
    - _Requirements: 29.5_
  
  - [ ] 45.10 Implement identifyHighPerformers method
    - Filter employees by performance status
    - _Requirements: 29.5_
  
  - [ ] 45.11 Write test for identifyEmployeesNeedingSupport
    - Test finds employees with under-performing status or no recent assessment
    - _Requirements: 29.6_
  
  - [ ] 45.12 Implement identifyEmployeesNeedingSupport method
    - Check performance status and assessment dates
    - _Requirements: 29.6_

- [ ] 46. Create ManagerController with tests
  - [ ] 46.1 Write test for GET /api/manager/team endpoint
    - Test manager retrieves assigned employees
    - Test includes pending evidence counts
    - _Requirements: 10.1, 10.2, 28.2, 28.3_
  
  - [ ] 46.2 Implement ManagerController.getTeam endpoint
    - Extract manager ID from JWT
    - Call ManagerService.getAssignedEmployees
    - Include pending evidence counts
    - _Requirements: 10.1, 10.2, 28.2, 28.3_
  
  - [ ] 46.3 Write test for GET /api/manager/team/overview endpoint
    - Test retrieves team statistics
    - Test includes performance distribution
    - _Requirements: 29.1, 29.2, 29.3, 29.4_
  
  - [ ] 46.4 Implement ManagerController.getTeamOverview endpoint
    - Call ManagerService.getTeamOverview
    - _Requirements: 29.1, 29.2, 29.3, 29.4_
  
  - [ ] 46.5 Write test for GET /api/manager/employees/{employeeId}/evidence/pending endpoint
    - Test retrieves pending evidence for employee
    - _Requirements: 10.3, 10.4, 10.5_
  
  - [ ] 46.6 Implement ManagerController.getPendingEvidence endpoint
    - Verify manager-employee relationship
    - Call ManagerService.getPendingEvidenceForEmployee
    - _Requirements: 10.3, 10.4, 10.5_
  
  - [ ] 46.7 Write test for GET /api/manager/employees/{employeeId}/progress endpoint
    - Test manager can view employee progress
    - _Requirements: 28.4, 28.5, 28.6, 28.7, 28.8, 28.9, 28.10_
  
  - [ ] 46.8 Implement ManagerController.getEmployeeProgress endpoint
    - Call ManagerService.getEmployeeProgress
    - _Requirements: 28.4, 28.5, 28.6, 28.7, 28.8, 28.9, 28.10_
  
  - [ ] 46.9 Write test for GET /api/manager/team/performance endpoint
    - Test retrieves team performance distribution
    - _Requirements: 29.3_
  
  - [ ] 46.10 Implement ManagerController.getTeamPerformance endpoint
    - Call ManagerService.getTeamOverview
    - Return performance distribution
    - _Requirements: 29.3_

- [ ] 47. Create IndependentAssessorService and Controller with tests
  - [ ] 47.1 Write test for getAssignedEmployees (assessor)
    - Test independent assessor retrieves assigned employees
    - _Requirements: 11.1, 11.2_
  
  - [ ] 47.2 Implement IndependentAssessorService.getAssignedEmployees method
    - Query assessor assignments
    - _Requirements: 11.1, 11.2_
  
  - [ ] 47.3 Write test for getPendingEvidenceForEmployee (assessor)
    - Test retrieves evidence needing assessor evaluation
    - _Requirements: 11.3, 11.4, 11.5_
  
  - [ ] 47.4 Implement IndependentAssessorService.getPendingEvidenceForEmployee method
    - Similar to manager version but for assessor
    - _Requirements: 11.3, 11.4, 11.5_
  
  - [ ] 47.5 Create IndependentAssessorController with endpoints
    - GET /api/assessor/assignments
    - GET /api/assessor/employees/{employeeId}/evidence/pending
    - _Requirements: 11.1, 11.2, 11.3, 11.4, 11.5_

- [ ] 48. Checkpoint - Verify manager and assessor features
  - Run all manager and assessor tests
  - Test team overview calculations
  - Test pending evidence filtering
  - Verify access control for manager/assessor features
  - Ask user if questions arise

### Phase 11: Frontend - Authentication and Layout

- [ ] 49. Create authentication context and components
  - [ ] 49.1 Create AuthContext with login, logout, session state
    - Store JWT token in localStorage
    - Store user role in context
    - _Requirements: 1.1, 1.3, 1.4_
  
  - [ ] 49.2 Create LoginPage component
    - Username and password form
    - Call /api/auth/login endpoint
    - Store token and redirect based on role
    - _Requirements: 1.1, 1.2_
  
  - [ ] 49.3 Create ProtectedRoute component
    - Check authentication status
    - Redirect to login if not authenticated
    - _Requirements: 1.3_
  
  - [ ] 49.4 Create RoleBasedRoute component
    - Check user role matches required role
    - Redirect if unauthorized
    - _Requirements: 18.1, 18.2, 18.3_

- [ ] 50. Create main layout and navigation
  - [ ] 50.1 Create AppLayout component
    - Header with navigation
    - Sidebar for role-specific menu
    - Main content area
    - _Requirements: 19.1, 19.2_
  
  - [ ] 50.2 Create role-specific navigation menus
    - Employee menu: Evidence, Assessments, Progress, Development Plans
    - Manager menu: Team, Evidence Review, Team Progress
    - Assessor menu: Assignments, Evidence Review
    - _Requirements: 18.1, 18.2, 18.3_

- [ ] 51. Checkpoint - Verify authentication and layout
  - Test login flow
  - Test role-based routing
  - Verify navigation works
  - Ask user if questions arise

### Phase 12: Frontend - Employee Features

- [ ] 52. Create employee profile components
  - [ ] 52.1 Create EmployeeProfile component
    - Display name, jobTitle, department, grade, hireDate
    - Call GET /api/employees/profile
    - _Requirements: 2.1, 2.2, 2.3_
  
  - [ ] 52.2 Create EmployeeDashboard component
    - Show profile summary
    - Show recent evidence count
    - Show latest ECDF assessment
    - Show active development plans
    - _Requirements: 2.1, 2.2_


- [ ] 53. Create evidence management components
  - [ ] 53.1 Create EvidenceList component
    - Display all evidence for employee
    - Show description, createdAt, assessment status
    - Call GET /api/evidence
    - _Requirements: 13.1, 13.2_
  
  - [ ] 53.2 Create EvidenceForm component
    - Form for description, impact, complexity, personalContribution
    - Validate all quality factors are present
    - Call POST /api/evidence
    - _Requirements: 4.1, 4.2, 4.3, 4.4, 4.5_
  
  - [ ] 53.3 Create EvidenceCard component
    - Display single evidence item
    - Show quality factors
    - Show assessment status (employee, manager, assessor)
    - _Requirements: 13.2_
  
  - [ ] 53.4 Create EvidenceDetailView component
    - Show complete evidence details
    - Show scores from all assessor types (after all complete)
    - _Requirements: 13.3, 17.3_

- [ ] 54. Create evidence scoring components
  - [ ] 54.1 Create EvidenceScoreForm component
    - Select pillars evidence demonstrates
    - Input score 1-5 for each pillar
    - Show pillar descriptions
    - Call POST /api/evidence/{evidenceId}/scores
    - _Requirements: 5.1, 5.2, 5.3, 5.7, 16.2, 16.3_
  
  - [ ] 54.2 Create PillarSelector component
    - Checkboxes for 9 pillars
    - Show pillar descriptions on hover/click
    - _Requirements: 5.2, 16.2_
  
  - [ ] 54.3 Create ScoreInput component
    - Number input or slider for score 1-5
    - Validation for range
    - _Requirements: 5.3, 5.4, 16.3_
  
  - [ ] 54.4 Create PillarDescriptionModal component
    - Show pillar description and scoring guidance
    - _Requirements: 16.1, 16.2, 16.4, 16.5_
  
  - [ ] 54.5 Create MultiAssessorScoreComparison component
    - Show scores from Employee, Manager, Assessor side-by-side
    - Highlight differences
    - _Requirements: 13.3, 14.5_

- [ ] 55. Create ECDF assessment components
  - [ ] 55.1 Create ECDFAssessmentForm component
    - Button to create new assessment
    - Call POST /api/assessments/ecdf
    - Show aggregated pillar scores
    - _Requirements: 9.1, 9.7_
  
  - [ ] 55.2 Create ECDFVersionList component
    - Display all ECDF versions chronologically
    - Show versionNumber, date, summary
    - Call GET /api/assessments/ecdf
    - _Requirements: 13.4, 25.5_
  
  - [ ] 55.3 Create ECDFVersionDetail component
    - Show complete assessment details
    - Show pillar scores with performance status
    - Show evidence contributions for each pillar
    - Call GET /api/assessments/ecdf/{versionId}
    - _Requirements: 13.5, 25.6, 25.8_
  
  - [ ] 55.4 Create PillarScoreDisplay component
    - Show pillar score with visual indicator
    - Show expected score for comparison
    - Show performance status (color-coded)
    - _Requirements: 21.6, 21.7, 21.8_
  
  - [ ] 55.5 Create PerformanceStatusIndicator component
    - Visual indicator (color/icon) for under/at/over performing
    - _Requirements: 21.7_

- [ ] 56. Create development plan components
  - [ ] 56.1 Create DevelopmentPlanForm component
    - Show under-performing pillars
    - Form to create goals for each pillar
    - Call POST /api/development-plans
    - _Requirements: 22.1, 22.2, 22.3_
  
  - [ ] 56.2 Create DevelopmentGoalCard component
    - Display goal details
    - Show current score vs expected score
    - Update goal status
    - Call PUT /api/development-plans/goals/{goalId}
    - _Requirements: 23.2, 23.3_
  
  - [ ] 56.3 Create DevelopmentPlanList component
    - Show active development plans
    - Call GET /api/development-plans
    - _Requirements: 23.1_
  
  - [ ] 56.4 Create GoalProgressTracker component
    - Show pillar score improvements over time
    - Highlight when goal is achieved
    - _Requirements: 23.4, 23.5, 23.6_

- [ ] 57. Create progress visualization components
  - [ ] 57.1 Create ProgressDashboard component
    - Overview of career development over time
    - Call GET /api/progress
    - _Requirements: 14.1_
  
  - [ ] 57.2 Create PillarRadarChart component
    - Radar chart showing all 9 pillars for selected ECDF version
    - Use Chart.js or Recharts
    - _Requirements: 14.3_
  
  - [ ] 57.3 Create PillarTrendChart component
    - Line chart for individual pillar across ECDF versions
    - _Requirements: 14.2, 27.7_
  
  - [ ] 57.4 Create ECDFComparisonView component
    - Side-by-side comparison of multiple ECDF versions
    - Show score changes
    - Call GET /api/assessments/ecdf/compare
    - _Requirements: 27.1, 27.2, 27.3, 27.4_
  
  - [ ] 57.5 Create EvidenceGrowthChart component
    - Visualize evidence accumulation over time
    - _Requirements: 27.5_

- [ ] 58. Checkpoint - Verify employee frontend features
  - Test evidence creation and scoring
  - Test ECDF assessment creation
  - Test progress visualization
  - Test development plan creation
  - Ask user if questions arise

### Phase 13: Frontend - Manager Features

- [ ] 59. Create manager dashboard components
  - [ ] 59.1 Create ManagerDashboard component
    - Show team overview statistics
    - Call GET /api/manager/team/overview
    - _Requirements: 29.1, 29.2_
  
  - [ ] 59.2 Create TeamMemberList component
    - List of assigned employees
    - Show pending evidence counts
    - Call GET /api/manager/team
    - _Requirements: 10.1, 10.2, 28.2, 28.3_
  
  - [ ] 59.3 Create TeamOverviewDashboard component
    - Average pillar scores across team
    - Performance distribution charts
    - Recent improvements and needs attention lists
    - _Requirements: 29.1, 29.2, 29.3, 29.4, 29.5, 29.6_

- [ ] 60. Create manager evidence review components
  - [ ] 60.1 Create EmployeeEvidenceReview component
    - Show employee evidence list
    - Filter by assessment status
    - Call GET /api/employees/{employeeId}/evidence
    - _Requirements: 10.3, 10.4, 10.5_
  
  - [ ] 60.2 Create ManagerEvidenceScoreForm component
    - Similar to employee score form but for manager
    - Call POST /api/evidence/{evidenceId}/scores
    - _Requirements: 6.1, 6.2, 6.3, 6.7, 6.8_
  
  - [ ] 60.3 Create PendingEvidenceList component
    - Show evidence needing manager assessment
    - Call GET /api/manager/employees/{employeeId}/evidence/pending
    - _Requirements: 10.2, 10.3, 10.5_

- [ ] 61. Create manager progress tracking components
  - [ ] 61.1 Create EmployeeProgressView component
    - Show individual employee progress over time
    - Display all ECDF versions
    - Show pillar trends
    - Call GET /api/manager/employees/{employeeId}/progress
    - _Requirements: 28.4, 28.5, 28.6, 28.7, 28.8, 28.9, 28.10_
  
  - [ ] 61.2 Create TeamPerformanceHeatmap component
    - Visualize team competency distribution
    - Color-coded by performance status
    - _Requirements: 29.3, 29.8_
  
  - [ ] 61.3 Create TeamTrendAnalysis component
    - Track team pillar score trends over time
    - _Requirements: 29.2, 29.8_

- [ ] 62. Checkpoint - Verify manager frontend features
  - Test team overview dashboard
  - Test evidence review workflow
  - Test employee progress tracking
  - Ask user if questions arise

### Phase 14: Frontend - Independent Assessor Features

- [ ] 63. Create independent assessor components
  - [ ] 63.1 Create AssessorDashboard component
    - Show assigned employees
    - Call GET /api/assessor/assignments
    - _Requirements: 11.1, 11.2_
  
  - [ ] 63.2 Create AssignedEmployeeList component
    - List of employees requiring assessment
    - Show pending evidence counts
    - _Requirements: 11.1, 11.2_
  
  - [ ] 63.3 Create AssessorEvidenceScoreForm component
    - Similar to employee/manager score form but for assessor
    - Call POST /api/evidence/{evidenceId}/scores
    - _Requirements: 7.1, 7.2, 7.3, 7.7, 7.8_
  
  - [ ] 63.4 Create AssessorPendingEvidenceList component
    - Show evidence needing assessor evaluation
    - Call GET /api/assessor/employees/{employeeId}/evidence/pending
    - _Requirements: 11.3, 11.4, 11.5_

- [ ] 64. Checkpoint - Verify assessor frontend features
  - Test assessor dashboard
  - Test evidence assessment workflow
  - Ask user if questions arise

### Phase 15: Data Security and Encryption

- [ ] 65. Implement data encryption
  - [ ] 65.1 Write test for evidence data encryption
    - Test evidence fields are encrypted before storage
    - Test decryption on retrieval
    - _Requirements: 17.1_
  
  - [ ] 65.2 Implement encryption service
    - Use AES-256 encryption
    - Encrypt description, impact, complexity, personalContribution
    - _Requirements: 17.1_
  
  - [ ] 65.3 Write test for development plan encryption
    - Test goal descriptions and actions are encrypted
    - _Requirements: 17.1_
  
  - [ ] 65.4 Add encryption to development plan data
    - Encrypt sensitive fields
    - _Requirements: 17.1_

- [ ] 66. Verify access control and security
  - [ ] 66.1 Write integration tests for access control
    - Test employees can only access own data
    - Test managers can only access assigned employees
    - Test assessors can only access assigned employees
    - _Requirements: 17.2, 17.3, 18.1, 18.2, 18.3_
  
  - [ ] 66.2 Write test for HTTPS transmission
    - Verify all API calls use HTTPS in production
    - _Requirements: 17.6_
  
  - [ ] 66.3 Write test for audit logging
    - Test access to evidence and scores is logged
    - _Requirements: 17.7_
  
  - [ ] 66.4 Implement audit logging
    - Log all data access with user ID, timestamp, resource
    - _Requirements: 17.7_

- [ ] 67. Checkpoint - Verify security features
  - Run all security tests
  - Test encryption/decryption
  - Verify access control across all endpoints
  - Ask user if questions arise

### Phase 16: Integration Testing and Polish

- [ ] 68. Create end-to-end integration tests
  - [ ] 68.1 Test complete employee workflow
    - Login → Create evidence → Score evidence → Create ECDF assessment → View progress
    - _Requirements: All employee requirements_
  
  - [ ] 68.2 Test complete manager workflow
    - Login → View team → Review evidence → Score evidence → View employee progress
    - _Requirements: All manager requirements_
  
  - [ ] 68.3 Test complete assessor workflow
    - Login → View assignments → Score evidence
    - _Requirements: All assessor requirements_
  
  - [ ] 68.4 Test multi-role assessment workflow
    - Employee scores → Manager scores → Assessor scores → ECDF assessment aggregates all
    - _Requirements: 5, 6, 7, 8, 9_
  
  - [ ] 68.5 Test versioning and temporal features
    - Create evidence → Score → Create ECDF v1 → Add more evidence → Score → Create ECDF v2 → Compare
    - _Requirements: 24, 25, 26, 27_

- [ ] 69. Performance optimization
  - [ ] 69.1 Add database query optimization
    - Review and optimize N+1 queries
    - Add appropriate indexes
    - Use eager loading where needed
  
  - [ ] 69.2 Add caching for expected scores
    - Cache expected scores in Redis or in-memory
    - _Requirements: 20.5_
  
  - [ ] 69.3 Optimize frontend rendering
    - Add React.memo for expensive components
    - Implement pagination for large lists
    - Add loading states

- [ ] 70. Final polish and documentation
  - [ ] 70.1 Add API documentation
    - Generate OpenAPI/Swagger documentation
    - Document all endpoints
  
  - [ ] 70.2 Add user documentation
    - Create user guide for each role
    - Document workflows
  
  - [ ] 70.3 Add developer documentation
    - Document architecture decisions
    - Document setup instructions
    - Document testing approach
  
  - [ ] 70.4 Code cleanup and refactoring
    - Remove unused code
    - Improve code organization
    - Add missing comments

- [ ] 71. Final checkpoint - System verification
  - Run all tests (unit, integration, end-to-end)
  - Verify all 29 requirements are implemented
  - Test with PostgreSQL database
  - Perform security review
  - Ask user for final feedback

## Notes

- All tasks follow TDD approach: write test first, then implementation
- Tasks are small and incremental to enable steady progress
- Each task references specific requirements for traceability
- Checkpoints ensure validation at key milestones
- Focus on class behavior testing rather than property-based testing
- Multi-role assessment is a core feature throughout
- Versioning (evidence scores and ECDF assessments) is critical for temporal tracking
- Access control must be verified at every layer
