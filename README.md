# THM Modulmanager

A module planner for THM students – Java/Groovy, Spring Boot REST API.

## 📌 Project Description
This application allows THM students to plan and manage their study modules.  
You can:
- Add modules with name, number, credits, weight, pool, semester, and grade.
- Filter and search modules.
- (Later) Calculate weighted averages and check graduation requirements.

## 🏗️ Tech Stack
- **Java/Groovy** (mixed codebase)
- **Spring Boot** (REST API)
- **Gradle** (Groovy DSL)
- **Insomnia** (API testing)

## 📂 Project Structure
src/main/groovy/de/thm/modulmanager
├── app/ # Application entry point & configuration
├── controller/ # REST controllers (API endpoints)
├── domain/ # Domain model (entities, enums, value objects)
├── service/ # Business logic
└── repo/ # Data repositories (in-memory or DB access)



## 📊 Domain Model (Mermaid)

```mermaid
classDiagram
direction LR

class Student {
  +studentId: UUID
  +firstName: String
  +lastName: String
  +studentNumber: int
  +vertiefung: Vertiefung
}

class StudyPlan {
  +planId: UUID
  +calculateCrpByPool(): Map<Pool,int>
  +calculateSwsBySemester(): Map<int,int>
  +validate(): ValidationResult
}

class Module {
  +moduleId: UUID
  +moduleName: String
  +moduleNumber: int
  +moduleCredits: int   // CrP
  +moduleWeight: boolean
  +modulePool: Pool
  +moduleSemester: int  // 0..10
  +moduleGrade: int
  +vertiefung: Vertiefung? // only if Pool=Vertiefungspool
}

enum Pool {
  Vertiefungspool
  Wahlpflichtpool
  Ueberfachlicherpool
}

enum Vertiefung {
  Medien
  BWL
  Informatik
}

class PoolRules <<policy>> {
  +minCrp(pool:Pool): int
  +maxCrp(pool:Pool): int
}

class ValidationResult {
  +valid: boolean
  +messages: List<String>
}

Student "1" --> "1" StudyPlan
StudyPlan "1" *-- "*" Module
StudyPlan ..> PoolRules : uses
Module --> Pool
Module ..> Vertiefung : optional

note for StudyPlan
  Rules (short):
  1) SWS in semesters 3–5 derived from modules in all pools (report).
  2) Student selects exactly one Vertiefung for 3–5 incl. GEN1002 + GEN1003.
  3) CrP budgets:
     - Wahlpflichtpool:      27..39 CrP
     - Ueberfachlicherpool:  12..24 CrP
     - Together (2+3):       51 CrP
end note

## 🚀 Getting Started
1. Clone the repository:
   ```bash
   git clone https://github.com/<your-username>/thm-modulmanager.git
   cd thm-modulmanager

2. Run the application: ./gradlew bootRun


3. Test in Insomnia:

GET http://localhost:8080/api/modules

