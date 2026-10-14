# Database Design

```mermaid
erDiagram
    USERS ||--o{ PROJECTS : owns
    USERS ||--o{ PROJECT_MEMBERS : "joins as member"
    PROJECTS ||--o{ PROJECT_MEMBERS : has
    PROJECTS ||--o{ TASKS : contains
    USERS ||--o{ TASKS : creates
    USERS |o--o{ TASKS : "is assigned"
    TASKS ||--o{ COMMENTS : has
    USERS ||--o{ COMMENTS : writes

    USERS {
        bigint id PK
        varchar name
        varchar email "unique"
        varchar password_hash
        varchar role "ADMIN or USER"
        timestamp created_at
    }
    PROJECTS {
        bigint id PK
        varchar name
        text description
        bigint owner_id FK
        timestamp created_at
    }
    PROJECT_MEMBERS {
        bigint project_id PK, FK
        bigint user_id PK, FK
        timestamp joined_at
    }
    TASKS {
        bigint id PK
        varchar title
        text description
        varchar status "TODO, IN_PROGRESS, DONE"
        varchar priority "LOW, MEDIUM, HIGH"
        date due_date
        bigint project_id FK
        bigint creator_id FK
        bigint assignee_id FK "optional"
        timestamp created_at
        timestamp updated_at
    }
    COMMENTS {
        bigint id PK
        text content
        bigint task_id FK
        bigint author_id FK
        timestamp created_at
    }
```

## Relationships in plain words

- One user can own many projects.
- A project has many members, and a user can be a member of many projects. The `project_members` table connects them.
- A project contains many tasks, and every task belongs to exactly one project.
- Every task has a creator. It may also have an assignee, who can be empty.
- A task has many comments, and each comment has one author.