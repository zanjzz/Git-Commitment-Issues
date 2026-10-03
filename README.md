# AI Reading Comprehension Assessment Platform

## 1. Project Overview

We are developing a web-based **AI-assisted reading comprehension assessment platform** designed for **teachers and students from Grades 7–12**.

Students will read a short passage and answer a small set of comprehension questions. AI will analyze their responses and generate a reading comprehension assessment.

Teachers will have access to a compilation of their students' results, allowing them to identify **which students may be struggling and which specific areas of reading comprehension may require additional support**.

The platform is primarily a **monitoring and diagnostic tool**, not a replacement for classroom assessments, teachers, or formal grading systems.

---

# 2. The Problem

Teachers can already identify struggling students through:

- Grades
- Quizzes
- Exams
- Assignments
- Class participation

However, these methods do not always make it easy to **pinpoint the specific learning difficulties of individual students**, especially in larger classes.

A student may receive an overall score, but the teacher may not immediately know whether the underlying difficulty is:

- Understanding the main idea
- Identifying supporting details
- Making inferences
- Understanding vocabulary in context
- Interpreting the author's purpose

Our platform provides a more focused view of students' reading comprehension performance.

---

# 3. Main Goal

**To develop an AI-assisted assessment tool that helps Grades 7–12 teachers efficiently monitor students' reading comprehension and identify learners who may require additional support.**

The system should help teachers understand:

- **Who** may be struggling
- **What** comprehension skills they are struggling with
- **How** their performance changes over time

---

# 4. Target Users

### Teachers

Teachers use the platform to:

- Create or select assessments
- Generate student access
- Monitor class results
- View individual student performance
- Identify areas of difficulty
- Track progress

### Students — Grades 7–12

Students use the platform to:

- Access an assessment using a QR code
- Read a short passage
- Answer comprehension questions
- Submit their answers
- Receive appropriate feedback/results

Students do **not need to create an email account** to use the assessment.

---

# 5. Authentication & Student Access

## Teacher Authentication

Teachers will have a standard authenticated account.

The teacher account can be used to:

- Create/manage classes
- Create assessments
- Generate student assessment sessions
- View student results
- Monitor assessment history

Supabase Authentication can be used for teacher accounts.

---

## Student Authentication — QR-Based Temporary Access

Instead of requiring students to create email/password accounts, the system will use **temporary QR-based assessment sessions**.

### Basic Flow

```text
Teacher logs in
      ↓
Selects class / assessment
      ↓
System generates temporary session
      ↓
QR code is displayed
      ↓
Students scan QR code
      ↓
Temporary student session opens
      ↓
Student enters/selects their name or assigned identifier
      ↓
Completes assessment
      ↓
Submits answers
      ↓
Results are linked to the assessment session
```

### Why QR?

QR access allows students to enter an assessment using a phone, tablet, or computer without requiring:

- Email
- Password
- Account registration
- Complicated login credentials

This is particularly useful for younger students and classroom environments.

---

# 6. Temporary Session Design

The QR code should **not directly contain permanent student credentials**.

Instead, it should contain a short-lived **assessment/session token**.

For example:

```text
Teacher creates Assessment
        ↓
Session Token Generated
        ↓
QR Code
        ↓
Student scans
        ↓
Temporary Assessment Session
```

The session can have:

- Unique session ID
- Expiration time
- Assessment ID
- Class ID
- Optional maximum number of attempts

For example:

> **Assessment Session:** Reading Comprehension #01  
> **Expires:** 30 minutes  
> **Status:** Active

Once the assessment expires, the QR/session token becomes invalid.

This prevents old QR codes from being reused indefinitely.

---

# 7. Student Identity

Because students do not need permanent accounts, the system can associate their submission with a **class-specific student identifier**.

For the MVP, we can use:

**Student Name + Teacher/Class**

or preferably:

**Teacher-generated Student Code**

Example:

```text
Student:
Juan Dela Cruz

Student Code:
G7A-014
```

The student enters or selects their identifier after scanning the QR code.

This avoids requiring an email address while still allowing the teacher to recognize individual results.

### Important Privacy Principle

Student information should be kept to the **minimum necessary information** required for the assessment.

We do not need to collect unnecessary personal information.

---

# 8. Local Storage Strategy

The student's browser can use **localStorage** for temporary client-side session information, such as:

- Current session token
- Assessment progress
- Temporary student identifier
- Whether the assessment has been submitted

Example:

```text
localStorage
├── sessionToken
├── assessmentId
├── studentId
└── assessmentProgress
```

However, **localStorage should not be treated as the permanent source of truth for assessment results**.

Once the student submits:

```text
Student Browser
      ↓
Python Backend
      ↓
AI Assessment
      ↓
Supabase Database
```

The final assessment result is stored in Supabase so that the teacher can access it later.

This gives us:

**Temporary student access + persistent teacher results.**

---

# 9. Core User Flow

### Teacher

```text
Login
  ↓
Select Class
  ↓
Create / Select Assessment
  ↓
Generate Assessment Session
  ↓
Display QR Code
  ↓
Students Complete Assessment
  ↓
View Results
  ↓
Monitor Students
```

