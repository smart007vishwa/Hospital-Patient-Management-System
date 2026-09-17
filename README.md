# Hospital Patient Management System

A modern, responsive CRUD dashboard for hospital staff. It supports patient registration, search, detail views, editing, deletion, validation, dashboard totals, and an optional sample-data loader.

## What is included

- `src/` — the React + Vite dashboard used by the live preview.
- `backend/` — a complete Spring Boot 3 + Java 21 + MySQL REST API implementation matching the requested college-project stack.
- `lib/api-spec/openapi.yaml` — the shared REST contract in the full Replit project.

## Live preview in Replit

The live preview uses the workspace's pre-configured Express API and PostgreSQL database so it can run immediately inside Replit. It exposes the same `/api/patients` and `/api/dashboard/summary` contract as the Spring Boot implementation.

```bash
pnpm --filter @workspace/api-server run dev
pnpm --filter @workspace/hospital-patient-management run dev
```

Open the web preview at `/`. The dashboard starts empty; use **Load samples** to insert the two clearly identified demo records.

## Spring Boot + MySQL backend

The downloadable backend is in `backend/`.

1. Create the database:

   ```sql
   CREATE DATABASE hospital_db;
   ```

2. Set the database environment variables. The password is intentionally not stored in the source:

   ```bash
   export DB_URL=jdbc:mysql://localhost:3306/hospital_db
   export DB_USERNAME=root
   export DB_PASSWORD=your-local-password
   ```

3. Start the API:

   ```bash
   cd backend
   mvn spring-boot:run
   ```

The API runs on `http://localhost:8080` by default. Hibernate creates or updates the `patients` table with `spring.jpa.hibernate.ddl-auto=update`.

## API endpoints

| Method | Endpoint | Purpose |
| --- | --- | --- |
| `GET` | `/api/patients` | List all patient records |
| `POST` | `/api/patients` | Create a patient |
| `GET` | `/api/patients/{id}` | View one patient |
| `PUT` | `/api/patients/{id}` | Update a patient |
| `DELETE` | `/api/patients/{id}` | Delete a patient |
| `GET` | `/api/patients/search?name=Arun` | Search by patient name |
| `POST` | `/api/patients/seed-samples` | Add demo records only when requested |
| `GET` | `/api/dashboard/summary` | Return dashboard totals |

## Sample JSON

```json
{
  "patientName": "Arun Kumar",
  "age": 25,
  "gender": "Male",
  "phone": "9876543210",
  "email": "arun@example.com",
  "address": "Kalyan Nagar, Bengaluru",
  "bloodGroup": "B+",
  "disease": "Fever",
  "doctorName": "Dr. Kumar",
  "admissionDate": "2026-09-17"
}
```

## Beginner-friendly viva notes

- `Patient.java` is the entity that maps Java fields to the MySQL table.
- `PatientRepository` talks to the database through Spring Data JPA.
- `PatientService` keeps CRUD rules out of the controller.
- `PatientController` exposes the REST endpoints and returns JSON.
- The React UI uses `fetch` through generated API hooks, so saving or deleting a record updates the database and refreshes the visible list.
- Validation is applied in the browser for quick feedback and again on the server with Jakarta Validation.