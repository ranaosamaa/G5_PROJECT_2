# 🏥 Hospital Management System

A **console-based Hospital Management System** written in C++ that simulates core hospital operations — patient registration, doctor management, room admission, appointment booking, emergency handling, and medical test tracking — built entirely with **OOP** and **STL data structures**.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Data Structures Used](#data-structures-used)
- [Class Design](#class-design)
- [Enumerations](#enumerations)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
  - [Build with Visual Studio](#build-with-visual-studio)
  - [Build with g++](#build-with-g)
- [Test Cases](#test-cases)
- [Example Output](#example-output)

---

## Overview

This project models a hospital system using three core classes — `Patient`, `Doctor`, and `Hospital` — each with clearly defined responsibilities. It demonstrates practical use of **STL containers** (`stack`, `queue`, `vector`) to manage real-world scenarios like LIFO medical history, FIFO appointment and test queues, and priority emergency handling.

---

## ✨ Features

### 👤 Patient Management
- Register patients with auto-assigned unique IDs
- Admit patients to specific room types (ICU, Private Room, etc.)
- Discharge admitted patients
- Add medical records using a **history stack** (most recent first)
- Request and perform diagnostic tests via a **test queue** (FIFO)
- Display full patient information and medical history

### 👨‍⚕️ Doctor Management
- Add doctors with auto-assigned IDs and assigned departments
- Book patient appointments via a **FIFO appointment queue**
- See the next patient in queue with `seePatient()`
- Display doctor info including pending appointment count

### 🚨 Emergency Handling
- Add patients to a hospital-wide **emergency queue**
- Handle emergencies in arrival order (FIFO)
- Reports when no emergency cases remain

### 🏠 Room Management
- Supports 4 room types: General Ward, ICU, Private Room, Semi-Private
- Guards against double-admission
- Displays current room type in patient info

---

## 📦 Data Structures Used

| Structure | Used In | Purpose |
|-----------|---------|---------|
| `vector<Patient>` | `Hospital` | Store all registered patients |
| `vector<Doctor>` | `Hospital` | Store all registered doctors |
| `stack<string>` | `Patient` | Medical history — last record shown first (LIFO) |
| `queue<string>` | `Patient` | Diagnostic test requests — served in order (FIFO) |
| `queue<int>` | `Doctor` | Appointment queue — first booked, first seen (FIFO) |
| `queue<int>` | `Hospital` | Emergency queue — first arrived, first handled (FIFO) |

---

## 🏗️ Class Design

```
Patient
├── id, name, age, contact
├── isAdmitted: bool
├── roomType: RoomType (enum)
├── medicalHistory: stack<string>     ← LIFO: most recent record on top
├── testQueue: queue<string>          ← FIFO: tests served in request order
│
├── admitPatient(RoomType)            → admits if not already admitted
├── dischargePatient()                → discharges if admitted
├── addMedicalRecord(string)          → pushes onto history stack
├── requestTest(string)              → enqueues a test
├── performTest()                    → dequeues and performs next test
└── displayHistory()                 → prints full history stack (copy)

Doctor
├── id, name, department: Department (enum)
├── appointmentQueue: queue<int>      ← FIFO: patient IDs in booking order
│
├── addAppointment(patientId)         → enqueues patient
└── seePatient()                     → dequeues and returns next patient ID

Hospital
├── patients: vector<Patient>
├── doctors: vector<Doctor>
├── emergencyQueue: queue<int>        ← FIFO: emergency patient IDs
├── patientCounter, doctorCounter     → auto-increment IDs
│
├── registerPatient(name, age, contact) → creates Patient, returns ID
├── addDoctor(name, Department)         → creates Doctor, returns ID
├── admitPatient(patientId, RoomType)   → finds patient, calls admitPatient()
├── addEmergency(patientId)             → enqueues to emergencyQueue
├── handleEmergency()                   → dequeues next emergency
├── bookAppointment(doctorId, patientId)→ validates both, books appointment
├── displayPatientInfo(patientId)       → prints full patient details
└── displayDoctorInfo(doctorId)         → prints doctor + queue size
```

---

## 🔢 Enumerations

### `Department`
| Value | Label |
|-------|-------|
| `CARDIOLOGY` | Cardiology |
| `NEUROLOGY` | Neurology |
| `ORTHOPEDICS` | Orthopedics |
| `PEDIATRICS` | Pediatrics |
| `EMERGENCY` | Emergency |
| `GENERAL` | General Practice |

### `RoomType`
| Value | Label |
|-------|-------|
| `GENERAL_WARD` | General Ward |
| `ICU` | ICU |
| `PRIVATE_ROOM` | Private Room |
| `SEMI_PRIVATE` | Semi-Private |

---

## 📁 Project Structure

```
G5_PROJECT_2/
│
├── G5_PROJECT_2.cpp              # All source code
│
├── G5_PROJECT_2.slnx             # Visual Studio solution file
├── G5_PROJECT_2.vcxproj          # Visual Studio project file
├── G5_PROJECT_2.vcxproj.filters  # VS filter definitions
│
├── .gitattributes
└── .gitignore
```

---

## 🚀 Getting Started

No external libraries needed — uses only the C++ Standard Library (`iostream`, `string`, `vector`, `stack`, `queue`).

### Build with Visual Studio

1. Open `G5_PROJECT_2.slnx` in **Visual Studio 2022** or later
2. Select **Debug** or **Release** from the build target dropdown
3. Press `Ctrl+Shift+B` to build
4. Run with `Ctrl+F5`

### Build with g++

**Linux / macOS:**
```bash
g++ -std=c++11 -o HospitalSystem G5_PROJECT_2.cpp
./HospitalSystem
```

**Windows (MinGW):**
```bash
g++ -std=c++11 -o HospitalSystem.exe G5_PROJECT_2.cpp
HospitalSystem.exe
```

---

## 🧪 Test Cases

The `main()` function covers 10 test scenarios automatically:

| # | Test | What It Covers |
|---|------|----------------|
| 1 | Register patients | Auto ID assignment, patient creation |
| 2 | Add doctors | Department assignment, ID auto-increment |
| 3 | Admit patients | Room type assignment, double-admission guard |
| 4 | Book appointments | Valid/invalid doctor and patient ID handling |
| 5 | Medical tests | Test request and FIFO performance |
| 6 | Emergency queue | Adding and handling emergencies in order |
| 7 | Discharge patients | Status change on discharge |
| 8 | Display patient info | Full details including admission status and room |
| 9 | Doctor seeing patients | Appointment queue dequeue |
| 10 | Edge cases | Empty hospital — no patients, doctors, or emergencies |

---

## 💡 Example Output

```
Patient registered: John Doe (ID: 1)
Patient registered: Jane Smith (ID: 2)
Patient registered: Mike Johnson (ID: 3)

Doctor added: Dr. Smith (ID: 1) - Cardiology
Doctor added: Dr. Brown (ID: 2) - Neurology
Doctor added: Dr. Lee (ID: 3) - Pediatrics

Patient John Doe admitted to Private Room
Patient Jane Smith admitted to ICU
Patient John Doe is already admitted

Appointment added for patient 1 with Dr. Smith
Appointment added for patient 2 with Dr. Smith
Appointment added for patient 3 with Dr. Brown
Doctor not found.
Patient not found.

Emergency added for patient 3
Emergency added for patient 1
Handling emergency for patient 3
Handling emergency for patient 1
No emergency cases.

=== Patient Information ===
ID: 1
Name: John Doe
Age: 35
Contact: 555-1234
Admission Status: Admitted
Room Type: Private Room

=== Doctor Information ===
ID: 1
Name: Dr. Smith
Department: Cardiology
Pending Appointments: 2

Patient not found.
Doctor Not Found.
No emergency cases.
```

---

## 👥 Authors

**Group 5** — G5_PROJECT_2