### Student

```text
Scan QR
  ↓
Temporary Session
  ↓
Enter / Select Student Identifier
  ↓
Read Passage
  ↓
Answer Questions
  ↓
Submit
  ↓
AI Evaluation
  ↓
Result
```

---

# 10. AI Assessment

AI will evaluate students' responses based on defined reading comprehension skills.

Possible assessment categories:

- **Main Idea**
- **Supporting Details**
- **Inference**
- **Vocabulary in Context**
- **Author's Purpose**
- **Overall Comprehension**

The system should provide both a score and an understandable breakdown.

Example:

> **Student A**
>
> Main Idea — Proficient  
> Supporting Details — Proficient  
> Inference — Developing  
> Vocabulary — Needs Practice
>
> **Primary area for improvement:** Inference

The AI should assist with assessment rather than completely replace teacher judgment.

---

# 11. Teacher Dashboard

The dashboard is the primary feature of the platform.

### Class Overview

- Students assessed
- Overall performance
- Common comprehension difficulties
- Students who may require additional attention

### Individual Student

- Assessment history
- Skill breakdown
- Responses
- AI analysis
- Areas requiring improvement
- Progress over time

The goal is to transform individual assessment results into information that is **quick and easy for teachers to understand and act upon**.

---

# 12. Assessment Design

Each assessment should contain:

1. A short reading passage
2. A small number of questions
3. Questions targeting specific comprehension skills
4. Student responses
5. AI-generated analysis

Assessments should be **short and repeatable**, allowing teachers to use them periodically without creating another major workload.

---

# 13. Integrity and Intended Use

The platform is intended primarily for **diagnostic and formative assessment**, not formal grading.

Schools or teachers may decide whether assessments are completed:

- In the classroom
- At home
- Through assigned activities

The platform itself does not determine how schools implement the assessment.

Students should be encouraged to answer honestly because the purpose is to accurately identify areas where they may need support.

---

# 14. Technology Stack

### Frontend

**React + Vite**

Used for:

- Teacher dashboard
- Student assessment interface
- QR scanning/access flow
- Assessment management
- Results visualization

### Backend

**Python**

Used for:

- Backend/API
- AI processing
- Assessment evaluation
- Authentication/session validation
- Communication between frontend, AI services, and database

### Database & Authentication

**Supabase**

Used for:

- Teacher authentication
- Classes
- Assessments
- Student identifiers
- Assessment submissions
- AI-generated results
- Assessment history

### QR

A frontend QR-generation library can generate temporary assessment QR codes.

The QR should contain a **temporary session URL/token**, rather than sensitive student information.

---

# 15. Proposed Architecture

```text
                     ┌──────────────────┐
                     │   React + Vite   │
                     │   Web Interface  │
                     └────────┬─────────┘
                              │
               ┌──────────────┴──────────────┐
               │                             │
               ↓                             ↓
       ┌───────────────┐              ┌──────────────┐
       │ Teacher Portal │              │ Student Portal│
       └───────┬───────┘              └───────┬──────┘
               │                              │
               └──────────────┬───────────────┘
                              ↓
                    ┌──────────────────┐
                    │  Python Backend  │
                    │      / API       │
                    └───────┬───┬──────┘
                            │   │
                  ┌─────────┘   └─────────┐
                  ↓                       ↓
          ┌──────────────┐        ┌──────────────┐
          │ AI / NLP     │        │   Supabase   │
          │ Assessment   │        │ Auth + DB    │
          └──────────────┘        └──────────────┘
```

---

# 16. Hackathon MVP

The MVP should focus on one complete working cycle.

### Must Have

- Teacher login
- Create/select class
- Create/select assessment
- Generate temporary QR session
- Student scans QR
- Student identifier
- Reading passage
- Comprehension questions
- Answer submission
- AI assessment
- Results stored in Supabase
- Teacher dashboard
- Individual student results
- Basic assessment history

### Not Required for MVP

- School-wide integration
- Existing LMS integration
- Complex grading systems
- Mobile application
- Advanced gamification
- Large content library
- Parent accounts

---

# 17. Future Innovation — Speech Skills

If the core reading comprehension system is completed early, we will explore extending the platform into **speech/verbal skills assessment**.

Possible flow:

```text
Student speaks
      ↓
Speech Recognition
      ↓
Transcription
      ↓
AI Analysis
      ↓
Speech / Verbal Assessment
```

Possible technologies include **Agora or other speech/real-time communication and recognition tools**, depending on available development time.

This is considered a **secondary innovation**, not part of the core MVP.

---

# 18. Project Focus

Our platform is **not intended to replace teachers, formal examinations, or existing school systems.**

Its purpose is to provide teachers with a **clearer and more detailed picture of individual students' reading comprehension performance**.

### Core Principle

**Assess → Identify → Monitor → Support**

The AI handles repetitive analysis, while the teacher remains responsible for interpreting the results and deciding what support a student needs.

### One-Sentence Pitch

> **An AI-assisted reading comprehension assessment platform that helps Grades 7–12 teachers quickly identify which students need support and understand exactly where they are struggling.**
