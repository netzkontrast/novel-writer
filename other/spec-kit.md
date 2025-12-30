# Spec-Kit Practical Programming Guide: Spec-Driven Development (SDD) Methodology

> A hands-on tutorial for Spec-Kit aimed at Chinese developers
> Building an "Intelligent Classroom" online education platform from scratch

---

## Zero. Getting Started with Spec-Kit

### Installing the Specify CLI Tool

Spec-Kit provides the `specify` command-line tool to quickly initialize projects. Choose either of the following installation methods:

#### Method 1: Persistent Installation (Recommended)

Install once, use anywhere:

```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

After installation, you can use it directly:

```bash
specify init <project-name>
specify check
```

To upgrade the tool:

```bash
uv tool install specify-cli --force --from git+https://github.com/github/spec-kit.git
```

#### Method 2: One-time Use

Run directly without installation:

```bash
uvx --from git+https://github.com/github/spec-kit.git specify init <project-name>
```

### Initializing a Project

Use the `specify init` command to initialize a Spec-Kit project:

```bash
# Create a new project
specify init my-project

# Specify an AI agent
specify init my-project --ai claude

# Initialize in the current directory
specify init . --ai claude
# Or use the --here flag
specify init --here --ai claude

# Force merge into a non-empty directory (skip confirmation)
specify init . --force --ai claude
```

### Supported AI Agents

| AI Agent | Support Status | Notes |
|---------|---------|------|
| [Claude Code](https://www.anthropic.com/claude-code) | ✅ | |
| [GitHub Copilot](https://code.visualstudio.com/) | ✅ | |
| [Gemini CLI](https://github.com/google-gemini/gemini-cli) | ✅ | |
| [Cursor](https://cursor.sh/) | ✅ | |
| [Windsurf](https://windsurf.com/) | ✅ | |
| [Qwen Code](https://github.com/QwenLM/qwen-code) | ✅ | |
| [opencode](https://opencode.ai/) | ✅ | |
| [Roo Code](https://roocode.com/) | ✅ | |

### Prerequisites

- **Operating System**: Linux/macOS (or Windows WSL2)
- **AI Agent**: Any of the supported AI programming tools listed above
- **Python**: 3.11+
- **Package Manager**: [uv](https://docs.astral.sh/uv/)
- **Version Control**: [Git](https://git-scm.com/downloads)

---

## One. Core Philosophy: The Essence of Spec-Driven Development

### What is Spec-Driven Development?

**SDD (Spec-Driven Development)** is a revolutionary software development methodology. Its core idea is:

> **Specs no longer serve the code; the code serves the specs.**

**The dilemma of traditional software development**:
```
Requirements Document (Word) → Design Document (Visio) → Coding → Testing → Deployment
    ↓               ↓                ↓      ↓
  Outdated         Doesn't match code      The truth   Remediation
```
Result: Documentation always lags, and in the end, "the code is the documentation."

**The cycle of SDD-driven development**:
```
Project Constitution → Feature Spec → Clarification & Decisions → Technical Plan → Task Breakdown
                                              ↓
                                          Quality Gate Check
                                              ↓
                                          Automated Code Implementation
  ↑                                           ↓
  └────── Discover issues, return to the appropriate level to modify the spec ──────┘
```

### Why Do We Need SDD Now?

**Three major trends make SDD inevitable:**

1. **Breakthroughs in AI capabilities**: LLMs like GPT-4/Claude can understand natural language specs and generate usable code.
2. **Complexity explosion**: Modern systems integrate dozens of components like Redis, MQ, and microservices, making it increasingly difficult to maintain consistency manually.
3. **Accelerated pace of requirement changes**: In the normal state of agile development, requirements change weekly. With traditional methods, changing requirements is a disaster; with SDD, you just need to update the spec and regenerate.

**The core insight of SDD**:

Traditional development: Code is the truth, documentation is an accessory → Code constantly evolves, documentation gradually becomes obsolete.

SDD inverts this relationship: **The spec is the truth, and the code is its expression in Java/Python/Go.**
- Maintaining software = Evolving the spec
- Debugging BUGs = Fixing logical errors in the spec
- Refactoring = Reorganizing the structure of the spec

### SDD is Not a Linear Process, but a Recursive Cycle

**❌ Misunderstanding: A one-time linear execution**
```
Entire project: Constitution → Spec → Plan → Tasks → Implementation (manually writing 100 features)
                                      ↑
                              Stuck here, coding manually
```

**✅ Correct understanding: Layered recursion**
```
Layer 1 - Entire project:   Constitution → Spec (Architecture) → Technology Selection
           ↓
Layer 2 - Single feature:   Spec → Clarification → Plan → Tasks → Quality Gate → Implementation
           ↓
Layer 3 - Feature module:   Plan → Tasks → Implementation
           ↓
