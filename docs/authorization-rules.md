# Authorization Rules

## Roles

- **USER:** the default role for everyone who registers.
- **ADMIN:** can manage users and has full access to all projects and tasks.

Nobody can change their own role. Registration always creates a USER, and only an ADMIN can change another user's role.

## Rules by area

| Area | Rule |
|---|---|
| Register and login | Open to everyone. All other endpoints need a valid JWT token. |
| User management | Only an ADMIN can list, view, change the role of, or delete users. |
| Projects | A user sees only the projects they own or belong to. Only the project owner or an ADMIN can edit or delete a project and manage its members. |
| Tasks | Only project members can create tasks in a project and see its tasks. |
| Editing tasks | The task creator, the assignee, the project owner, or an ADMIN can edit a task or change its status. |
| Deleting tasks | The task creator, the project owner, or an ADMIN can delete a task. |
| Assignee | A task can only be assigned to a member of the same project. |
| Comments | Project members can read and add comments. Only the comment author or an ADMIN can delete a comment. |
| Dashboard | A user sees statistics only for their own projects and tasks. Only an ADMIN sees system-wide statistics. |

## How the backend enforces this

- The JWT token proves who the user is. The backend never trusts a user id or role sent by the browser.
- Every service method checks the rule before reading or changing data.
- A user without permission gets `403 Forbidden`. A request without a valid token gets `401 Unauthorized`.
- If a user asks for a project or task they cannot access, the backend answers `404 Not Found`, so it does not reveal that the item exists.