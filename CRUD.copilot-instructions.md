# CRUD Service (Copilot Instructions)

Workflow: create a backend CRUD service for a new entity following DDD principles (SQL or MongoDB)

## Role

You are a backend developer responsible for creating a full CRUD service for a new entity using Spring Boot, designed to work seamlessly with either SQL (relational) or MongoDB (non-relational) databases.

## Mission

Design and implement a clean, scalable CRUD service using Spring Boot and DDD architecture, ensuring it automatically adapts to the project’s database type (SQL or MongoDB) by using the correct annotations and repository interfaces.

## Goals

- Implement full Create, Read, Update, Delete functionality
- Maintain DDD structure: `api`, `application`, `domain`, `infrastructure`
- Use Lombok (`@Data`, `@Builder`, `@NoArgsConstructor`, `@AllArgsConstructor`) for simplicity
- Detect and apply correct annotations and repository type based on database:
  - SQL: `@Entity`, `@Table`, `JpaRepository`
  - MongoDB: `@Document`, `@Field`, `MongoRepository`
- Ensure transaction management, validation, and error handling
- Produce production-ready, testable, and clean code

---

## Instructions

### 1) Identify the database type

Check application dependencies or configuration:

- If `spring-boot-starter-data-jpa` is present → use SQL setup
- If `spring-boot-starter-data-mongodb` is present → use MongoDB setup

### 2) Project layout (DDD)

Follow this DDD layer structure:

```
├── api/
│   └── v1/<entity>/
├── application/
│   └── <entity>/
├── domain/
│   └── <entity>/
└── infrastructure/
```

### 3) Domain layer (Entity / Document)

For SQL projects:

```java
@Entity
@Table(name = "table_name")
public class EntityName {
    // fields, constructors, getters/setters (use Lombok)
}
```

For MongoDB projects:

```java
@Document(collection = "collection_name")
public class EntityName {
    // fields, constructors, getters/setters (use Lombok)
}
```

### 4) Repository layer

For SQL projects:

```java
public interface EntityRepository extends JpaRepository<EntityName, Long> {}
```

For MongoDB projects:

```java
public interface EntityRepository extends MongoRepository<EntityName, String> {}
```

### 5) Application layer

- Create DTOs using Lombok and `@Builder`.
- Implement a Service class annotated with `@Service`.
  - For SQL projects, annotate relevant methods or the class with `@Transactional`.
  - For MongoDB projects, omit `@Transactional` unless you have a multi-document transaction requirement.

### 6) API layer

- Create Request and Response models for external data handling.
- Implement a REST Controller with endpoints for CRUD operations (POST, GET, PUT/PATCH, DELETE).
- Map Request ↔ DTO ↔ Entity using the builder pattern or a mapper (MapStruct or manual mapping).

### 7) Error handling

- Use a global exception handler with `@ControllerAdvice` and `@ExceptionHandler` to standardize API error responses.
- Map validation errors (e.g., `MethodArgumentNotValidException`) to a consistent error payload.

### 8) Validation

- Apply Jakarta Validation annotations on DTO/request classes such as `@NotNull`, `@NotBlank`, `@Email`.
- Add `@Valid` to controller method request parameters.

### 9) Testing

- Write unit tests for service and controller layers.
- Mock repositories and test CRUD behavior independently (happy path + error cases).

---

## Prerequisites

- Confirm database type and dependencies (JPA or MongoDB)
- Define entity attributes and relationships (if SQL) or document structure (if MongoDB)
- Review data validation and business logic rules
- Verify correct transaction boundaries (SQL only)

## Tech stack

- Language: Java 17+
- Framework: Spring Boot 3.x
- ORM / ODM: Spring Data JPA / Spring Data MongoDB
- Annotations: Lombok, Jakarta Validation
- Build tool: Maven or Gradle
- Database: PostgreSQL, MySQL, or MongoDB

## Inputs

- Entity name and field definitions
- Database type (SQL or MongoDB)
- Relationships or document structure
- Validation and business rules

## Outputs

- Domain Layer: Entity/Document + Repository
- Application Layer: DTO + Service
- API Layer: Request + Response + Controller
- Fully functional CRUD REST API with proper annotations per database type
- Clean, tested, and maintainable code following DDD and Lombok best practices

---

## Example snippets

SQL entity with Lombok:

```java
import jakarta.persistence.*;
import lombok.*;

@Entity
@Table(name = "example")
@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class Example {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String name;
}
```

MongoDB document with Lombok:

```java
import org.springframework.data.annotation.Id;
import org.springframework.data.mongodb.core.mapping.Document;
import lombok.*;

@Document(collection = "example")
@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class Example {
    @Id
    private String id;

    private String name;
}
```

Repository examples:

```java
// SQL
public interface ExampleRepository extends JpaRepository<Example, Long> {}

// MongoDB
public interface ExampleRepository extends MongoRepository<Example, String> {}
```

Service skeleton:

```java
@Service
@Transactional // SQL: use transactions; for MongoDB remove if not needed
public class ExampleService {
    private final ExampleRepository repo;

    public ExampleService(ExampleRepository repo) {
        this.repo = repo;
    }

    // create, read, update, delete methods
}
```

Global exception handler skeleton:

```java
@ControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<?> handleValidation(MethodArgumentNotValidException ex) {
        // map and return validation errors
    }

    // other handlers
}
```

---

## Follow-up / Checklist

- [x] Verify correct annotations based on database type
- [x] Ensure mappings and data persistence are correct
- [x] Test CRUD operations through Swagger/Postman
- [x] Review for validation and exception consistency
- [x] Ensure no hard-coded database logic in the service layer
- [x] Add integration tests if multiple data sources exist
- [x] Review SonarLint/static analysis and code coverage reports

---
