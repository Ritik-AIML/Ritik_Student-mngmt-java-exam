# Student Profile and Skill Development Management System

Spring Boot REST API for storing student profiles, scores, courses, and technical skills in MySQL.

## Prerequisites

- Java 17
- MySQL 8+
- A MySQL account allowed to create/use the `student_management` database

The application creates the database (when the account permits it) and its tables on startup. Set database values before running; do not commit a real password.

```powershell
$env:DB_URL = "jdbc:mysql://localhost:3306/student_management?createDatabaseIfNotExist=true&serverTimezone=UTC"
$env:DB_USERNAME = "root"
$env:DB_PASSWORD = "your-password"
.\mvnw.cmd spring-boot:run
```

Run tests with:

```powershell
.\mvnw.cmd test
```

## API

| Method | Endpoint | Purpose |
|---|---|---|
| POST | `/api/students` | Add a student |
| GET | `/api/students` | Retrieve all students |
| GET | `/api/students/{studentId}` | Retrieve one student |
| GET | `/api/students/search?id=1` | Search by ID (`email` or `course` are alternatives) |
| PUT | `/api/students/{studentId}` | Update programming score, mathematics score, and skills |
| DELETE | `/api/students/{studentId}` | Delete a student |
| GET | `/api/students/merit-list` | List students by average score descending |
| GET | `/api/students/shared-skills` | Show technical skills shared by multiple students |
| GET | `/api/students/programming-improvement` | Programming below 50 and mathematics at least 75 |

Example create request:

```json
{
  "studentId": 101,
  "studentName": "Asha Sharma",
  "email": "asha@example.com",
  "courseName": "BCA",
  "programmingScore": 88,
  "mathematicsScore": 72,
  "technicalSkills": ["Java", "SQL", "Spring Boot"]
}
```

Example score/skill update request:

```json
{
  "programmingScore": 91,
  "mathematicsScore": 82,
  "technicalSkills": ["Java", "MySQL", "Spring Boot"]
}
```

Every student response includes the calculated `averageScore`, `performanceCategory`, and `strongerSubject`. Scores must be from 0 through 100; duplicate student IDs return HTTP 409.
