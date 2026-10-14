# API Design

All endpoints start with `/api`. Requests and responses use JSON. Except for register and login, every endpoint needs a JWT token in the `Authorization: Bearer <token>` header.

## Authentication

| Method | URL | Purpose | Who |
|---|---|---|---|
| POST | /api/auth/register | Create a new account (always with the USER role) | Anyone |
| POST | /api/auth/login | Log in and receive a JWT token | Anyone |
| GET | /api/auth/me | Get the logged-in user's profile | Logged-in user |

## Users (admin only)

| Method | URL | Purpose |
|---|---|---|
| GET | /api/users | List users (paginated) |
| GET | /api/users/{id} | Get one user |
| PATCH | /api/users/{id}/role | Change a user's role |
| DELETE | /api/users/{id} | Delete a user |

## Projects

| Method | URL | Purpose | Who |
|---|---|---|---|
| POST | /api/projects | Create a project | Logged-in user (becomes owner) |
| GET | /api/projects | List projects the user belongs to | Logged-in user |
| GET | /api/projects/{id} | Get one project with its progress | Project member |
| PUT | /api/projects/{id} | Update a project | Owner or admin |
| DELETE | /api/projects/{id} | Delete a project | Owner or admin |
| GET | /api/projects/{id}/members | List members | Project member |
| POST | /api/projects/{id}/members | Add a member | Owner or admin |
| DELETE | /api/projects/{id}/members/{userId} | Remove a member | Owner or admin |

## Tasks

| Method | URL | Purpose | Who |
|---|---|---|---|
| POST | /api/tasks | Create a task (the project id goes in the request body) | Project member |
| GET | /api/tasks | Search, filter, sort, and page through tasks | Project member (own projects only) |
| GET | /api/tasks/{id} | Get one task | Project member |
| PUT | /api/tasks/{id} | Update a task | Creator, assignee, project owner, or admin |
| PATCH | /api/tasks/{id}/status | Change only the status (used by the Kanban board) | Creator, assignee, project owner, or admin |
| DELETE | /api/tasks/{id} | Delete a task | Creator, project owner, or admin |

Query parameters for `GET /api/tasks`: `page`, `size`, `sort`, `status`, `priority`, `projectId`, `assigneeId`, `search`.

## Comments

| Method | URL | Purpose | Who |
|---|---|---|---|
| GET | /api/tasks/{taskId}/comments | List comments on a task | Project member |
| POST | /api/tasks/{taskId}/comments | Add a comment | Project member |
| DELETE | /api/comments/{id} | Delete a comment | Author or admin |

## Dashboard

| Method | URL | Purpose | Who |
|---|---|---|---|
| GET | /api/dashboard/stats | Task counts by status and priority for the user | Logged-in user |
| GET | /api/admin/dashboard/stats | Totals across all users, projects, and tasks | Admin only |