Layer 4 - Single task:   Implementation
```

**Each level can execute a complete SDD cycle!** This is the essence of the methodology.

---

## Two. The Seven-Step Method Explained

### Core Command Flow

**Command Format Explanation**:
- **Full format**: `/speckit.xxx` - The formal way, clearly namespacing the command.
- **Shorthand format**: `/xxx` - A convenient shorthand that executes the same command.
- For brevity, most examples in this document use the shorthand format.

SDD provides the following core commands to implement the complete development flow:

```
1. /speckit.constitution (or /constitution)  →  Project Constitution (highest principles, defines the project's DNA)
2. /speckit.specify (or /specify)            →  Feature Spec (single source of truth, defines what and why)
3. /speckit.clarify (or /clarify)            →  Clarification & Decisions (eliminates ambiguity, marks points to be confirmed) [Optional, recommended]
4. /speckit.plan (or /plan)                  →  Technical Plan (defines how, selects the tech stack)
5. /speckit.tasks (or /tasks)                →  Task Breakdown (executable steps, notes parallelism)
6. /speckit.analyze (or /analyze)            →  Quality Gate (consistency verification, CRITICAL issues must be fixed) [Optional, recommended]
7. /speckit.implement (or /implement)        →  Execution & Implementation (generates code in a TDD fashion)
```

**Optional Enhancement Command:**
- `/speckit.checklist` (or `/checklist`) - Generates a domain-specific quality checklist.
  - **When to use**: To generate in-depth checklists for high-risk areas (security/performance/compliance/UX).
  - **Execution time**: After `/plan` or `/tasks`, as a domain-specific supplement to `/analyze`.
  - **Difference from `analyze`**: `analyze` is a general automated check, while `checklist` is a targeted check from a domain expert's perspective.

### Core Responsibilities of Each Command

| Command | Input | Output | Key Constraints |
|------|------|------|---------|
| `/speckit.constitution` | Project vision | `Project_Constitution.md` | Defines non-negotiable principles |
| `/speckit.specify` | Feature description | `Feature_Spec.md` | Only write what/why, not how |
| `/speckit.clarify` | Spec document | Updated spec | Forces resolution of all points to be confirmed |
| `/speckit.plan` | Spec + tech choices | `Technical_Plan.md`+`Data_Model.md` | Must pass the constitution check |
| `/speckit.tasks` | Technical plan | `Task_List.md` | Notes dependencies and parallelism |
| `/speckit.analyze` | Spec+Plan+Tasks | Analysis report | **Read-only**, does not modify files |
| `/speckit.implement` | Task list | Code+Tests | Strict TDD: tests first, then implementation |
| `/speckit.checklist` | Spec document | Quality checklist | Verifies requirement completeness and consistency |

### The Quality Gate Mechanism of the Seven-Step Process

```
Constitution ──→ Spec ──→ Clarification ──→ Plan ──→ Tasks
         ↓         ↓        ↓        ↓
     [Mark Ambiguity] [Resolve Ambiguity] [Constitution Gate] [Task Gate]
                                     ↓
                                 Quality Gate ←── CRITICAL issues block
                                     ↓
                                   Implementation
```

**`/speckit.analyze` is the key quality gate**:
- **Mandatory run** after `/speckit.tasks` is complete and before `/speckit.implement`.
- Detects 6 types of issues: Duplication, Ambiguity, Coverage, Constitution Violations, Inconsistency, Terminology Drift.
- Levels: CRITICAL (must fix) / HIGH (recommended) / MEDIUM (improvement) / LOW (optional).
- **If there are CRITICAL issues, `/speckit.implement` is blocked.**

### How to Apply the Seven-Step Method at Each Level

| Level | Constitution | Spec | Clarify | Plan | Tasks | Quality Gate | Implement |
|------|------|------|------|------|------|--------|------|
| **Entire Project** | ✅ Once | ✅ High-level architecture | ✅ Core decisions | ✅ Technology selection | ✅ Feature list | ✅ Architecture validation | - |
| **Single Feature** | Inherited | ✅ Feature spec | ✅ Details | ✅ Implementation plan | ✅ Development tasks | ✅ **Mandatory** | ✅ Generate code |
| **Feature Module** | Inherited | Inherited | - | ✅ Module design | ✅ Sub-tasks | ✅ Optional | ✅ Module code |
| **Single Task** | Inherited | Inherited | - | - | - | - | ✅ Task code |

---

## Three. Practical Case Study: "Intelligent Classroom" Online Education Platform

### Project Background

- **Project Name**: Intelligent Classroom
- **Project Type**: K-12 Online Education Platform
- **Core Features**: Course Management, Homework Grading, Online Exams, Grade Analysis
- **Tech Stack**: Spring Boot 3.x + Vue 3 + MySQL 8.0 + Redis
- **Goal**: To demonstrate the complete SDD process from scratch to a runnable application.

---

## Scenario 1: Project Initialization - Defining the Project DNA with Constitution

### Step 1: Create the Project Constitution

```bash
/speckit.constitution Create project principles focusing on code quality, testing standards, education industry compliance, and performance requirements.
```

**Core content of the AI-generated constitution**:

```markdown
# Intelligent Classroom Project Constitution

## Article 1: Module-First Principle

Every feature must first be designed as an independent module. It is forbidden to directly implement business logic in the main application code.
It must first be abstracted into a reusable Service layer component.

**Reason**: To enforce modularity, reusability, and testability.

## Article 2: Clear Interface Principle

All Services must define clear interfaces:
- RESTful API: Uniformly return `Result<T>` wrapper.
- Error handling: `BusinessException` for business errors, uniform interception for system errors.
- Support JSON format, with camelCase field naming.

**Reason**: To ensure consistency between front-end and back-end interfaces, and to facilitate mock testing.

## Article 3: Test-First Principle (Non-negotiable)

It is forbidden to write implementation code before the following steps are completed:
1. The interface contract has been defined (OpenAPI specification).
2. Test cases have been written and passed review.
3. The tests have been run and confirmed to fail (TDD red light phase).

**Reason**: To prevent untestable code and ensure requirements are clear.

## Article 4: Data Security Principle (Education Industry Compliance)

Student personal information must be:
1. Stored encrypted for phone numbers/ID card numbers (AES-256).
2. Masked in logs (e.g., 135****1234).
3. Data export requires an approval process.
4. Comply with the "Personal Information Protection Law".

**Consequence of violation**: CRITICAL level, blocks deployment.

## Article 7: Simplicity Gate

- The initial version shall have at most 3 Maven modules: common, service, web.
- It is forbidden to introduce dependencies that "might be used in the future" (e.g., Elasticsearch, as MySQL is sufficient for now).
- YAGNI principle: You Aren't Gonna Need It.

## Article 8: Anti-Over-Abstraction Gate

- Use MyBatis-Plus directly; do not wrap it with another `BaseMapper` layer.
- Single model representation: Use Entity directly for Controller returns (no need to separate DTO/VO in a small project).

## Article 9: Integration Test Priority

- Use a real MySQL (docker-compose), not an H2 in-memory database.
- Controller tests should use `@SpringBootTest` for a full startup.
- Test data should be managed with SQL scripts (`src/test/resources/data.sql`).
```

**Why is the constitution important?**

The constitution is the project's "DNA," defining **non-negotiable principles**. All subsequent plans generated by `/speckit.plan` must pass these checks; otherwise, `/speckit.analyze` will report a CRITICAL error, preventing implementation.

---

## Scenario 2: Complete Feature Development Flow - From Spec to Implementation

### Step 2: Define the Feature Spec (Only write what, not how)

```bash
/speckit.specify Develop a "Homework Grading" feature. Teachers can publish homework (questions + deadline + attachments). Students can submit homework (supporting image/PDF uploads). Teachers can grade online and give scores (0-100 points + text comments). Initial version: 3 teachers, 30 students, 2 classes. Homework statuses include: Not Submitted, Submitted, Graded, Revision Needed. Students can view grading results and history. Teachers can view class submission rates and average scores. Late submissions are marked in red. Access control: Students can only see their own homework, teachers can only see homework for the classes they teach.
```

**Output**: `specs/001-homework-grading/spec.md`

**Key content**:

```markdown
## Feature Overview

**What**: An online homework submission and grading system.
**Why**: To replace traditional paper-based homework collection and submission, improve grading efficiency, and retain learning records.

## Functional Requirements

### Requirement 1: Homework Publishing
Teachers can create homework, including the following information:
- Homework title (required, ≤50 characters)
- Homework content (required, rich text, supports image insertion)
- Deadline (required, cannot be earlier than the current time)
- Attachments (optional, supports PDF/Word, single file ≤10MB)

### Requirement 2: Student Homework Submission
Students can submit their homework answers:
- Supports image uploads (jpg/png, single image ≤5MB, max 9 images)
- Supports PDF uploads (≤10MB)
- Can modify submitted content before the deadline
- Late submissions must be marked (display a red "Late" tag)

### Requirement 3: Teacher Grading
Teachers can grade submitted homework:
- Scoring: 0-100 integer
- Comments: Text feedback (≤500 characters)
- Grading time is automatically recorded

### Requirement 4: Status Management
Homework has 4 statuses for students:
- Not Submitted: Student has not submitted, within the deadline
- Submitted: Student has submitted, awaiting grading
- Graded: Teacher has graded and given a score
- Revision Needed: Teacher has requested the student to resubmit

### Requirement 5: Statistical Functions
Teachers can view class homework statistics:
- Submission rate: Number of submitted / Total number of students in the class
- Average score: Average score of graded homework
- Score distribution: Number of students in each score range

### Requirement 6: Access Control
- Students: Can only see their own submitted homework and grading results.
- Teachers: Can only see all student homework for the classes they teach.

## Non-Functional Requirements

### Performance Requirements
- Homework list loading time < 1 second (for up to 100 assignments)
- File upload speed: 5MB file completes within 10 seconds

### Data Security
- Student names and student IDs must comply with the "Personal Information Protection Law" and be masked in logs.
- Homework content and grading records are retained for 1 year.

### Points to be Confirmed
- [To be confirmed] Can students still submit after the deadline? Or is it locked?
  - Option A: Allow late submission, but mark as "Late". The teacher can deduct points at their discretion.
  - Option B: Prohibit submission after the deadline.

- [To be confirmed] Can students appeal after grading?
  - Option A: No appeals supported. The teacher's grade is final.
  - Option B: Students can initiate an appeal, and the teacher can re-grade.

## User Stories

### User Story 1: Teacher Publishes Homework
**As** a math teacher, Mr. Zhang
**I want to** create a "Chapter 3 Exercises" assignment in the system
**So that** students can submit it online and I can grade it centrally.

**Acceptance Criteria**:
- Enter title, content, and deadline (2024-06-30 23:59).
- Upload an attachment "Exercises.pdf".
- After clicking "Publish," students in 2 classes (Grade 7 Class 1, Grade 7 Class 2) can see it.

### User Story 2: Student Submits Homework
**As** a student in Grade 7 Class 1, Xiao Ming
**I want to** upload photos of my homework answers
**So that** the teacher can grade it.

**Acceptance Criteria**:
- Take 3 photos of the homework and upload them successfully.
- After submission, the status changes to "Submitted".
- If submitted after the deadline (2024-06-30 23:59), a red "Late" tag is displayed.

### User Story 3: Teacher Grades Homework
**As** Mr. Zhang
**I want to** grade Xiao Ming's homework
**So that** I can give a score and feedback.

**Acceptance Criteria**:
- View the 3 photos uploaded by Xiao Ming.
- Give a score: 85 points.
- Add a comment: "The calculation process is clear, but you made a careless mistake in question 3."
- After saving, Xiao Ming can see the grading result.

## Edge Cases

### Edge Case 1: Submitting exactly at the deadline
A student submits at 2024-06-30 23:59:58. Is it considered late?
**Decision**: Submissions within the deadline minute (≤23:59:59) are considered on time.

### Edge Case 2: File upload fails
A student uploads a 5MB image, and the network times out.
**Handling**: The front-end displays "Upload failed, please try again." Other already uploaded images are retained.

### Edge Case 3: Teacher grades incorrectly
The teacher gives Xiao Ming a score of 58 but realizes it was a mistake and should be 85.
**Handling**: The teacher can re-grade, overwriting the previous score and comments.
```

**Note**:
- ✅ Focuses on what (functional requirements) and why (business goals).
- ✅ Marks 2 points to be confirmed, awaiting resolution by `/speckit.clarify`.
- ❌ Does not mention technical details like MySQL, Redis, MyBatis, etc.

### Step 3: Clarify Points to be Confirmed

```bash
/speckit.clarify
```

**AI automatically analyzes and asks questions**:

```markdown
## Questions to be Confirmed

### Question 1: Late Submission Policy (related to Requirement 2)
The current spec is unclear. Possible options:

**Option A: Allow late submission (Recommended)**
- Pros: Flexible, teachers can give points at their discretion.
- Cons: May be abused.
- Implementation: Add an `is_late` field to the database to mark it.

**Option B: Prohibit submission after the deadline**
- Pros: Strict rules, cultivates a sense of time.
- Cons: Students who forget the deadline will complain.
- Implementation: The Controller checks the time and returns 403 on timeout.

**Recommendation**: Choose Option A for an educational setting, giving students a chance to correct themselves.

User's choice: Option A
```

**AI updates the spec document**:

```markdown
### Requirement 2: Student Homework Submission (Updated)

Students can submit their homework answers:
- Supports image uploads (jpg/png, single image ≤5MB, max 9 images)
- Supports PDF uploads (≤10MB)
- Can modify submitted content before the deadline
- **Late submission policy**: Late submissions are allowed, but the front-end displays a red "Late" tag, and the teacher can deduct points at their discretion.
```

**Question 2: Appeal Mechanism after Grading**

```markdown
**Option A: No appeals supported (Recommended for MVP)**
- Pros: Simple, reduces disputes.
- Cons: How to correct if the teacher makes a grading error?
- Implementation: No extra table design needed.

**Option B: Support an appeal process**
- Pros: Fair, allows for corrections.
- Cons: Complex, requires an appeal table and notification mechanism.
- Implementation: Requires a `homework_appeal` table + message push.

**Recommendation**: Choose Option A for the initial version, but allow teachers to modify their own grading results (equivalent to a simplified appeal).

User's choice: Option A, but allow teachers to modify.
```

**Clarification complete, all points to be confirmed have been resolved.**

### Step 4: Formulate the Technical Plan (start getting into the how)

```bash
/speckit.plan Use Spring Boot 3.2 + MyBatis-Plus + MySQL 8.0 + Redis as a cache. File uploads use Aliyun OSS. Statistical data is cached in Redis for 5 minutes. The front-end is Vue 3 + Element Plus, with viewerjs for image previews.
```

**Output**: `specs/001-homework-grading/plan.md`, `data-model.md`, `contracts/api-spec.yaml`

**Key content of the technical plan**:

```markdown
## Technical Architecture

### Technology Selection
- **Backend Framework**: Spring Boot 3.2.5
- **ORM**: MyBatis-Plus 3.5.6 (simplifies CRUD)
- **Database**: MySQL 8.0
- **Cache**: Redis 7.0 (caches class statistics)
- **File Storage**: Aliyun OSS (stores homework attachments and student-submitted images)
- **Frontend**: Vue 3.3 + Element Plus + Axios
- **Image Preview**: viewerjs

### Phase 0: Pre-implementation Check (Constitution Gate)

#### Simplicity Gate (Article 7)
- [✅] Use ≤3 Maven modules?
  Yes: homework-common, homework-service, homework-web
- [✅] No future-oriented features?
  Yes: Not introducing Elasticsearch (MySQL is sufficient for now)

#### Anti-Over-Abstraction Gate (Article 8)
- [✅] Use the framework directly?
  Yes: Use MyBatis-Plus `BaseMapper`, no further wrapping.
- [⚠️] Single model representation?
  No: Separate Entity and VO (Reason: Entity contains sensitive fields like `student_id_card`, VO needs masking).
  **Complexity tracking**: Recorded, user approval needed.

#### Test-First Gate (Article 3)
- [✅] Contract defined?
  Yes: See `contracts/api-spec.yaml`
- [⏳] Contract tests written?
  To be done: In the `/speckit.tasks` phase.

### Core Implementation Phases

#### Phase 1: Data Layer Setup
1. Create MySQL tables: `homework`, `homework_submission`, `homework_review`.
2. Implement Entities: `HomeworkEntity`, `HomeworkSubmissionEntity`, `HomeworkReviewEntity`.
3. MyBatis-Plus Mappers: `HomeworkMapper`, `SubmissionMapper`, `ReviewMapper`.
4. Data initialization: 3 teachers, 30 students, 2 classes.

#### Phase 2: Business Layer Implementation
1. `HomeworkService`: Publish homework, query homework list.
2. `SubmissionService`: Submit homework, query submission records, check for lateness.
3. `ReviewService`: Grade homework, modify grades.
4. `StatisticsService`: Calculate submission rate, average score (cached in Redis for 5 minutes).

#### Phase 3: Interface Layer Implementation
1. `HomeworkController`: `POST /api/homework` (publish), `GET /api/homework/list` (list).
2. `SubmissionController`: `POST /api/submission` (submit), `GET /api/submission/:id` (details).
3. `ReviewController`: `POST /api/review` (grade), `PUT /api/review/:id` (modify).
4. `FileController`: `POST /api/file/upload` (OSS upload).

#### Phase 4: Frontend Pages
1. Teacher side: Homework publishing page, grading page, statistics page.
2. Student side: Homework list page, submission page, result viewing page.
3. Common components: File upload component, image preview component.
```

**Data Model (`data-model.md`)**:

```markdown
## Database Table Design

### Homework Table (homework)
```sql
CREATE TABLE homework (
  id BIGINT PRIMARY KEY AUTO_INCREMENT COMMENT 'Homework ID',
  title VARCHAR(50) NOT NULL COMMENT 'Homework title',
  content TEXT NOT NULL COMMENT 'Homework content (rich text)',
  teacher_id BIGINT NOT NULL COMMENT 'ID of the publishing teacher',
  class_ids VARCHAR(200) NOT NULL COMMENT 'List of class IDs (JSON array)',
  deadline DATETIME NOT NULL COMMENT 'Deadline',
  attachment_url VARCHAR(500) COMMENT 'Attachment OSS address',
  created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
  updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  INDEX idx_teacher_id (teacher_id),
  INDEX idx_deadline (deadline)
) COMMENT='Homework table';
```

### Submission Table (homework_submission)
```sql
CREATE TABLE homework_submission (
  id BIGINT PRIMARY KEY AUTO_INCREMENT,
  homework_id BIGINT NOT NULL COMMENT 'Homework ID',
  student_id BIGINT NOT NULL COMMENT 'Student ID',
  content TEXT COMMENT 'Submitted content (text)',
  image_urls TEXT COMMENT 'List of image addresses (JSON array)',
  pdf_url VARCHAR(500) COMMENT 'PDF address',
  status TINYINT NOT NULL DEFAULT 1 COMMENT '1 Not Graded 2 Graded 3 Revision Needed',
  is_late TINYINT NOT NULL DEFAULT 0 COMMENT '0 On time 1 Late',
  submitted_at DATETIME DEFAULT CURRENT_TIMESTAMP COMMENT 'Submission time',
  updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  UNIQUE KEY uk_homework_student (homework_id, student_id),
  INDEX idx_student_id (student_id),
  INDEX idx_status (status)
) COMMENT='Homework submission table';
```

### Review Table (homework_review)
```sql
CREATE TABLE homework_review (
  id BIGINT PRIMARY KEY AUTO_INCREMENT,
  submission_id BIGINT NOT NULL COMMENT 'Submission ID',
  teacher_id BIGINT NOT NULL COMMENT 'ID of the grading teacher',
  score TINYINT NOT NULL COMMENT 'Score 0-100',
  comment VARCHAR(500) COMMENT 'Comments',
  reviewed_at DATETIME DEFAULT CURRENT_TIMESTAMP COMMENT 'Grading time',
  updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  UNIQUE KEY uk_submission (submission_id),
  INDEX idx_teacher_id (teacher_id)
) COMMENT='Review table';
```
```

**API Contract (`contracts/api-spec.yaml`)**:

```yaml
openapi: 3.0.0
info:
  title: Intelligent Classroom - Homework Grading API
  version: 1.0.0

paths:
  /api/homework:
    post:
      summary: Publish homework
      requestBody:
        content:
          application/json:
            schema:
              type: object
              required: [title, content, classIds, deadline]
              properties:
                title:
                  type: string
                  maxLength: 50
                  example: "Chapter 3 Exercises"
                content:
                  type: string
                  example: "<p>Complete exercises on pages 45-50</p>"
                classIds:
                  type: array
                  items:
                    type: integer
                  example: [1, 2]
                deadline:
                  type: string
                  format: date-time
                  example: "2024-06-30T23:59:59"
                attachmentUrl:
                  type: string
                  example: "https://oss.example.com/homework/xxx.pdf"
      responses:
        '200':
          description: Published successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Result'

  /api/submission:
    post:
      summary: Submit homework
      requestBody:
        content:
          application/json:
            schema:
              type: object
              required: [homeworkId]
              properties:
                homeworkId:
                  type: integer
                  example: 1
                content:
                  type: string
                  example: "My answer is..."
                imageUrls:
                  type: array
                  items:
                    type: string
                  example: ["https://oss.example.com/sub/1.jpg", "https://oss.example.com/sub/2.jpg"]
                pdfUrl:
                  type: string
      responses:
        '200':
          description: Submitted successfully

  /api/review:
    post:
      summary: Grade homework
      requestBody:
        content:
          application/json:
            schema:
              type: object
              required: [submissionId, score]
              properties:
                submissionId:
                  type: integer
                score:
                  type: integer
                  minimum: 0
                  maximum: 100
                  example: 85
                comment:
                  type: string
                  maxLength: 500
                  example: "The calculation process is clear, but you made a careless mistake in question 3."
      responses:
        '200':
          description: Graded successfully

components:
  schemas:
    Result:
      type: object
      properties:
        code:
          type: integer
          example: 200
        message:
          type: string
          example: "Operation successful"
        data:
          type: object
```

### Step 5: Decompose into Executable Tasks

```bash
/speckit.tasks
```

**Output**: `specs/001-homework-grading/tasks.md`

```markdown
## Task Breakdown

### Phase 0: Scaffolding and Contract

#### Task 001 [Parallelizable] - Create Project Structure
- Initialize Spring Boot project (maven)
- Create 3 modules: common, service, web
- Configure `pom.xml` dependencies (MyBatis-Plus, Redis, OSS SDK)
- **Deliverable**: The project compiles successfully.

#### Task 002 [Parallelizable] - Define API Contract
- Copy `contracts/api-spec.yaml` to the project
- Use `swagger-codegen` to generate Java DTOs
- **Deliverable**: DTO classes can be referenced by Controllers.

#### Task 003 [Depends on: Task 002] - Write Contract Tests
- Test: `POST /api/homework` returns 200
- Test: `POST /api/submission` without `homeworkId` returns 400
- Test: `POST /api/review` with `score=101` returns 400
- **Deliverable**: All tests fail (TDD red light phase).

### Phase 1: Data Layer (Parallelizable Group A)

#### Task 004 [Parallelizable] [Depends on: Task 003] - Set up MySQL
- Start MySQL 8.0 with `docker-compose`
- Create the database `classroom_db`
- Execute DDL scripts (homework, homework_submission, homework_review)
- **Deliverable**: Table structures are created.

#### Task 005 [Parallelizable] [Depends on: Task 003] - Initialize Seed Data
- Insert 3 teachers: Mr. Zhang (Math), Ms. Li (Language), Mr. Wang (English)
- Insert 30 students: Student IDs 1001-1030, assigned to 2 classes
- Insert 2 classes: Grade 7 Class 1, Grade 7 Class 2
- **Deliverable**: Seed data can be queried.

### Phase 2: Business Layer Implementation (Depends on: Phase 1)

#### Task 006 [Depends on: Task 004] - Implement HomeworkService
- Create `HomeworkMapper` (MyBatis-Plus)
- Implement homework publishing: `publishHomework(HomeworkDTO dto)`
- Implement list query: `listHomework(Long teacherId)`
- **Deliverable**: The `POST /api/homework` test from Task 003 passes (turns green).

#### Task 007 [Depends on: Task 004] - Implement SubmissionService
- Create `SubmissionMapper`
- Implement homework submission: `submitHomework(SubmissionDTO dto)`
- Implement late check: `isLate(Long homeworkId, LocalDateTime submitTime)`
- Implement query: `getSubmission(Long submissionId)`
- **Deliverable**: The `POST /api/submission` test passes.

#### Task 008 [Depends on: Task 004] - Implement ReviewService
- Create `ReviewMapper`
- Implement grading: `reviewHomework(ReviewDTO dto)`
- Implement grade modification: `updateReview(Long reviewId, ReviewDTO dto)`
- **Deliverable**: The `POST /api/review` test passes.

### Phase 3: Interface Layer (Parallelizable Group B)

#### Task 009 [Parallelizable] [Depends on: Task 006] - HomeworkController
- Create Controller, inject `HomeworkService`
- Implement `POST /api/homework`
- Implement `GET /api/homework/list?teacherId=1`
- Permission check: Only teachers can publish
- **Deliverable**: Postman tests pass.

#### Task 010 [Parallelizable] [Depends on: Task 007] - SubmissionController
- Implement `POST /api/submission`
- Implement `GET /api/submission/:id`
- Permission check: Students can only query their own submissions
- **Deliverable**: Can be called from the frontend.

#### Task 011 [Depends on: Task 008] - ReviewController
- Implement `POST /api/review`
- Implement `PUT /api/review/:id` (modify grade)
- Permission check: Teachers can only grade homework of students in their own classes
- **Deliverable**: The complete grading flow is connected.

#### Task 012 [Parallelizable] - FileController for File Uploads
- Integrate Aliyun OSS SDK
- Implement `POST /api/file/upload` (supports images/PDFs)
- File size validation: images ≤5MB, PDFs ≤10MB
- Return OSS address
- **Deliverable**: The frontend can upload files and get a URL.

#### Task 013 [Depends on: Task 007] - StatisticsService for Statistical Functions
- Implement submission rate calculation: `calcSubmitRate(Long homeworkId)`
- Implement average score calculation: `calcAvgScore(Long homeworkId)`
- Cache in Redis for 5 minutes (key: `stats:homework:{id}`)
- **Deliverable**: `GET /api/statistics/:homeworkId` returns statistical data.

### Phase 4: Frontend Pages

#### Task 014 [Depends on: Task 009] - Teacher Homework Publishing Page
- Vue component: `HomeworkPublish.vue`
- Form: Title, rich text editor, deadline picker
- Call `POST /api/homework`
- **Deliverable**: Teachers can publish homework.

#### Task 015 [Depends on: Task 010, Task 012] - Student Homework Submission Page
- Vue component: `HomeworkSubmit.vue`
- Image upload component (max 9 images)
- PDF upload component
- Late submission indicator (red "Late" tag)
- **Deliverable**: Students can submit homework.

#### Task 016 [Depends on: Task 011] - Teacher Grading Page
- Vue component: `HomeworkReview.vue`
- Image preview (`viewerjs`)
- Score input box (0-100)
- Comment text box
- **Deliverable**: Teachers can grade homework.

### Phase 5: Integration Testing and Deployment Preparation

#### Task 017 - End-to-End Testing
- Scenario 1: Mr. Zhang publishes homework → Xiao Ming submits (on time) → Mr. Zhang grades it 85 points.
- Scenario 2: Xiao Hong submits late → A red tag is displayed.
- Scenario 3: Mr. Zhang modifies Xiao Ming's score to 90 points.
- **Deliverable**: All scenarios pass.

#### Task 018 - Performance Validation
- Load a list of 100 assignments, test response time (target <1 second).
- Upload a 5MB file, test upload time (target <10 seconds).
- **Deliverable**: Non-functional requirements are met.

## Execution Plan

### Week 1
- Parallel group: Task 001, Task 002, Task 004, Task 005
- Serial: Task 003 (after contract tests are written)

### Week 2
- Serial: Task 006 → Task 007 → Task 008 (business layer)
- Start in parallel: Task 012 (file upload, independent)

### Week 3
- Parallel group B: Task 009, Task 010 (Controller layer)
- Serial: Task 011, Task 013

### Week 4
- Frontend: Task 014 → Task 015 → Task 016
- Finally: Task 017, Task 018 (testing and validation)

## Statistical Metrics
- Total number of tasks: 18
- Estimated duration: 4 weeks
- Number of parallelizable tasks: 7
- Critical path: Task 002 → Task 003 → Task 004 → Task 006 → Task 009 → Task 014 → Task 017
```

### Step 6: Run the Quality Gate Check (Crucial!)

```bash
/speckit.analyze
```

**Checks performed by the AI**:

```markdown
# Spec Analysis Report

## Executive Summary
- Total functional requirements: 6
- Total non-functional requirements: 2
- Total number of tasks: 18
- Coverage: 100% (8/8 requirements have corresponding tasks)
- **CRITICAL issues: 1**
- HIGH issues: 2
- MEDIUM issues: 3
- LOW issues: 1

## ❌ CRITICAL Issues (Must be fixed before /implement)

| ID | Category | Severity | Location | Issue Description | Recommendation |
|----|------|--------|------|----------|------|
| C1 | Constitution Violation | CRITICAL | tasks.md Phase 2 | Violates Article 3 "Test-First Principle": Task 006 "Implement HomeworkService" is executed before Task 003 "Contract Tests". | Adjust dependencies: Task 003 must be before Tasks 004-005 to ensure tests fail before implementation. |

## ⚠️ HIGH Issues (Strongly recommended to fix)

| ID | Category | Severity | Location | Issue Description | Recommendation |
|----|------|--------|------|----------|------|
| H1 | Coverage Gap | HIGH | spec.md Requirement 4 | The "Revision Needed" status has no corresponding task. | Add a `setNeedRevision()` method in `ReviewService`. |
| H2 | Ambiguity | HIGH | plan.md Phase 3 | "Permission check" does not specify what error code to return on failure. | Specify: Return 403 with the message "Permission denied" for no permission. |

## 📊 Coverage Details

| Requirement | Task? | Task ID | Notes |
|------|--------|--------|------|
| Req 1: Publish Homework | ✅ | Task 006, Task 009 | Fully covered |
| Req 2: Student Submission | ✅ | Task 007, Task 010 | Fully covered |
| Req 3: Teacher Grading | ✅ | Task 008, Task 011 | Fully covered |
| Req 4: Status Management | ⚠️ | Task 007 (partial) | Missing handling for "Revision Needed" status, see H1. |
| Req 5: Statistical Functions | ✅ | Task 013 | Fully covered |
| Req 6: Access Control | ✅ | Tasks 009-011 | In every Controller |
| Non-func 1: Performance | ✅ | Task 018 | Fully covered |
| Non-func 2: Data Security | ✅ | Automatically enforced by Constitution Article 4 | Fully covered |

## 🚨 Constitution Alignment Issues

### Article 3 Violation (CRITICAL)
**Location**: tasks.md Phase 2
**Issue**: Task 006 "Implement HomeworkService" depends on Task 004 (database), but it should depend on Task 003 (contract tests failing).
**Constitution text**: "It is forbidden to write implementation code before tests are confirmed to fail."
**Fix**: Adjust the task dependency order to Task 003 → Task 004 → Task 006.

## 📈 Statistical Metrics
- Requirement coverage: 100% (8/8)
- Task mapping rate: 94% (17/18, Task 001 is basic setup)
- Ambiguity count: 1 (H2)
- Duplication count: 0
- CRITICAL issues: 1

## ⚙️ Next Actions

### ❌ Blocked: Cannot execute /implement
**Reason**: 1 CRITICAL issue (Constitution violation)

### Must be fixed:
1. **Fix C1**: Adjust the task dependency order in `tasks.md`.
   - Change: The dependency for Task 006 from `[Depends on: Task 004]` to `[Depends on: Task 003]`.
   - Reason: Contract tests must fail first before implementation can begin.

2. **Resolve H1**: Add handling for the "Revision Needed" status.
   - Add a sub-task in Task 008: "Implement `setNeedRevision()` method."

### After fixing:
Rerun `/speckit.analyze` to confirm CRITICAL=0, then execute `/speckit.implement`.
```

**User's response**:

```
Fix C1 and H1
```

**AI automatically fixes `tasks.md`**:

```markdown
#### Task 003 [Depends on: Task 002] - Write Contract Tests
(Content unchanged)

#### Task 004 [Depends on: Task 003] - Set up MySQL  ← Modified: Added dependency on Task 003
- Start MySQL 8.0 with `docker-compose`
- ...

#### Task 006 [Depends on: Task 004, Task 003] - Implement HomeworkService  ← Modified: Explicit dependency on 003
- ...

#### Task 008 [Depends on: Task 004] - Implement ReviewService
- Create `ReviewMapper`
- Implement grading: `reviewHomework(ReviewDTO dto)`
- Implement grade modification: `updateReview(Long reviewId, ReviewDTO dto)`
- **New: Implement setting revision needed: `setNeedRevision(Long submissionId, String reason)`**  ← New
- **Deliverable**: The `POST /api/review` test passes.
```

**Rerunning `/speckit.analyze`**:

```bash
/speckit.analyze
```

```markdown
# Spec Analysis Report

## Executive Summary
- **CRITICAL issues: 0** ✅
- HIGH issues: 1 (H2 - error code standard)
- MEDIUM issues: 3
- LOW issues: 1

## ⚙️ Next Actions
✅ **Ready**: No CRITICAL issues. HIGH/MEDIUM issues are improvements but do not block implementation.

Can now execute: `/speckit.implement`
```

### Step 7: Execute Implementation (in a TDD fashion)

```bash
/speckit.implement
```

**Execution flow by the AI** (Strict TDD):

```
[Task 001] Creating project structure...
✅ Maven project created successfully, 3 modules compile.

[Task 002] Defining API contract...
✅ DTOs generated from `api-spec.yaml`: `HomeworkDTO`, `SubmissionDTO`, `ReviewDTO`.

[Task 003] Writing contract tests...
```java
@SpringBootTest
@AutoConfigureMockMvc
class HomeworkControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    void testPublishHomework_Success() throws Exception {
        HomeworkDTO dto = new HomeworkDTO()
            .setTitle("Chapter 3 Exercises")
            .setContent("<p>Complete pages 45-50</p>")
            .setClassIds(Arrays.asList(1L, 2L))
            .setDeadline(LocalDateTime.of(2024, 6, 30, 23, 59));

        mockMvc.perform(post("/api/homework")
                .contentType(MediaType.APPLICATION_JSON)
                .content(JsonUtil.toJson(dto)))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.code").value(200));
    }

    @Test
    void testPublishHomework_TitleIsEmpty_Returns400() throws Exception {
        HomeworkDTO dto = new HomeworkDTO()
            .setContent("Content")
            .setDeadline(LocalDateTime.now().plusDays(1));

        mockMvc.perform(post("/api/homework")
                .contentType(MediaType.APPLICATION_JSON)
                .content(JsonUtil.toJson(dto)))
            .andExpect(status().isBadRequest());
    }
}
```
❌ Tests fail (as expected) - Controller not yet implemented.

[Task 004] Setting up MySQL...
✅ Docker container started, table structures created.

[Task 005] Initializing seed data...
✅ Data for 3 teachers, 30 students, 2 classes inserted.

[Task 006] Implementing HomeworkService...
```java
@Service
public class HomeworkServiceImpl implements HomeworkService {

    @Autowired
    private HomeworkMapper homeworkMapper;

    @Override
    public Long publishHomework(HomeworkDTO dto) {
        HomeworkEntity entity = new HomeworkEntity();
        BeanUtils.copyProperties(dto, entity);
        entity.setCreatedAt(LocalDateTime.now());

        homeworkMapper.insert(entity);
        return entity.getId();
    }

    @Override
    public List<HomeworkVO> listHomework(Long teacherId) {
        return homeworkMapper.selectList(
            new QueryWrapper<HomeworkEntity>()
                .eq("teacher_id", teacherId)
                .orderByDesc("created_at")
        ).stream()
         .map(this::toVO)
         .collect(Collectors.toList());
    }
}
```

[Task 009] Implementing HomeworkController...
```java
@RestController
@RequestMapping("/api/homework")
public class HomeworkController {

    @Autowired
    private HomeworkService homeworkService;

    @PostMapping
    public Result<Long> publishHomework(@Validated @RequestBody HomeworkDTO dto,
                                        @RequestAttribute Long teacherId) {
        Long id = homeworkService.publishHomework(dto);
        return Result.success(id);
    }

    @GetMapping("/list")
    public Result<List<HomeworkVO>> listHomework(@RequestAttribute Long teacherId) {
        List<HomeworkVO> list = homeworkService.listHomework(teacherId);
        return Result.success(list);
    }
}
```
✅ The tests from Task 003 now pass - TDD cycle complete.

... (Similar TDD cycles for Tasks 007-018)

[Task 018] Performance validation...
✅ Loading a list of 100 assignments took: 0.8 seconds (target <1 second).
✅ Uploading a 5MB image took: 7 seconds (target <10 seconds).

🎉 All 18 tasks completed. The feature is ready for testing.
```

**Final deliverables**:
- ✅ A complete, runnable "Homework Grading" feature.
- ✅ All 18 tasks completed, all tests passed.
- ✅ Meets all constitution principles (modularity, test-first, data security).
- ✅ Frontend and backend are connected, teachers and students can actually use it.

---

## Scenario 3: Real Case of an Issue Found by /analyze

### Case: Data Security Violation Discovered

Suppose that when running `/speckit.analyze` in step 6, the following is found:

```markdown
| ID | Category | Severity | Location | Issue Description | Recommendation |
|----|------|--------|------|----------|------|
| C2 | Constitution Violation | CRITICAL | plan.md Data Model | Violates Article 4 "Data Security Principle": the `student_id_card` field is not encrypted. | Add a field `student_id_card_encrypted` to store the AES-encrypted data. |
```

**Remediation process**:

**Step 1: Determine the level of the issue**

This is an issue at the `/speckit.plan` level (data model design), so we need to go back to `/speckit.plan`.

**Step 2: Update `data-model.md`**

```markdown
### Student Table (student) - Fixed

```sql
CREATE TABLE student (
  id BIGINT PRIMARY KEY,
  name VARCHAR(50) NOT NULL,
  student_no VARCHAR(20) NOT NULL COMMENT 'Student ID number',
  -- Before change: id_card VARCHAR(18) COMMENT 'ID card number'
  id_card_encrypted VARCHAR(200) COMMENT 'ID card number (AES-256 encrypted)',  ← Fixed
  phone_encrypted VARCHAR(200) COMMENT 'Phone number (AES-256 encrypted)',     ← Fixed
  class_id BIGINT NOT NULL,
  ...
);
```
```

**Step 3: Update `tasks.md`**

```markdown
#### Task 005 - Initialize Seed Data (Updated)
- Insert data for 30 students
- **New: Encrypt ID card numbers and phone numbers with `AESUtil` before storing.**
- **New: Configure the encryption key (read from environment variables, not committed to Git).**
```

**Step 4: Rerun `/speckit.analyze`**

```bash
/analyze
```

```markdown
## Executive Summary
- **CRITICAL issues: 0** ✅
- Article 4 of the Constitution: Compliant.

✅ Ready, can execute `/speckit.implement`.
```

**Key takeaway**: `/speckit.analyze` acts like a compiler, catching security vulnerabilities before "runtime."

---

## Scenario 4: Rapid Response to Requirement Changes

### Change Request

The product manager proposes:

```
Requirement change: I want to support a "Showcase of Excellent Homework" feature. Teachers can mark homework with a score ≥90 as "excellent" and display it on the class bulletin board. Other students can view and learn from it (but without seeing the student's name, only "Excellent Homework").
```

**SDD handling process**:

**Step 1: Determine the level of the change**

| Change Type | Affected Level | Need to go back to |
|---------|---------|-----------|
| Change button color | Implementation detail | `/speckit.implement` |
| Add `is_excellent` field | Data model | `/speckit.plan` |
| Add "showcase feature" | Feature definition | `/speckit.specify` |
| Change project architecture | Project DNA | `/speckit.constitution` |

**Current change**: A new feature → Go back to `/speckit.specify`.

**Step 2: Update `spec.md`**

```markdown
### Requirement 7: Showcase of Excellent Homework (New)

Teachers can mark homework with a score ≥90 as "excellent homework" and display it on the class bulletin board.

**Feature details**:
- When grading, teachers can check "Set as excellent homework."
- The class bulletin board displays a list of excellent homework (in reverse chronological order).
- Students can view the content of excellent homework, but not the submitter's name (displays "Excellent Homework Example").
- Teachers can unmark homework as "excellent."

**Acceptance criteria**:
- Mr. Zhang grades Xiao Ming's homework 95 points and checks "excellent."
- Xiao Hong, a student in Grade 7 Class 1, sees this homework on the bulletin board, displayed as "Excellent Homework Example."
- Xiao Hong can view the homework images, but cannot see who submitted it.
```

**Step 3: Rerun the subsequent steps**

```bash
/clarify  # AI might ask: Can one assignment have multiple excellent examples?
/plan     # AI updates plan.md, adding an `is_excellent` field to the `homework_submission` table.
/tasks    # AI automatically inserts new tasks.
```

**New tasks generated by the AI**:

```markdown
#### Task 008.1 [Depends on: Task 008] - Excellent Homework Marking Feature
- Add `markAsExcellent(Long submissionId)` method to `ReviewService`.
- Add an `is_excellent` field to the `homework_submission` table (0 for no, 1 for yes).
- **Deliverable**: Teachers can mark homework as excellent.

#### Task 019 [Depends on: Task 008.1] - Excellent Homework Showcase API
- Implement `GET /api/excellent-homework/list?classId=1`.
- Anonymize the returned data: change the `studentName` field to "Excellent Homework Example."
- **Deliverable**: The frontend can get a list of excellent homework.

#### Task 020 [Depends on: Task 019] - Frontend Bulletin Board Page
- Vue component: `ExcellentHomeworkList.vue`.
- Display excellent homework (without student information).
- Click to view details.
- **Deliverable**: Students can view excellent homework.
```

**Step 4: Run `/analyze`**

```bash
/analyze
```

```markdown
## Executive Summary
- **CRITICAL issues: 0** ✅
- New requirement coverage: 100% (Requirement 7 is covered by 3 tasks).

✅ Ready: `/speckit.implement`
```

**Step 5: Incremental Implementation**

```bash
/implement
```

The AI only generates the new Tasks 008.1, 019, and 020, without regenerating the entire project.

**Time comparison**:

| Phase | Traditional Development | SDD Method |
|------|---------|---------|
| Update PRD | 1 hour | 5 minutes (edit `spec.md`) |
| Design review | 2 hours | 0 minutes (automatic) |
| Modify code | 4 hours | 15 minutes (`/implement`) |
| Test validation | 2 hours | Automatic (TDD) |
| **Total** | **9 hours** | **20 minutes** |
| **Speedup** | - | **27x** |

---

## Four. In-depth Analysis of the Three Core Mechanisms

### 4.1 Constitution: How to Automatically Constrain LLM Behavior

#### The Nine Categories of Principles in the Constitution (using the Intelligent Classroom as an example)

| Article | Content | How to Detect | Consequence of Violation |
|------|------|---------|---------|
| **Article 1: Module-First** | Features must be implemented as independent Services first. | `/speckit.analyze` checks if business logic is written directly in the Controller. | CRITICAL error |
| **Article 2: Clear Interface** | Uniform `Result<T>` return format. | `/speckit.analyze` checks the return type of Controllers. | HIGH warning |
| **Article 3: Test-First** | Write tests before implementation. | `/speckit.implement` checks the task order. | Refuses to generate code |
| **Article 4: Data Security** | Encrypt sensitive information for storage. | `/speckit.analyze` checks for `_encrypted` fields. | CRITICAL error |
| **Article 7: Simplicity** | ≤3 Maven modules. | Checked when `/speckit.plan` is generated. | Requires written justification |
| **Article 8: Anti-Over-Abstraction** | Use MyBatis-Plus directly. | `/speckit.analyze` checks for unnecessary wrappers. | Requires justification |
| **Article 9: Integration Testing** | Test with a real MySQL. | `/speckit.implement` checks the test configuration. | Test suite fails |

#### How the Constitution is Automatically Enforced during `/implement`

**Example: Enforcement of Article 3 "Test-First"**

1. **User runs `/speckit.implement`**.
2. **AI reads Article 3 of the Constitution**:
   ```
   It is forbidden to write implementation code before the following steps are completed:
   1. Contract tests have been written.
   2. The tests have passed user review.
   3. The tests have been run and confirmed to fail.
   ```
3. **AI enforces the TDD flow**:
   ```
   [Task 003] Writing contract tests...

   @Test
   void testPublishHomework() {
       // Call the interface
       Result<Long> result = homeworkController.publishHomework(dto, 1L);
       // Assert
       assertEquals(200, result.getCode());
   }

   ❌ Tests fail (as expected) - `HomeworkService` not yet implemented.

   [Waiting for user confirmation...]

   User: "The tests are well-written, continue."

   // Now the AI generates the implementation code
   [Task 006] Implementing `HomeworkService`...
   ```

**Traditional AI coding** vs. **Constitution-constrained AI coding**:

| Aspect | Traditional AI | Constitution-constrained AI |
|------|--------|------------|
| Testing | Add tests afterwards (or not at all). | Enforces test-first. |
| Architecture | Create modules arbitrarily. | ≤3 modules, more requires justification. |
| Data Security | May store in plaintext. | Sensitive fields must be encrypted, otherwise CRITICAL. |
| Quality | Relies on manual review. | `/speckit.analyze` automatically detects violations. |

#### How to Customize the Constitution for Your Own Project

**Step 1: Identify non-negotiable principles**

Ask yourself:
- What technical decision errors will cause the project to fail?
- What code quality standards must be 100% adhered to?
- What architectural principles cannot be sacrificed for deadlines?

**Example: Constitution for a financial payment project**

```markdown
# Payment System Project Constitution

## Article 1: Idempotency Principle (Non-negotiable)

All write operation APIs must:
1. Accept an idempotency key (`idempotency-key` request header).
2. Return the same result for repeated requests, without double-charging.
3. The idempotency key is stored in Redis for 24 hours.

Violation = CRITICAL, blocks deployment.

## Article 2: Mandatory Audit Log

All state changes must:
1. Record the operator, time, and before/after values.
2. Be written to the `audit_log` table (append-only).
3. Be retained for 7 years (regulatory requirement).

Violation = CRITICAL, compliance failure.

## Article 3: Monetary Precision

All monetary calculations must:
1. Use `BigDecimal`, forbidding `float`/`double`.
2. Round to the cent (2 decimal places).
3. Unit tests must cover boundary values (0.01, -0.01, Max).

Violation = HIGH, needs fixing.

## Article 4: Performance Budget

All APIs must have:
1. P95 latency < 200ms.
2. P99 latency < 500ms.
3. Support for 1000 QPS concurrency.

Violation = HIGH, performance optimization task needed.
```

**Step 2: Define check gates in the `/plan` phase**

```markdown
### Phase 0: Pre-implementation Check

#### Idempotency Gate (Article 1)
- [ ] Do all POST/PUT interfaces accept an `idempotency-key`?
- [ ] Is the Redis configuration ready?
- [ ] Have test cases for repeated requests been written?

#### Audit Gate (Article 2)
- [ ] Has the `audit_log` table been created?
- [ ] Has the AOP aspect been configured?

#### Monetary Precision Gate (Article 3)
- [ ] Does `pom.xml` forbid the introduction of Lombok (which can generate builders with `float`)?
- [ ] Are all monetary field types `BigDecimal`?
```

**Step 3: `/analyze` automatically performs the checks**

```bash
/analyze
```

The AI automatically scans:
```markdown
| ID | Category | Severity | Location | Issue | Recommendation |
|----|------|--------|------|------|------|
| C1 | Constitution Violation | CRITICAL | PaymentService.java:45 | Violation of Article 3: Uses `double` type for monetary calculation. | Change to `BigDecimal`. |
| C2 | Constitution Violation | CRITICAL | tasks.md | Violation of Article 2: No task for audit logging. | Add a task: Implement audit AOP. |
```

### 4.2 Template: Guiding the LLM to Generate High-Quality Specs

#### How Templates Constrain LLM Output

**Problem**: Directly asking an LLM "help me write a PRD for a login feature" will result in a vague output.

**Traditional Prompt** (low quality):
```
User: Help me write a requirements document for a student homework submission feature.

AI: The student homework submission feature includes:
1. Students can upload homework after logging in.
2. Supports file uploads.
3. The teacher can see it after submission.

(Problem: Too vague, no boundary conditions, no error handling)
```

**Template-constrained method** (high quality):
```
User: /specify a student homework submission feature

AI: (reads `spec-template.md`)

Template requirements:
- ✅ Focus on what and why, not how.
- ✅ Mark ambiguous points with `[To be confirmed]`.
- ✅ Include boundary cases.
- ✅ Define measurable acceptance criteria.

Generated `spec.md`:

## Feature Overview
**What**: Students submit homework answers online.
**Why**: To replace paper-based submission, for easier retention and grading.

## Functional Requirements

### Requirement 1: File Upload
Students can upload homework files.

[To be confirmed: What file formats are supported? Images, PDF, or Word?]
[To be confirmed: File size limit? 5MB or 10MB?]

## Boundary Cases

### Boundary Case 1: Upload Timeout
A student uploads a 5MB file, the network is slow, and it doesn't complete in 30 seconds.
**Handling**: The frontend displays an "Uploading" progress bar. After 30 seconds, it prompts "Please check your network."

### Boundary Case 2: Repeated Submission
A student submits, finds a mistake, and wants to resubmit.
[To be confirmed: Is modification allowed? Or is it locked after one submission?]

## Non-functional Requirements

### Performance Requirements
- A 5MB file uploads within 10 seconds (on a normal network).
- Supports 50 students uploading simultaneously.

### Data Security
- Homework files are stored in OSS with access control.
- Students can only access their own homework, not others'.
```

**Comparison**: The template forced the AI to think about:
1. ✅ Core functionality (Requirement 1)
2. ✅ Boundary cases (timeout, repeated submission)
3. ✅ Marked ambiguity (2 points to be confirmed)
4. ✅ Performance metrics (with specific numbers: 10 seconds, 50 people)
5. ✅ Data security (access control)
6. ✅ Testability (clear acceptance criteria)
7. ✅ No technical implementation (no mention of OSS, MySQL)

#### The `[To be confirmed]` Mechanism: Forcing the AI to Acknowledge Uncertainty

**Problem**: LLMs tend to "guess" missing information, leading to incorrect assumptions.

**Solution**: The template enforces marking:

```markdown
Rules for creating specs:
1. **Mark all ambiguous points**: Use `[To be confirmed: specific question]`.
2. **Don't guess**: If the prompt doesn't say, mark it.
3. **Provide options**: Give 2-3 alternative solutions.
```

**Practical example**:

User prompt:
```
/specify develop a file upload feature
```

AI-generated (with `[To be confirmed]` marks):
```markdown
## Requirement 1: File Upload

Users can upload files.

[To be confirmed: What file types are supported?]
- Option A: Images only (jpg, png) - suitable for homework submission.
- Option B: Documents (pdf, docx) - suitable for sharing materials.
- Option C: Any file (higher security risk).

[To be confirmed: File size limit?]
- Option A: 5MB (web standard) - suitable for images.
- Option B: 10MB - suitable for PDFs.
- Option C: 100MB - suitable for videos (requires CDN support).

[To be confirmed: Where is it stored?]
- Option A: Local server disk - simple, but not easily scalable.
- Option B: Aliyun OSS/Tencent Cloud COS - recommended, reliable.
- Option C: Database BLOB field - not recommended, poor performance.
```

**The user resolves this in the `/speckit.clarify` phase**:
```
User: For file type, choose Option A (images only); for size, choose Option A (5MB); for storage, choose Option B (OSS).
```

The AI updates the spec:
```markdown
### Requirement 1: File Upload (Clarified)

Users can upload image files (jpg, png format), single file ≤5MB, stored in Aliyun OSS.
```

#### Phase Gates: Automatic Quality Checks

**Definition**: When generating with `/speckit.plan`, the AI must check the gates defined in the constitution.

**`plan-template.md` snippet**:
```markdown
### Phase 0: Pre-implementation Check

#### Simplicity Gate (Article 7)
- [ ] Use ≤3 Maven modules?
  - If no: Explain why in the "Complexity Tracking" section.

#### Anti-Over-Abstraction Gate (Article 8)
- [ ] Use the framework directly?
  - If no: Explain why a wrapper is needed.

#### Test-First Gate (Article 3)
- [ ] Are contract tests before implementation?
  - Must be in Phase 0 of `tasks.md`.
```

**The AI's flow when executing `/speckit.plan`**:

```
[AI] Reading constitution...
[AI] Article 7: At most 3 Maven modules.
[AI] Checking the plan...
[AI] Found: common, service, web, admin = 4 modules.
[AI] ❌ Simplicity Gate not passed.
[AI] Writing to `plan.md`:

### Phase 0: Pre-implementation Check

#### Simplicity Gate (Article 7)
- [❌] Use ≤3 Maven modules?
  **Status**: Not passed (4 modules).
  **Reason**: The admin module needs separate permission control and cannot be mixed with the web module.
  **Approval**: Awaiting user confirmation.

[Waiting for user review...]

User: Remove the admin module. The initial version will not have a backend management interface.

[AI] ✅ `plan.md` updated, admin module removed, Simplicity Gate passed.
```

### 4.3 Analyze Quality Gate: Six Types of Detection + Four Severity Levels

#### The Six Detection Types of `/analyze`

**1. Duplication Detection**

Detects similar or duplicate requirements.

**Example**:
```markdown
spec.md:
Requirement 1: Students can upload homework images.
Requirement 5: The system supports students submitting image-based homework.

`/analyze` report:
| D1 | Duplication | HIGH | Req 1 vs Req 5 | The two requirements describe the same function. | Merge into Requirement 1, delete Requirement 5. |
```

**2. Ambiguity Detection**

Detects vague adjectives and non-measurable standards.

**Example**:
```markdown
spec.md:
Non-functional Requirement 1: The system should respond quickly.

`/analyze` report:
| A1 | Ambiguity | HIGH | Non-func Req 1 | "Quickly" is not measurable. | Change to "API response time P95 < 200ms." |
```

**Common ambiguous words**: fast, slow, good, stable, secure, smooth, intuitive.

**3. Underspecification Detection**

Detects requirements missing objects or outcomes.

**Example**:
```markdown
spec.md:
Requirement 3: Teachers can delete.

`/analyze` report:
| U1 | Underspecification | MEDIUM | Req 3 | Missing object: delete what? | Change to "Teachers can delete homework they have published." |
```

**4. Constitution Alignment**

Detects violations of constitutional principles.

**Example**:
```markdown
constitution.md:
Article 3: Test-First Principle (Non-negotiable).

tasks.md:
Phase 1: Task 001 - Implement `HomeworkService`.
Phase 2: Task 010 - Write tests for `HomeworkService`.

`/analyze` report:
| C1 | Constitution Violation | CRITICAL | tasks.md Phase 1 | Violates Article 3: Implementation is before testing. | Adjust order: Task 010 must be before Task 001. |
```

**5. Coverage Gap**

Detects requirements with no corresponding tasks, or tasks with no corresponding requirements.

**Example**:
```markdown
spec.md:
Non-functional Requirement 2: The system must support 100 students submitting homework simultaneously.

tasks.md:
(No concurrency test task found)

`/analyze` report:
| G1 | Coverage Gap | CRITICAL | Non-func Req 2 | No task covers concurrency testing. | Add Task 020: "JMeter stress test with 100 concurrent users." |
```

**6. Inconsistency Check**

Detects terminology drift and data model conflicts.

**Example**:
```markdown
spec.md:
Uses the term "homework status" (Not Submitted, Submitted, Graded).

data-model.md:
Field name: `homework_state` (enum).

api-spec.yaml:
Parameter name: `homeworkStatus` (string).

`/analyze` report:
| I1 | Inconsistency | MEDIUM | spec/plan/contracts | Terminology drift: "status" vs "state" vs "Status". | Unify to "status" (database, API, documentation). |
```

#### The Four Severity Levels

| Level | Definition | Typical Issues | How to Handle |
|------|------|---------|---------|
| **CRITICAL** | Violates a MUST clause of the constitution, or zero coverage for a core feature. | Test-first not followed, data security violation, main feature has no tasks. | **Blocks `/speckit.implement`**, must be fixed. |
| **HIGH** | Duplicate/conflicting requirements, vague security/performance requirements. | Two requirements describe the same function, non-functional requirement has no metrics. | Strongly recommended to fix, continuing is risky. |
| **MEDIUM** | Terminology drift, non-functional requirements not covered. | `status` vs `state` inconsistency, different wording in docs and code. | Recommended to fix, does not affect functionality. |
| **LOW** | Style/wording improvements. | Inconsistent task ID format (T1 vs T001), non-standard comments. | Optional improvement, does not affect quality. |

#### Typical Output Structure of `/analyze`

```markdown
# Spec Analysis Report

## Executive Summary
- Functional requirements: 6
- Non-functional requirements: 2
- Total number of tasks: 18
- Coverage: 100% (8/8 requirements have tasks)
- **CRITICAL issues: 1**
- HIGH issues: 2
- MEDIUM issues: 3
- LOW issues: 1

## ❌ CRITICAL Issues (Must be fixed)

| ID | Category | Severity | Location | Issue Description | Recommendation |
|----|------|--------|------|----------|------|
| C1 | Constitution Violation | CRITICAL | tasks.md Phase 2 | Violates Article 3: Task 006 is implemented before Task 003's tests. | Adjust the dependency order. |

## ⚠️ HIGH Issues (Strongly recommended)

| ID | Category | Severity | Location | Issue Description | Recommendation |
|----|------|--------|------|----------|------|
| H1 | Duplication | HIGH | Req 2 vs Req 7 | Both describe "student homework submission." | Merge into Requirement 2. |
| H2 | Ambiguity | HIGH | Non-func Req 1 | "The system should be secure" has no standard. | Make it specific: "Pass Level 3 of the national information security certification." |

## 📊 Coverage Details

| Requirement | Task? | Task ID | Notes |
|------|--------|--------|------|
| Req 1: Publish Homework | ✅ | Task 006, Task 009 | Fully covered |
| Req 2: Student Submission | ✅ | Task 007, Task 010 | Fully covered |
| ...
| Non-func Req 2: Concurrency | ❌ | None | **Missing** - See G1 |

## 🚨 Constitution Alignment Issues

### Article 3 Violation (CRITICAL)
- ❌ Task 006 "Implement Service" is before Task 003 "Tests".
- ✅ Other tasks follow the TDD order.

## 📈 Statistical Metrics
- Requirement coverage: 88% (7/8)
- Task mapping rate: 94% (17/18)
- Ambiguity count: 1
- CRITICAL issues: 1

## ⚙️ Next Actions

### ❌ Blocked: Cannot execute `/implement`
**Reason**: 1 CRITICAL issue.

### Must be fixed:
1. **C1**: Adjust task dependencies in `tasks.md`.
2. **G1**: Add a concurrency test task.

### After fixing:
Rerun `/speckit.analyze` to confirm CRITICAL=0, then execute `/speckit.implement`.
```

---

## Five. Frequently Asked Questions

### Q1: The AI-generated code deviates from the requirements. How do I update the spec?

**Scenario**: After running `/speckit.implement`, you find that the AI-generated grading page does not have a "batch grading" feature, and you want to add it now.

**❌ Wrong way**:
```
Directly tell the AI: "Add a batch grading button."
→ The AI adds it, but `spec.md`/`plan.md` are not updated.
→ It disappears the next time it's regenerated.
```

**✅ Correct way**:

**Step 1: Determine the level of the change**

"Batch grading" is a new functional requirement → affects the `/speckit.specify` level.

**Step 2: Go back to `/speckit.specify` and update `spec.md`**

```markdown
### Requirement 8: Batch Grading (New)

Teachers can grade multiple students' homework at once to improve efficiency.

**Feature details**:
- On the homework list page, teachers can check multiple "Submitted" status assignments.
- Clicking the "Batch Grade" button enters batch mode.
- Homework is displayed one by one, and the teacher can quickly give a score and comments.
- Supports keyboard shortcuts: Enter for the next one, Ctrl+S to save.

**Acceptance criteria**:
- Mr. Zhang checks 5 assignments and enters batch mode.
- Grades them one by one, spending 10 seconds on each.
- After the 5 assignments are graded, their statuses all change to "Graded."
```

**Step 3: Rerun the subsequent flow**

```bash
/clarify  # AI might ask: Can a specific assignment be skipped during batch grading?
/plan     # AI updates `BatchReviewService`.
/tasks    # AI automatically inserts a new task.
/analyze  # Check for consistency.
/implement  # Generate the batch grading feature.
```

**Key principle**:

| Change Type | Go back to | Example |
|---------|--------|------|
| UI style | `/speckit.implement` | Change button color to blue. |
| Add a field | `/speckit.plan` | Add a `difficulty` field to the homework table. |
| New feature | `/speckit.specify` | Add batch grading. |
| Change core logic | `/speckit.specify` | Switch to AI-based auto-grading. |
| Architectural change | `/speckit.constitution` | Change to a microservices architecture. |

### Q2: What if `/analyze` finds a CRITICAL issue?

**Scenario**: Running `/speckit.analyze` shows:

```markdown
## CRITICAL Issues

| C1 | Constitution Violation | CRITICAL | tasks.md | Article 3 violation: Task 010 is implemented before Task 009's tests. |
```

**Handling process**:

**Step 1: Understand the meaning of CRITICAL**

CRITICAL = **An issue that blocks `/speckit.implement`**. It must be fixed, otherwise:
- It violates the project constitution (a non-negotiable principle).
- Or it causes a core feature to be unimplementable.

**Step 2: Assess the scope of the impact**

```
Task 009 - Write unit tests for `HomeworkService`.
Task 010 - Implement `HomeworkService`.  ← Wrong order
```

Impact: If implementation comes before testing, it violates the TDD principle → Article 3 of the constitution.

**Step 3: Fix `tasks.md`**

**Before fix**:
```markdown
Phase 1: Business Layer
- Task 009 [Parallelizable] - Write tests for `HomeworkService`.
- Task 010 [Parallelizable] - Implement `HomeworkService`.  ← Wrong!
```

**After fix**:
```markdown
Phase 1: Business Layer
- Task 009 [Parallelizable] - Write tests for `HomeworkService`.
- Task 010 [Depends on: Task 009] - Implement `HomeworkService`.  ← Added dependency
```

**Step 4: Rerun `/speckit.analyze`**

```bash
/analyze
```

```markdown
## Executive Summary
- **CRITICAL issues: 0** ✅
- HIGH issues: 2

✅ Ready: Can execute `/speckit.implement`.
```

**Key takeaways**:
- ❌ Do not skip CRITICAL issues and go straight to implementation.
- ✅ After fixing, you must rerun `/speckit.analyze` to confirm.
- ✅ A constitution violation is always CRITICAL, no compromises.

### Q3: Requirements change frequently. How to respond quickly?

**Scenario**: The product manager changes requirements every week, which is painful with traditional development.

**SDD solution: The spec is the single source of truth.**

**Pain points of traditional development**:
```
Requirement change → Update PRD → Notify developers → Manually change code → Re-test → Update documentation (often forgotten)
              ↓          ↓           ↓
           Always outdated   Missed a spot   Docs and code don't match
```

**SDD method**:
```
Requirement change → Update `spec.md` → /plan → /tasks → /analyze → /implement
           (Single source of truth)                               ↓
                                                 Automatically regenerate code
```

**Practical example: Requirement change in the Intelligent Classroom**

**Week 1**: Initial requirement
```
/specify The homework deadline is a fixed point in time.
```

**Week 2**: Product says they want to support extensions.
```
# Edit `spec.md`

### Requirement 9: Deadline Extension (New)

Teachers can extend the homework deadline.

# Rerun
/plan    # AI automatically updates `HomeworkService.extendDeadline()`.
/tasks   # AI automatically adds a task: "Implement extension feature."
/analyze
/implement  # Only `HomeworkService` is regenerated, the rest is unchanged.
```

**Time taken**: 8 minutes.

**Week 3**: Product says they now want to support extensions for individual students.
```
# Edit `spec.md`

### Requirement 9: Deadline Extension (Updated)

Teachers can extend the homework deadline:
- For the whole class: The deadline is extended for all students.
- For a single student: The deadline is extended for a specific student (e.g., who was on leave).

# Rerun
/plan    # AI adds a `student_id` parameter.
/tasks   # AI adds a task: "Support student-level extensions."
/analyze
/implement
```

**Time taken**: 10 minutes.

**Key advantages**:
1. **Single source of truth**: `spec.md` is always up-to-date, and code is generated from the spec.
2. **Incremental updates**: `/speckit.implement` only regenerates the affected parts.
3. **Automatic consistency**: `/speckit.analyze` ensures that the spec, plan, tasks, and code are consistent.
4. **Version control**: `spec.md` is tracked with Git, so the change history is clear.

**Time comparison**:

| Task | Traditional Development | SDD Method | Speedup |
|------|---------|---------|-------|
| Add extension feature | 2 hours | 8 minutes | 15x |
| Support student-level extensions | 3 hours | 10 minutes | 18x |
| Ensure docs are in sync | 1 hour (often forgotten) | 0 minutes (automatic) | ∞ |

### Q4: How to ensure Test-First?

**Scenario**: You want the team to strictly follow TDD, but some people always write code first.

**SDD solution: Dual protection with the Constitution + `/analyze`.**

**Mechanism 1: Constitution enforcement**

```markdown
# constitution.md

## Article 3: Test-First Principle (Non-negotiable)

It is forbidden to write implementation code before the following steps are completed:
1. Contract tests have been written.
2. The tests have passed review.
3. The tests have been run and confirmed to fail (TDD red light).

Consequences of violation:
- `/analyze` reports a CRITICAL error.
- `/implement` refuses to generate code.
```

**Mechanism 2: `tasks.md` enforces order**

```markdown
## Phase 0: Contract and Testing (Always first)

#### Task 001 - Define API Contract
- Create an OpenAPI specification.
- **Deliverable**: `api-spec.yaml`.

#### Task 002 [Depends on: Task 001] - Write Contract Tests
- Test all endpoints.
- **Deliverable**: All tests fail (red light) ← Explicitly requires failure.

#### Task 003 [Depends on: Task 002] - User Review Checkpoint
- The user reviews the quality of the tests.
- **Deliverable**: The user gives explicit approval.

## Phase 1: Implementation (After the test pass gate)

#### Task 004 [Depends on: Task 003] - Implement API
- Make the tests from Task 002 pass.
- **Deliverable**: All tests turn green.
```

**Mechanism 3: `/implement` execution logic**

```
AI internal logic:

def implement_task(task_id):
    if task_id == "Task 004":  # Implementation task
        # Check dependencies
        if not is_completed("Task 003"):
            raise Error("Cannot implement: Task 003 (test review) is not complete.")

        # Check test status
        test_results = run_tests("Task 002")
        if test_results.status != "FAILED":
            raise Error("Cannot implement: Tests must fail first (TDD red light).")

        # Generate implementation code
        generate_implementation()

        # Verify that tests turn green
        test_results = run_tests("Task 002")
        if test_results.status != "PASSED":
            raise Error("Implementation is incomplete: Tests still fail.")

        return "✅ Task 004 complete: Tests now pass."
```

**Actual execution flow**:

```bash
/implement

[Task 001] Defining API contract...
✅ `api-spec.yaml` created.

[Task 002] Writing contract tests...
```java
@Test
void testPublishHomework_Success() {
    // Call the API
    Result<Long> result = client.post("/api/homework", dto);
    // Assert
    assertEquals(200, result.getCode());
}
```
Running tests...
❌ 10/10 tests fail (as expected - TDD red light).

[Task 003] Awaiting user review...

User: "The tests are comprehensive, approved."

[Task 004] Implementing API...
```java
@RestController
public class HomeworkController {
    @PostMapping("/api/homework")
    public Result<Long> publishHomework(@RequestBody HomeworkDTO dto) {
        Long id = homeworkService.publish(dto);
        return Result.success(id);
    }
}
```

Running tests...
✅ 10/10 tests pass (TDD green light).
```

**If someone tries to skip testing**:

```bash
/implement

[Checking prerequisites...]
❌ Error: Violation of Article 3 of the Constitution.

`tasks.md` shows:
- Task 010 [Parallelizable] - Implement `HomeworkService`  ← No test dependency!

Required structure:
- Task 009 - Write tests for `HomeworkService`.
- Task 010 [Depends on: Task 009] - Implement `HomeworkService`.

Must: Fix `tasks.md` to enforce the TDD order.

Run `/analyze` to find all violations.
```

---

## Six. Best Practices

### 1. To what level of detail should specs be written?

**Criteria**:
- ✅ Testable
- ✅ Unambiguous
- ❌ No implementation (no HOW)

**Detail comparison**:

| Detail Level | ❌ Too coarse (untestable) | ✅ Appropriate (testable) | ❌ Too fine (implementation details) |
|------|----------------|----------------|------------------|
| **Functional Req** | "The system should be fast." | "API response P95 < 200ms." | "Use Redis to cache query results." |
| **User Action** | "Students can submit homework." | "Students can upload jpg/png, ≤5MB." | "The frontend uses axios to POST to `/api/submission`." |
| **Error Handling** | "The system should be robust." | "After a 30-second network timeout, prompt to retry." | "Use axios's `timeout` config of 30000ms." |

**Practical case: File upload feature**

**❌ Too coarse**:
```markdown
Requirement 1: Students can upload files.
```
Problem: What files? How large? Where are they stored? Can't write tests.

**✅ Appropriate**:
```markdown
Requirement 1: Students can upload homework images.

**Feature details**:
- Supported formats: jpg, png
- File size: Single image ≤5MB, max 9 images
- Upload time limit: Complete within 10 seconds (on a normal network)
- On success, return an OSS address.

**Acceptance criteria**:
- Upload a 3MB jpg image → Returns a URL.
- Upload a 6MB image → Prompts "File too large."
- Upload a `.docx` file → Prompts "Format not supported."
```
Now we can write automated tests!

**❌ Too fine**:
```markdown
Requirement 1: Students POST images to `/api/file/upload` via `multipart/form-data`.

**Implementation details**:
- The backend receives with `MultipartFile`.
- Call `OSSUtil.upload()` to upload to Aliyun.
- Return JSON: `{"code":200,"data":"https://..."}`
```
Problem: This is content for `/speckit.plan`, not the spec.

**How to decide**:

1. **Can I write a test for it?** If not, it's too coarse.
2. **Are there multiple ways to implement it?** If only one, it's too fine.
3. **Does it still hold if the tech stack changes?** If not, it's too fine.

### 2. When is it mandatory to use `/clarify`?

**Mandatory use**:
- ✅ `spec.md` has `[To be confirmed]` marks.
- ✅ The AI-generated spec clearly omits key decisions.
- ✅ Enforced before `/speckit.plan`.

**Not necessary**:
- ❌ The spec is already clear enough (zero `[To be confirmed]`).
- ❌ Just changing some wording (edit `spec.md` directly).

**Practical example**:

```bash
# Step 1: Generate spec with `/specify`
/specify Develop a search feature that supports searching for homework by keyword.

# The AI-generated spec includes:
[To be confirmed: Search algorithm - exact match or fuzzy search?]
[To be confirmed: Search scope - only titles or content as well?]

# Step 2: Must run `/clarify`
/clarify

AI: Found 2 points to be confirmed.

Question 1: Search algorithm
Option A: Exact match (`LIKE 'keyword'`) - simple, but not user-friendly.
Option B: Fuzzy search (`LIKE '%keyword%'`) - recommended.
Option C: Full-text search (MySQL FULLTEXT or ES) - complex.

User's choice: Option B

AI: ✅ Spec updated → "Use SQL fuzzy matching (`LIKE %keyword%`)."

Question 2: Search scope
Option A: Search titles only - fast.
Option B: Titles + content - recommended, better experience.

User's choice: Option B

AI: ✅ Spec updated → "Search fields: `title`, `content`."

All points to be confirmed have been resolved. Can now execute `/plan`.
```

### 3. The best time to use `/analyze`

**Mandatory run**:
1. **After `/speckit.tasks`, before `/speckit.implement`** (Most important!)
2. **After a requirement change** - Run after updating `spec.md`.
3. **Before code review** - Ensure the spec and code are consistent.

**Optional run**:
- After `/speckit.plan`: Check if the plan covers all requirements.
- Periodically every week: Check for spec drift.

**Practical case 1: Discovering a hidden coverage gap**

```bash
# You think all tasks are covered
/tasks

# Run analyze
/analyze

Report:
| G1 | Coverage Gap | CRITICAL | Non-func Req 2 | "Data security: encrypt sensitive information" has no corresponding task. |

# You realize: Oh right, I forgot about encryption!
# Add a task
Task 025 - Implement an AES-256 encryption utility class `EncryptUtil`.

# Re-analyze
/analyze
✅ Coverage: 100%
```

**Practical case 2: Early detection of inconsistent terminology**

```bash
/analyze

Report:
| I1 | Inconsistency | MEDIUM | spec vs plan | Spec uses "homework," plan uses "assignment." |

# Found early, easy to fix.
# If found after the code is written, the scope of changes would be huge (table names, APIs, variable names...).
```

### 4. When to use `/checklist`

**Purpose**: Domain-specific in-depth quality checks to verify the completeness of specs in a particular area.

**Typical use cases**:
```bash
# High-risk areas: payment/security/compliance
/plan → /checklist payment security → /tasks → /analyze → /implement

# Specific domains: UX/performance/API
/specify → /checklist ux → /clarify → /plan
```

**Relationship with `/analyze`**:
- `/analyze` - Mandatory, general quality check (duplication, ambiguity, coverage, etc.).
- `/checklist` - Optional, domain expert perspective check (e.g., "Are idempotency requirements clear?").
- Use together: First `/checklist` for an in-depth check, then `/analyze` for general validation.

**Decision to use**: High-risk/critical areas (payment, security, compliance) → Must use; ordinary features → Not necessary.

### 5. How to create a good Constitution

**Principle 1: No more than 10 articles**

Too many principles = no principles.

**Principle 2: Every article must be automatically detectable**

If `/speckit.analyze` can't detect it, it's not a good principle.

**❌ Undetectable**:
```markdown
Article 5: The code should be elegant and easy to maintain.
```
Problem: "Elegant" and "easy to maintain" are not quantifiable.

**✅ Detectable**:
```markdown
Article 5: Code Complexity Limits

All methods must have:
- Cyclomatic complexity ≤ 10
- Max nesting level ≤ 3
- Method length ≤ 50 lines

Detection method: Run a code complexity analysis tool during the `/analyze` phase.
```

**Principle 3: Clear consequences for violations**

Each principle should specify the severity of a violation.

**Constitution template**:

```markdown
## Article [N]: [Principle Name]

### Rule
[Specific, measurable requirements]

### Reason
[Why this principle exists]

### Violation Handling
- Severity: CRITICAL / HIGH / MEDIUM
- Detection method: How `/analyze` detects it
- Consequence: Block/Warning/Log

### Exceptions
[Under what circumstances it can be violated, what approval is needed]
```

**Example: Constitution for a financial system**

```markdown
## Article 1: Monetary Precision Principle

### Rule
All monetary calculations must:
1. Use the `BigDecimal` type, forbidding `float`/`double`.
2. Round to the cent (`setScale(2, RoundingMode.HALF_UP)`).
3. Unit tests must cover boundary values (0.01, -0.01, `Long.MAX_VALUE`).

### Reason
`float`/`double` have precision loss, which can lead to financial discrepancies and reconciliation problems.

### Violation Handling
- Severity: CRITICAL
- Detection method: `/analyze` scans the code for `float`/`double` fields.
- Consequence: Blocks deployment, must be fixed.

### Exceptions
No exceptions. Monetary precision is non-negotiable.
```

---

## Seven. Conclusion: The Core Value of SDD

### Traditional Development vs. SDD Comparison

| Dimension | Traditional Development | SDD Method | Improvement |
|------|---------|---------|------|
| **Source of Truth** | Code (docs are outdated) | Spec document | Single source of truth |
| **Requirement Change** | Manually change code + docs | Update spec and regenerate | 20-30x speedup |
| **Quality Assurance** | Manual code review | Constitution + `/analyze` auto-detection | Automation |
| **Test-First** | Relies on self-discipline | Enforced TDD flow | 100% compliance |
| **Consistency** | Often inconsistent | `/analyze` enforces detection | 0 inconsistencies |
| **Newcomer Onboarding** | Takes months | Just follow the template | Onboard in weeks |

### Projects Suitable for SDD

✅ **Ideal scenarios**:
- New projects (from scratch)
- Frequently changing requirements
- Teams of 3-20 people
- High quality requirements (finance, healthcare, education)
- Need for rapid trial and error

⚠️ **Needs adjustment**:
- Legacy system modernization - Need to "specify" existing code first.
- Very large projects (>50 people) - Need a layered constitution.

❌ **Not suitable for**:
- One-off scripts
- Purely frontend UI debugging

### Start Your SDD Journey

**Step 1: Install Spec-Kit**

```bash
# Install with uv (recommended)
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git

# Or run temporarily with uvx
uvx --from git+https://github.com/github/spec-kit.git specify init my-project
```

**Step 2: Initialize the project**

```bash
specify init Intelligent-Classroom --ai claude
# Choose: Use Claude Code as the AI assistant
```

**Step 3: Establish the constitution**

```bash
/constitution Create principles focusing on code quality, testing standards, and education industry compliance.
```

**Step 4: Develop the first feature**

```bash
/specify Develop a student homework submission feature that supports uploading images and PDFs.
/clarify
/plan Use Spring Boot + MyBatis-Plus + MySQL + Aliyun OSS.
/tasks
/analyze  # Crucial step!
/implement
```

**Step 5: Join the community**

- GitHub: https://github.com/github/spec-kit
- Raise issues to share your practical experience.
- Participate in discussions to improve the SDD methodology.

---

## Remember the Four Cores of SDD

1. **The spec is the truth, the code is the expression.**
   - Maintaining software = Evolving the spec
   - Debugging = Fixing logic in the spec
   - Refactoring = Reorganizing the structure of the spec

2. **The constitution is the DNA, non-negotiable.**
   - Define ≤10 core principles.
   - Each one must be automatically detectable.
   - Violation = CRITICAL, blocks implementation.

3. **`/analyze` is the quality gate, CRITICAL issues must be fixed.**
   - 6 types of detection: Duplication, Ambiguity, Coverage, Constitution, Inconsistency, Terminology.
   - 4 severity levels: CRITICAL blocks, HIGH recommended, MEDIUM improvement, LOW optional.
   - Enforced run between `/tasks` and `/implement`.

4. **SDD is not linear, it's a recursive cycle.**
   - Project-level / feature-level / module-level / task-level.
   - Each level can execute a complete SDD cycle.
   - When issues are found, go back to the appropriate level, not start over.

---

**Start developing your next project with SDD!** 🚀

> This guide is based on the Spec-Kit project and written for Chinese developers.
> Last updated: January 2025
> Feedback welcome: https://github.com/github/spec-kit/issues
