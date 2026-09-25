# ProjectFlow — Secure Project Management Backend

> **Production-oriented REST API for multi-user project management, built with Spring Boot, JWT security and relational persistence.**

ProjectFlow is the **backend/API layer** of a full-stack project-management platform designed for organizations managing multiple projects, employees and work assignments.

The platform supports **multiple authenticated users with role-based access**, centralized project management, employee administration, categories and project–employee assignments through a structured REST API.

It is designed as the backend counterpart to the **ProjectHub Angular frontend**, forming a complete full-stack application architecture.

---

## ✨ Core Capabilities

* 🔐 **JWT-based authentication**
* 👥 **Multi-user access with role-based authorization**
* 🧑‍💼 Employee management
* 📁 Project lifecycle management
* 🏷️ Employee category management
* 🔗 Project–employee assignment management
* 🗓️ Assignment start/end dates
* 🛡️ BCrypt password hashing
* 🗄️ H2 development database
* 🐘 PostgreSQL production configuration
* 🌐 CORS configuration for frontend integration
* 🐳 Dockerized deployment
* ⚙️ Environment-based production configuration
* 🧱 Layered Spring architecture: Controller → Service → Repository → Database

---

## 🏗️ Architecture

```text
                         ┌──────────────────────┐
                         │      ProjectHub      │
                         │   Angular Frontend   │
                         └──────────┬───────────┘
                                    │ REST / JSON
                                    ▼
                    ┌─────────────────────────────┐
                    │        ProjectFlow API      │
                    │       Spring Boot 3.5       │
                    └──────────────┬──────────────┘
                                   │
                    ┌──────────────▼──────────────┐
                    │     Spring Security + JWT   │
                    │  Authentication / RBAC      │
                    └──────────────┬──────────────┘
                                   │
                    ┌──────────────▼──────────────┐
                    │        REST Controllers     │
                    └──────────────┬──────────────┘
                                   │
                    ┌──────────────▼──────────────┐
                    │           Services          │
                    │ Business Logic / Workflows  │
                    └──────────────┬──────────────┘
                                   │
                    ┌──────────────▼──────────────┐
                    │        JPA / Hibernate       │
                    └──────────────┬──────────────┘
                                   │
                         ┌─────────▼─────────┐
                         │ H2 / PostgreSQL   │
                         └───────────────────┘
```

The architecture separates **HTTP/API concerns, business logic, security and persistence**, making the backend easier to maintain and extend.

---

## 🔐 Security Architecture

Security is implemented as a first-class backend concern.

### Authentication

* User registration
* Login using email/password
* BCrypt password hashing
* JWT token generation
* Bearer-token authentication
* Stateless Spring Security sessions

### Authorization

The platform currently defines two application roles:

```text
ADMIN
EMPLOYE
```

Protected API resources are secured through Spring Security and the JWT filter.

The JWT contains the authenticated user's identity and role, allowing the backend to establish the corresponding Spring Security authorities.

### Request Flow

```text
Client
  │
  ▼
Authorization: Bearer <JWT>
  │
  ▼
JwtFilter
  │
  ├── Validate token
  ├── Extract user identity
  ├── Load employee
  └── Build authenticated SecurityContext
          │
          ▼
   Protected REST Endpoint
```

---

## 📦 Domain Model

The backend models the main business entities required for collaborative project management.

```text
                 ┌──────────────┐
                 │  Categorie   │
                 └──────┬───────┘
                        │
                        ▼
                 ┌──────────────┐
                 │   Employe    │
                 └──────┬───────┘
                        │
                        │
                 ┌──────▼───────┐
                 │  Affectation │
                 └──────┬───────┘
                        │
                        ▼
                 ┌──────────────┐
                 │    Projet    │
                 └──────────────┘
```

### Main entities

| Entity        | Responsibility                                    |
| ------------- | ------------------------------------------------- |
| `Employe`     | Users/employees participating in the platform     |
| `Projet`      | Projects managed by the organization              |
| `Categorie`   | Employee classification                           |
| `Affectation` | Links employees to projects with assignment dates |

This enables the application to represent **who is working on which project and during which period**.

---

## 🚀 REST API

### Authentication

| Method | Endpoint             | Purpose             |
| ------ | -------------------- | ------------------- |
| `POST` | `/api/auth/login`    | Authenticate a user |
| `POST` | `/api/auth/register` | Register a new user |

### Projects

| Method   | Endpoint            | Purpose            |
| -------- | ------------------- | ------------------ |
| `GET`    | `/api/projets`      | List projects      |
| `GET`    | `/api/projets/{id}` | Retrieve a project |
| `POST`   | `/api/projets`      | Create a project   |
| `PUT`    | `/api/projets/{id}` | Update a project   |
| `DELETE` | `/api/projets/{id}` | Delete a project   |

### Employees

```text
/api/admin/employes
```

Supports employee listing, retrieval, creation, update and deletion.

### Categories

