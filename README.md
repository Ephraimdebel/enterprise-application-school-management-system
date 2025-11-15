# 📘 School Management System – Project Proposal

## Team Members
| Name            | ID.NO       
|-----------------|------------
| Alpha Degago    | UGR/4592/15 
| Ashenafi Godana | UGR/7906/14
| Belean Redwan   | UGR/5921/15
| Ephraim Debel   | UGR/0640/15 
| Feben Getachew  | UGR/4295/15 

---

## 1. Business Problem Description

Schools—especially primary and secondary institutions—are responsible for managing thousands of small but critical tasks every day. However, many schools still rely on outdated, manual processes that make their work harder. Student information is often scattered across offices, grade reporting is slow and prone to errors, and communication between teachers, parents, and students is inconsistent. Parents struggle to track their children's performance, teachers waste time on administrative work, and administrators face challenges in coordinating schedules, exams, and fee payments. For private schools, managing tuition payments and keeping payment status updated adds another layer of complexity.  
With no unified digital system, these tasks become overwhelming and inefficient, negatively affecting teaching quality and student experience.  

Our proposed **School Management System** offers a single, modern platform where students, parents, teachers, and school administrators can seamlessly interact. It streamlines academic activities, centralizes communication, automates grade and schedule management, and even supports digital fee payments. Additionally, the system includes an **AI-powered academic assistant** that guides students with personalized study recommendations based on their performance—helping them improve academically without doing their homework for them. The goal is to transform the entire school workflow into an efficient, transparent, and supportive environment for everyone involved.

---

## 2. Domain Decomposition

### 🔵 Core Subdomains
These represent the most essential business capabilities of the school.

#### **1. Student Academic Management**
Tracks academic progress, attendance, grades, and learning performance.  
**Business value:** Directly tied to student success and school quality.

#### **2. Class & Exam Management**
Handles class scheduling, exam timetable creation, grade submission, and academic timelines.  
**Business value:** Supports daily teaching and learning operations.

#### **3. Payment & Finance Management (Private Schools)**
Manages fee payments, installment plans, Chapa integration, and receipts.  
**Business value:** Ensures smooth financial operations and transparency.

---

### 🟡 Supporting Subdomains
Subdomains that enable the core functionality.

#### **1. User & Role Management**
Controls authentication and permissions for students, parents, teachers, and admins.  
**Business value:** Protects sensitive academic data with role-based access.

#### **2. Notification & Communication**
Manages direct messaging and alerts between stakeholders.  
**Business value:** Ensures reliable school-wide communication.

---

### ⚪ Generic Subdomains
Standard, reusable subdomains.

#### **1. Reporting & Analytics**
Provides school-wide summaries, performance insights, attendance patterns, etc.  
**Business value:** Helps administrators make informed decisions.

#### **2. System Administration**
Handles configurations, logs, and platform maintenance.  
**Business value:** Ensures platform reliability and stability.

---

## 3. Bounded Contexts (Core Domain)

### **BC1: Student Management Context**
**Responsibility:**  
Maintains student records, academic history, attendance, and enrollment details. Controls student profiles throughout their entire lifecycle.

---

### **BC2: Academic Context**
**Responsibility:**  
Handles class schedules, exam schedules, grade submissions, subject assignments, and overall academic structure.

---

### **BC3: Payment Management Context**
**Responsibility:**  
Oversees school fee payments, Chapa integration, verification, receipts, and payment status updates for each student.

---

## 4. Cross-Context User Stories

---

### **Story 1: Student Enrollment & Class Assignment**

#### **Contexts Involved**
- Student Management (primary)
- Academic Context (secondary)
- User Management (secondary)

#### **Domain Events**
- `StudentRegistered`
- `StudentEnrolledInGradeLevel`
- `ClassAssignedToStudent`

#### **Eventual Consistency**
Student Management → StudentRegistered → Academic Context
Student Management → StudentEnrolledInGradeLevel → Academic Context
Academic assigns class → ClassAssignedToStudent → Student Management & Notification


---

### **Story 2: Grade Submission and Parent Notification**

#### **Contexts Involved**
- Academic Context (primary)
- Student Management (secondary)
- Notification Context (secondary)

#### **Domain Events**
- `GradeSubmitted`
- `TranscriptUpdated`
- `ParentNotifiedOfGrade`

#### **Eventual Consistency**
Academic → GradeSubmitted → Student Management
Student Management updates transcript → TranscriptUpdated → Notification
Notification → ParentNotifiedOfGrade


---

### **Story 3: Parent Pays School Fee Through Chapa**

#### **Contexts Involved**
- Payment Management (primary)
- Student Management (secondary)
- Notification Context (secondary)

#### **Domain Events**
- `PaymentInitiated`
- `PaymentVerified`
- `StudentPaymentStatusUpdated`

#### **Eventual Consistency**
Payment Context → PaymentInitiated → Chapa API
Chapa verifies → PaymentVerified → Payment Context
Payment Context updates student record → StudentPaymentStatusUpdated → Student Management
Notification → Sends digital receipt to parent


---

## 5. AI-Driven Feature

### 🤖 AI Academic Mentor — Personalized Student Success Assistant

The system includes an intelligent AI module designed to support student learning—not by solving homework, but by guiding students to improve their study habits and academic performance.

**Capabilities:**
- Analyzes grades, attendance, and performance trends  
- Recommends personalized study plans  
- Suggests books and learning resources based on weaknesses  
- Identifies at-risk students using predictive analytics  
- Helps students stay on track with reminders and insights  

**AI Integration Points:**
- Subscribes to events such as `GradeSubmitted`, `TranscriptUpdated`, and `AttendanceRecorded`
- Uses RAG to answer student questions based on school policies and academic resources  
- Sends insights and recommendations through Notification Context  

---

## 6. Technical Implementation Approach

### **Phase 1 — Modular Monolith (Angular + .NET)**

SchoolManagement/
├── StudentManagement/
├── Academic/
├── PaymentManagement/
├── UserManagement/
├── Notification/
├── AIAdvisor/
└── Shared/

### **Phase 2 — Microservice Extraction**
**Microservice Candidate:** AIAdvisor  
- Independent scaling  
- Different processing needs  
- Clear domain boundaries  
- Ideal for separate deployment lifecycle  
