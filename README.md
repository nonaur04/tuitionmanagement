# TUITION MANAGEMENT
A web based tuition class management system for tutors, students and payments

## Table of Contents

1. [Introduction](##1.0-introduction)  
    [1.1 Project Overview](###1.1-project-overview)  
    [1.2 Commercial Value / Third-Party Integration](###1.2-commercial-value--third-party-integration)

2. [System Architecture](#2-system-architecture)  
   - [2.1 High-Level Diagram](#21-high-level-diagram)

3. [Backend Application](#3-backend-application)  
   - [3.1 Technology Stack](#31-technology-stack)  
   - [3.2 API Documentation](#32-api-documentation)  
     - [3.2.1 Endpoints List](#321-endpoints-list)  
     - [3.2.2 Request Format](#322-request-format)  
     - [3.2.3 Example Responses](#323-example-responses)  
     - [3.2.4 Security Measures](#324-security-measures)

4. [Frontend Applications](#4-frontend-applications)  
   - [4.1 Student App](#41-student-app)  
     - [4.1.1 Purpose](#411-purpose)  
     - [4.1.2 Technology Stack](#412-technology-stack)  
     - [4.1.3 API Integration](#413-api-integration)  
   - [4.2 Admin App](#42-admin-app)  
     - [4.2.1 Purpose](#421-purpose)  
     - [4.2.2 Technology Stack](#422-technology-stack)  
     - [4.2.3 API Integration](#423-api-integration)

5. [Database Design](#5-database-design)  
   - [5.1 Entity-Relationship Diagram (ERD)](#51-entity-relationship-diagram-erd)  
   - [5.2 Schema Justification](#52-schema-justification)

6. [Business Logic and Data Validation](#6-business-logic-and-data-validation)  
   - [6.1 Use Case Diagrams / Flowcharts](#61-use-case-diagrams--flowcharts)  
   - [6.2 Data Validation Rules](#62-data-validation-rules)

7. [Conclusion](#7-conclusion)


## 1.0 Introduction

  ### 1.1 Project Overview
  Tuition Management System helps tutors and students manage tuition classes in organizing and managing tuition related data. The system provides key features such as class scheduling, schedule status, student enrollment, attendance tracking and payment management. Our team focuses on automating manual processes like registration, fee tracking, students attendance tracking and timetable coordination. The system aims to reduces administrative workload and human error. 

  Tutors can add easily create and update class sessions, update class time, class status and access list of students. Students can browse available classes, register new classes, display tuition fees and proceed to payment. 

  ### 1.2 Commercial Value / Third-Party Integration
The Tuition Management System has a strong commercial potential especially for private tutors, freelance educator and tuition centers as the system can cater from small to big audience. The system seek a streamlined digital solution in managing daily operation usually done by human. This system can be offered as a software-as-a-services (SaaS) platform, targeting small to medium tuition educators.



# 2.0 System Architecture

  ### 2.1 High-Level Diagram


# 3. Backend Application

### 3.1 Technology Stack

The backend of this tuition management system is built using a combination of powerful technologies to ensure scalability, maintainability and performance. The Tuition Management System is developed using Spring Boot, a Java-based framework that simplifies the development of web application. Eclipse IDE is used as development environment and providing tools for efficient coding. The system uses MySQL database to store and manage data.

| Component        | Technology           |
|------------------|----------------------|
| Language          | Java                |
| IDE               | Eclipse              |
| Framework         | Spring Boot          |
| Database          | MySQL                |


### 3.2 API Documentation

#### 3.2.1 Endpoints List

| Endpoint             | Method | Description                  |
|----------------------|--------|------------------------------|
| `/api/users`         | GET    | List all users               |
| `/api/users/:id`     | GET    | Get user by ID               |
| `/api/classes`       | GET    | Get all classes              |
| `/api/classes`       | POST   | Create a new class           |
| `/api/payments`      | POST   | Make a payment               |
| `/api/login`         | POST   | Login and receive token      |

#### 3.2.2 Request Format

Example (POST `/api/classes`):
json
{
  "subject": "Math",
  "day": "Monday",
  "start_time": "10:00",
  "end_time": "11:30",
  "tutor_id": 2
}


## 5.0 Database Design
### 5.1 ERD diagram

### 5.2 Schema Justification
The database schema is designed to support the system's core functionalities such as user management, class scheduling and payment tracking. The schema follows normalization principles to avoid redundancy and ensure data integrity.

---

####  user Table
- *Purpose: Stores both **students* and *tutors*.
- *Key Fields*:
  - id: Primary key.
  - name, email, username: Basic identity fields.
  - role: Differentiates between 'STUDENT' and 'TUTOR'.
- *Justification*: A single unified table with a role field simplifies user management and authentication without needing separate tables for tutors and students.

---

#### class_entity Table
- *Purpose*: Stores information about tuition classes.
- *Key Fields*:
  - id: Primary key.
  - subject, day, start_time, end_time: Class details.
  - tutor_id: Foreign key to user.id (must have role = 'tutor').
- *Justification*: Each class is linked to one tutor. Storing class timing and subject allows for flexible scheduling and conflict checking.

---

#### class_entity_student_ids Table
- *Purpose*: Bridge for classes and students.
- *Key Fields*:
  - class_entity_id: Foreign key to class_entity.id.
  - student_ids: Foreign key to user.id (must have role = 'student').
- *Justification*: Supports assigning multiple students to a class and reusing classes across terms. It maintains relational integrity without storing arrays or comma-separated lists.

---

#### payment Table
- *Purpose*: Records payment transactions from students.
- *Key Fields*:
  - id: Primary key.
  - student_id: Foreign key to user.id.
  - amount, method, status: Details of each payment.
  - created_date: Timestamp of the transaction.
- *Justification*: This allows tracking of who paid, how much, and by which method. The status field helps in monitoring pending or failed payments.

---

## 6.0 Business Logic & Data Validation

### 6.1 Use Case Diagram

### 6.2 Data Validation Rules

This section outlines the key validation rules implemented across both the **Student** and **Tutor** applications. These rules ensure user inputs are complete, correct, and compatible with backend requirements.

#### Student App Validation

| Field              | Rule                                                                       | Type             | Message / Action                    |
| ------------------ | -------------------------------------------------------------------------- | ---------------- | ----------------------------------- |
| Subject Selection  | Must select a subject from the dropdown (not default)                      | Required Field   | `alert("Select a subject")`         |
| Payment Method     | Must select a payment method                                               | Required Field   | `alert("Choose payment method")`    |
| Subject Price      | Only allowed values: "Bahasa Melayu", "English", "Math", "Science"         | Enumerated List  | Subject not in list → block payment |
| Payment Amount     | Calculated from subject (e.g., Math = RM120)                               | Internal Logic   | Prevent price tampering             |
| Receipt Generation | PDF auto-generated after successful payment                                | System Generated | File: `receipt.pdf`                 |

#### Tutor App Validation

| Field                   | Rule                                                 | Type             | Message / Action                       |
| ----------------------- | ---------------------------------------------------- | ---------------- | -------------------------------------- |
| Add Class - Subject     | Subject name cannot be blank                         | Required Field   | `alert("Please enter a subject name")` |
| Edit Class - Day        | Must select a valid day from dropdown (Mon–Sun)      | Required Field   | Validation at modal level              |
| Edit Class - Time       | Start time must be before end time                   | Logical Check    | If invalid: prevent save               |
| Edit Class - Status     | Must be one of: Completed, Scheduled, Canceled       | Enumerated Field | Invalid status blocks update           |
| Filter/Search Students  | Optional; no restrictions                            | Optional         | —                                      |

#### Backend API Validation (Spring Boot)

| Field                          | Rule                                       | HTTP Error if Violated   |
| ------------------------------ | ------------------------------------------ | ------------------------ |
| POST `/api/classes`            | Subject, day, time must be provided        | 400 Bad Request          |
| POST `/api/payments`           | Amount, method, and studentId are required | 400 Bad Request          |
| All endpoints with ID params   | Must be valid integers and exist in DB     | 404 Not Found / 400      |