```text
/api/admin/categories
```

Supports category management through REST operations.

### Assignments

```text
/api/admin/affectations
```

Supports:

* Listing assignments
* Filtering assignments by employee
* Filtering assignments by project
* Creating project assignments
* Removing assignments

---

## 🛠️ Technology Stack

### Backend

* **Java 17**
* **Spring Boot 3.5.13**
* Spring Web
* Spring Security
* Spring Data JPA
* Hibernate
* Bean Validation
* Lombok

### Security

* JWT / JJWT `0.12.3`
* BCrypt
* Stateless authentication
* Role-based authorization
* Security filter chain

### Data

* PostgreSQL
* H2
* JPA / Hibernate

### Engineering & Deployment

* Maven
* Docker
* Environment-based configuration
* REST / JSON API

---

## 🐳 Docker

The project includes a multi-stage Docker build:

```text
Maven + Java 17
      │
      ▼
Build Spring Boot application
      │
      ▼
Lightweight Java 17 runtime image
      │
      ▼
Run backend JAR
```

Build:

```bash
docker build -t projectflow-backend .
```

Run:

```bash
docker run -p 8081:8081 projectflow-backend
```

The containerized architecture separates the build environment from the runtime environment and provides a reproducible deployment path.

---

## 💻 Local Development

### Requirements

* Java 17+
* Maven
* PostgreSQL (production configuration)
* Docker — optional

### Start the application

```bash
mvn spring-boot:run
```

The development configuration uses an in-memory H2 database.

Default backend port:

```text
8081
```

H2 console:

```text
/h2-console
```

---

## 🌍 Production Configuration

The backend includes a dedicated production configuration using PostgreSQL and environment variables.

Supported configuration includes:

```text
SPRING_DATASOURCE_URL
SPRING_DATASOURCE_USERNAME
SPRING_DATASOURCE_PASSWORD
PORT
```

Activate the production profile with:

```bash
SPRING_PROFILES_ACTIVE=prod
```

The deployed architecture is therefore separated into:

```text
Angular / ProjectHub
        │
        │ HTTPS REST API
        ▼
ProjectFlow / Spring Boot
        │
        ▼
PostgreSQL
```

---

## 🔄 ProjectHub Integration

ProjectFlow is designed as the **backend service layer** for the ProjectHub frontend.

The Angular application communicates with the deployed API through REST endpoints and automatically attaches the JWT bearer token to authenticated requests.

```text
ProjectHub
Angular Client
     │
     │ HTTP + JWT
     ▼
ProjectFlow
Spring Boot REST API
     │
     ▼
PostgreSQL
```

This separation provides a clean **frontend/backend architecture** rather than coupling the UI directly to persistence.

---

## 📁 Project Structure

```text
src/
└── main/
    ├── java/
    │   └── com/gestionprojets/backend/
    │       ├── config/
    │       ├── controller/
    │       ├── model/
    │       ├── repository/
    │       ├── security/
    │       ├── service/
    │       └── BackendApplication.java
    │
    └── resources/
        ├── application.properties
        └── application-prod.properties

Dockerfile
pom.xml
```

The backend follows a classic layered architecture with clear separation between:

**API → Business Logic → Persistence → Database**

---

## 🧠 Engineering Highlights

This project goes beyond a basic CRUD implementation by combining:

* Secure authentication and authorization
* Multi-user application design
* Role-aware access control
* Relational domain modeling
* Assignment/workforce management
* Development and production database profiles
* Containerized backend deployment
* Frontend/backend separation
* Environment-driven deployment configuration

It demonstrates practical experience with the architecture required to turn a project-management application into a **deployable full-stack system**.

---

## 🔒 Production Hardening Opportunities

The current implementation provides a strong functional foundation, while several areas can be further hardened for a larger production environment:

* Move JWT signing secrets to environment/secret management
* Restrict CORS to trusted frontend origins
* Introduce Flyway or Liquibase for controlled database migrations
* Replace schema recreation with migration-based production evolution
* Remove development/demo credentials from production initialization
* Add richer API-level validation and standardized error responses
* Expand automated integration and security testing
* Add structured logging and observability
* Introduce CI/CD quality gates

These improvements provide a clear path from the current deployed academic/portfolio system toward a more operationally hardened production platform.

---

## 🚀 Future Evolution

Potential extensions include:

* Refresh-token authentication
* Advanced project filtering and pagination
* Project activity history
* Notifications
* Audit logging
* OpenAPI / Swagger documentation
* CI/CD pipelines
* Monitoring and centralized logging
* Fine-grained permissions
* Advanced reporting and analytics

---

## 👩‍💻 Author

**Erij Kacem**

Computer Engineering Student
 AI · Data & Cloud Systems .Software Engineering · Backend 

---

## 📄 License

No separate open-source license is currently declared in the repository.

Unless a license is added, the source code should be considered available for portfolio and demonstration purposes without granting broad redistribution rights.
