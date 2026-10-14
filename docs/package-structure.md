# Backend Package Structure

The backend lives in the `backend/` folder. Java code is grouped by layer (what each class does) under one base package, `com.example.taskmanager`.

```
backend/
  pom.xml
  src/
    main/
      java/com/example/taskmanager/
        TaskManagerApplication.java
        config/          application settings (OpenAPI, CORS)
        controller/      REST controllers (receive requests, return responses)
        dto/             request and response objects
        entity/          classes that map to database tables
        exception/       custom exceptions and the global error handler
        repository/      interfaces that talk to the database
        security/        JWT, login, and Spring Security setup
        service/         business rules and permission checks
      resources/
        application.yml
        db/migration/    Flyway SQL files (V1__..., V2__...)
    test/
      java/com/example/taskmanager/   automated tests
```

## Rules we will follow

- A controller never talks to a repository directly. It calls a service.
- Entities are never sent straight to the client. DTOs are.
- Permission checks live in services, not in controllers.
- Every table change is a new Flyway migration file. We never edit a migration that has already run.