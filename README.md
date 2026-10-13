# TaskFlow

![Documentation fixture](https://img.shields.io/badge/status-documentation_fixture-blue)
![Angular and TypeScript](https://img.shields.io/badge/frontend-Angular%20%2B%20TypeScript-red)
![.NET 10](https://img.shields.io/badge/backend-.NET%2010-purple)
![Markdown](https://img.shields.io/badge/format-GitHub_Flavored_Markdown-lightgrey)

**A proposed full-stack workspace for keeping team tasks organized, visible, and moving.**

TaskFlow is a fictional task management application documented as an open-source project. This README is also a Markdown rendering fixture for **DEL-990**, covering heading hierarchy, paragraphs, lists, links, code blocks, responsive layout, and readability.

> **Repository status:** The repository currently contains a directory scaffold and empty application/configuration files. The functionality, API contracts, commands, and configuration below are proposed examples, not implemented or tested capabilities. Badges describe the documentation and intended stack; they do not report successful builds.

## Overview

TaskFlow's intended workflow is straightforward: capture a task, assign a priority, set a due date, and move it through _To do_, _In progress_, and _Done_. A compact dashboard would help a team identify overdue work and understand what needs attention without opening every task individually.

The proposed application separates presentation, HTTP request handling, and task business rules. Angular would provide a typed, component-based interface, while ASP.NET Core would expose a JSON API backed by MySQL through Entity Framework Core. This separation would allow developers to iterate on the interface independently of database persistence and make service-level behavior easier to test. The examples in this document establish a coherent target design for a future implementation and supply varied content for Markdown rendering checks.

The proposed scope favors practical team workflows over complicated administration. ~~Email-driven task tracking~~ would give way to searchable task records and explicit status transitions. Authentication, authorization, and activity history would require additional implementation before deployment to a shared environment.

Read the [architecture document](docs/architecture.md) and [getting started guide](docs/getting-started.md) for the intended documentation locations. Both files currently exist as empty scaffold files.

---

## Features

These are **proposed features**, not a report of implemented behavior.

| Feature              | Intended behavior                                                    | Implementation status |
| :------------------- | :------------------------------------------------------------------- | :-------------------: |
| Task creation        | Capture a title, optional description, priority, and due date.       |       Proposed        |
| Status tracking      | Move tasks between To do, In progress, and Done.                     |       Proposed        |
| Search and filters   | Filter by status and search titles without losing the selected view. |       Proposed        |
| Due-date visibility  | Highlight overdue tasks and sort upcoming work by due date.          |       Proposed        |
| Responsive dashboard | Keep primary actions accessible on desktop and narrow screens.       |       Proposed        |
| Persistent storage   | Store task records in MySQL using EF Core.                           |       Proposed        |
| Service tests        | Check validation and state changes with xUnit.                       |       Proposed        |

An intended dashboard could contain:

- A task summary with counts by status.
- A task list with:
  - A title and short description.
  - Priority and due-date indicators.
  - Actions to edit or complete a task.
- An empty state explaining how to create the first task.

![Proposed TaskFlow dashboard showing task status columns, priority labels, and upcoming due dates](docs/images/dashboard.png)

_Screenshot reference only: `docs/images/dashboard.png` is intentionally not supplied. No screenshot has been fabricated._

## Technology Stack

| Layer       | Proposed technology             | Purpose                                          |
| ----------- | ------------------------------- | ------------------------------------------------ |
| Frontend    | Angular with TypeScript         | Components, routing, forms, and typed API access |
| Backend     | ASP.NET Core Web API on .NET 10 | HTTP endpoints and dependency injection          |
| Database    | MySQL                           | Relational task persistence                      |
| Data access | Entity Framework Core           | Entity mapping, queries, and schema migrations   |
| Containers  | Docker and Docker Compose       | Reproducible local services                      |
| Testing     | xUnit                           | Backend unit tests                               |
| Automation  | GitHub Actions                  | Proposed build and test workflow                 |

Useful references include [Angular documentation](https://angular.dev/), [ASP.NET Core documentation](https://learn.microsoft.com/aspnet/core/), [Entity Framework Core documentation](https://learn.microsoft.com/ef/core/), and [GitHub Flavored Markdown](https://github.github.com/gfm/).

## Architecture

The proposed request flow is:

```text
Browser
  -> Angular component
  -> Angular task API service
  -> ASP.NET Core TasksController
  -> TaskService
  -> EF Core database context
  -> MySQL
```

### Frontend boundary

Components would render task data and collect user input. Services would handle HTTP requests and map API responses into TypeScript interfaces. Components should show loading, empty, success, and error states explicitly.

### Backend boundary

Controllers would translate HTTP requests into service calls. `TaskService` would own task validation and lifecycle rules. EF Core would handle persistence through a database context that has not yet been added to this scaffold.

#### Task lifecycle

A task would begin in `Todo`, move to `InProgress`, and eventually reach `Done`. Reopening completed work could be supported as an explicit transition.

##### Validation rules

The proposed validation contract requires a nonblank title of at most 160 characters. Descriptions could be limited to 4,000 characters, and priority values would be constrained to a documented enumeration.

###### Date handling

Due dates would use ISO 8601 calendar dates such as `2030-04-18`. Creation and update timestamps would use UTC instants such as `2030-04-12T09:30:00Z`. The interface would format timestamps for the viewer's locale.

> **Note:** Calendar dates and timestamps serve different purposes. A due date should not silently shift to the previous day when displayed in another time zone.

## Project Structure

The following tree matches the requested TaskFlow scaffold. The pre-existing `src/hello.js` remains in the repository and is outside this proposed application structure.

```text
.
├── .github/
│   └── workflows/
│       └── ci.yml
├── client/
│   ├── src/
│   │   └── app/
│   │       ├── components/
│   │       ├── services/
│   │       └── app.ts
│   └── package.json
├── server/
│   ├── Controllers/
│   │   └── TasksController.cs
│   ├── Models/
│   │   └── TaskItem.cs
│   ├── Services/
│   │   └── TaskService.cs
│   └── Program.cs
├── docs/
│   ├── architecture.md
│   └── getting-started.md
├── tests/
│   └── TaskServiceTests.cs
├── .gitignore
├── docker-compose.yml
├── LICENSE
└── README.md
```

Additional project files, Angular workspace configuration, dependency manifests, Dockerfiles, and a database context would be required to make this scaffold runnable.

---

## Prerequisites

For a future implementation, prepare:

- The .NET 10 SDK.
- A Node.js version supported by the chosen Angular release, with npm.
- MySQL for native local development, or Docker with Compose support.
- Git and a shell such as Bash or PowerShell.
- An EF Core MySQL provider explicitly compatible with the selected EF Core version.

> **Warning:** Provider compatibility must be verified before selecting package versions. This scaffold does not contain a dependency lockfile or a validated .NET 10 database integration.

## Installation

The sequence below illustrates the intended onboarding process once executable project configuration exists. It will **not** build the current empty scaffold.

1. Obtain the repository and open its root directory.
2. Configure local database connectivity as described below.
3. Restore backend dependencies.
4. Install frontend dependencies.
5. Apply database migrations.
6. Start the API and frontend in separate terminals.

Example Bash commands, assuming future project files under `server/`:

```bash
# Run from the repository root after project configuration is implemented.
dotnet restore server
cd client
npm install
cd ..
```

The equivalent PowerShell navigation is:

```powershell
# Run after the backend project and frontend manifest are populated.
dotnet restore .\server
Push-Location .\client
try {
    npm install
} finally {
    Pop-Location
}
```

> **Tip:** After a lockfile is committed, use `npm ci` for reproducible installation in CI and clean development environments.

## Environment Configuration

The examples use fictional local names and contain no passwords, tokens, or API keys. Supply database credentials through your local secret mechanism; do not commit them.

| Setting                       | Example                              | Intended use                  |
| ----------------------------- | ------------------------------------ | ----------------------------- |
| `ASPNETCORE_ENVIRONMENT`      | `Development`                        | Select development behavior   |
| `ASPNETCORE_URLS`             | `http://localhost:5080`              | Bind the local API            |
| `ConnectionStrings__TaskFlow` | Supplied through local configuration | Connect EF Core to MySQL      |
| `TaskFlow__AllowedOrigin`     | `http://localhost:4200`              | Configure the frontend origin |

Illustrative application settings for a future configuration file:

```json
{
  "TaskFlow": {
    "AllowedOrigin": "http://localhost:4200",
    "DefaultPageSize": 25,
    "MaximumPageSize": 100
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  }
}
```

Example shell environment configuration:

```bash
export ASPNETCORE_ENVIRONMENT=Development
export ASPNETCORE_URLS=http://localhost:5080
export TaskFlow__AllowedOrigin=http://localhost:4200
# Set ConnectionStrings__TaskFlow using your local secret mechanism.
```

## Running with Docker

The intended Compose setup would provide three services:

- `client`: the frontend, exposed on port `4200` for development.
- `server`: the API, exposed on port `5080`.
- `db`: MySQL, with data stored in a named volume.

The existing [Compose file](docker-compose.yml) is empty. These commands become applicable after service definitions, Dockerfiles, health checks, and database configuration are implemented:

```bash
docker compose up --build -d
docker compose ps
docker compose logs --follow server
docker compose down
```

> **Warning:** `docker compose down --volumes` would also remove Compose-managed named volumes. Use it only when intentionally discarding local database data.

Inside the container network, the API would connect to the hostname `db`; a browser would still use the published API URL `http://localhost:5080`.

## Running the Frontend

After adding a valid Angular workspace and a `start` script to [client/package.json](client/package.json):

```bash
cd client
npm start -- --port 4200
```

Open `http://localhost:4200` in a browser. A future development proxy could forward `/api` to `http://localhost:5080`, allowing the client service to use relative API paths.

Proposed interface expectations include:

1. Show a loading indicator while fetching tasks.
2. Show an actionable empty state when the list is empty.
3. Preserve entered form values after a validation error.
4. Announce save outcomes in accessible status text.

## Running the Backend

After a backend project file, dependencies, and startup configuration are added:

```powershell
$env:ASPNETCORE_ENVIRONMENT = 'Development'
$env:ASPNETCORE_URLS = 'http://localhost:5080'
dotnet run --project .\server
```

The proposed base URL is `http://localhost:5080/api`. A future OpenAPI document could describe request and response schemas, but no documentation endpoint currently exists in this scaffold.

## Database Migrations

EF Core migrations would version the schema alongside application code. Migration commands require a configured database context, a compatible provider, project files, and the EF CLI tooling; none are established by the empty scaffold.

```bash
# Illustrative commands once EF Core tooling and the context exist.
dotnet ef migrations add InitialCreate --project server --startup-project server
dotnet ef database update --project server --startup-project server
dotnet ef migrations script --idempotent --project server --startup-project server
```

An illustrative query against the proposed schema:

```sql
SELECT Id, Title, Status, Priority, DueDate
FROM TaskItems
WHERE Status <> 'Done'
  AND DueDate IS NOT NULL
ORDER BY DueDate ASC, Id ASC
LIMIT 25;
```

Review generated migrations before applying them to shared data. A production deployment plan would need a documented migration owner, backup procedure, and recovery strategy.

---

## API Endpoints

This table defines an **example contract**. The controller file is currently empty, so these endpoints are not available.

|  Method  | Endpoint                 | Intended behavior                                                           | Example response                     |
| :------: | ------------------------ | --------------------------------------------------------------------------- | ------------------------------------ |
|  `GET`   | `/api/tasks`             | List tasks with optional `status`, `search`, `page`, and `pageSize` filters | `200 OK`                             |
|  `GET`   | `/api/tasks/{id}`        | Retrieve one task                                                           | `200 OK` or `404 Not Found`          |
|  `POST`  | `/api/tasks`             | Create a validated task                                                     | `201 Created` with a location header |
|  `PUT`   | `/api/tasks/{id}`        | Replace editable task fields                                                | `200 OK` or `404 Not Found`          |
| `PATCH`  | `/api/tasks/{id}/status` | Update the task status                                                      | `200 OK`                             |
| `DELETE` | `/api/tasks/{id}`        | Delete a task                                                               | `204 No Content`                     |

Validation errors would use a consistent JSON response with field-specific messages. List responses would include items and pagination metadata so the interface can show accurate result counts.

## Usage Examples

### Create a task

The following request illustrates the proposed API payload:

```bash
curl --request POST 'http://localhost:5080/api/tasks' \
  --header 'Content-Type: application/json' \
  --data '{"title":"Review release checklist","description":"Confirm the migration review and documentation updates.","priority":"High","dueDate":"2030-04-18"}'
```

An example response would be:

```json
{
  "id": 42,
  "title": "Review release checklist",
  "description": "Confirm the migration review and documentation updates.",
  "status": "Todo",
  "priority": "High",
  "dueDate": "2030-04-18",
  "createdAt": "2030-04-12T09:30:00Z"
}
```

### Model an API response in TypeScript

```typescript
export type TaskStatus = "Todo" | "InProgress" | "Done";
export type TaskPriority = "Low" | "Normal" | "High";

export interface TaskItem {
  id: number;
  title: string;
  description: string | null;
  status: TaskStatus;
  priority: TaskPriority;
  dueDate: string | null;
  createdAt: string;
}

export function isCompleted(task: TaskItem): boolean {
  return task.status === "Done";
}
```

### Validate a title in C#

This standalone example shows a potential validation rule; it does not describe an existing service implementation.

```csharp
using System;

public static class TaskTitleRules
{
    public static string Normalize(string title)
    {
        if (string.IsNullOrWhiteSpace(title))
        {
            throw new ArgumentException("A task title is required.", nameof(title));
        }

        var normalized = title.Trim();
        if (normalized.Length > 160)
        {
            throw new ArgumentException("A task title must not exceed 160 characters.", nameof(title));
        }

        return normalized;
    }
}
```

## Testing

The intended xUnit suite would exercise service behavior, including:

- Title validation:
  - Reject empty or whitespace-only titles.
  - Trim surrounding whitespace.
  - Reject titles exceeding the length limit.
- Task lifecycle:
  - Persist a valid creation request.
  - Update a task status.
  - Report missing task IDs consistently.
- Query behavior:
  - Apply status filters.
  - Return stable pagination ordering.

The [test file](tests/TaskServiceTests.cs) is currently empty. After creating an xUnit test project and linking it to the backend, the proposed command is:

```bash
dotnet test tests --configuration Release
```

Database integration tests should run against an isolated MySQL database to check provider-specific behavior. Unit tests alone would not demonstrate that migrations or database queries work correctly.

## CI/CD Pipeline

The intended [GitHub Actions workflow](.github/workflows/ci.yml) would run on pull requests and pushes to the default branch. The file is currently empty; no successful workflow run or deployment is claimed.

Proposed stages:

1. Check out the repository.
2. Set up the selected Node.js version and .NET 10 SDK.
3. Install frontend dependencies from the committed lockfile.
4. Build the Angular application.
5. Restore and build the backend in Release configuration.
6. Run xUnit tests and publish test results.
7. Validate container builds when Dockerfiles exist.

Deployment would be a separate future stage requiring a defined hosting target, environment configuration, and approval policy.

## Troubleshooting

| Symptom                                 | Likely explanation                           | Suggested check                                                 |
| --------------------------------------- | -------------------------------------------- | --------------------------------------------------------------- |
| `npm install` reports invalid JSON      | The scaffold package manifest is empty       | Populate the Angular manifest before running package commands   |
| `dotnet run` cannot find a project      | No backend project file exists yet           | Add the Web API project configuration                           |
| Compose reports no services             | The Compose file is empty                    | Define services and their build contexts                        |
| Browser requests fail after setup       | API URL, proxy, or allowed origin may differ | Compare the browser request URL with the configured API binding |
| Migration tooling cannot find a context | EF Core services or tooling may be missing   | Check context registration and provider compatibility           |

> **Tip:** In a future running environment, inspect the browser's Network panel and API logs together. A server validation response and a connection failure need different fixes.

## Contributing

TaskFlow is a fictional documentation fixture, but the following process models how a team could review future contributions:

1. Describe the problem and expected behavior in an issue.
2. Create a focused branch.
3. Implement the smallest coherent change.
4. Add relevant tests when executable code exists.
5. Update documentation when behavior or setup changes.
6. Open a pull request with a clear description and actual validation results.

A proposed review checklist:

- [ ] The change has a concrete user-facing purpose.
- [ ] API changes include request and response examples.
- [ ] Configuration examples contain no credentials.
- [ ] Relevant automated checks have been run and reported accurately.
- [ ] Narrow-screen and keyboard behavior have been reviewed for UI changes.

Use clear names and keep business rules in services. Prefer explicit error handling over silently ignoring failed operations.

## Roadmap

Checked items indicate completed documentation/scaffold work only.

- [x] Create the requested repository folders and file placeholders.
- [x] Document the proposed stack and API contract.
- [x] Add a broad Markdown rendering fixture.
- [ ] Implement the Angular dashboard and task forms.
- [ ] Implement API controllers and task services.
- [ ] Configure EF Core and MySQL migrations.
- [ ] Add xUnit projects and meaningful tests.
- [ ] Define Docker services and GitHub Actions checks.
- [ ] Design authentication, authorization, and audit history.

## License

The repository includes a [LICENSE](LICENSE) placeholder, but it is currently empty. No license has been selected or granted by this README. A future project owner would need to add the full license text before distributing TaskFlow as a licensed open-source application.

---

## Markdown Rendering Test Cases

This section deliberately combines edge cases with ordinary project content. It supports manual evaluation of DEL-990; its presence does not establish that any renderer has passed those checks.

### Deeply nested list

- Release preparation
  - Application readiness
    - Backend review
      - Persistence review
        - Migration review
          - Confirm ordering and rollback notes.
          - Confirm the sample database contains only fictional records.
    - Frontend review
      1. Open the task list.
      2. Apply a status filter.
         - Confirm the empty state remains readable.
         - Confirm keyboard focus stays visible.
  - Documentation readiness
    - Review the [getting started guide](docs/getting-started.md).

### Long table cells and linked content

| Scenario        | Detailed expected presentation                                                                                                                                                                                                                                                                                                      | Reference                                           |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------- |
| Narrow viewport | This intentionally long cell describes a task whose title, explanatory text, priority, due date, and action labels all need to remain readable when the surrounding documentation panel is narrower than a typical desktop browser window. A renderer may provide horizontal table scrolling while keeping the overall page usable. | [Architecture notes](docs/architecture.md)          |
| Large paragraph | Text should wrap naturally within the available space, preserving emphasis such as **required fields**, inline values such as `pageSize=25`, and links without overlapping neighboring cells or truncating content unexpectedly.                                                                                                    | [GFM specification](https://github.github.com/gfm/) |

### Long code block

This fictional TypeScript fixture provides enough lines to inspect indentation, spacing, and code-block scrolling. It is not application code in this repository.

```typescript
type FixtureStatus = "Todo" | "InProgress" | "Done";

interface FixtureTask {
  id: number;
  title: string;
  status: FixtureStatus;
  dueDate: string | null;
}

const fixtureTasks: FixtureTask[] = [
  {
    id: 101,
    title: "Review keyboard navigation on the task form",
    status: "Todo",
    dueDate: "2030-04-18",
  },
  {
    id: 102,
    title: "Document the proposed pagination response",
    status: "InProgress",
    dueDate: "2030-04-19",
  },
  {
    id: 103,
    title: "Prepare fictional task data for rendering checks",
    status: "Done",
    dueDate: null,
  },
];

function selectTasks(
  tasks: readonly FixtureTask[],
  status: FixtureStatus | "All",
  search: string,
): FixtureTask[] {
  const query = search.trim().toLowerCase();

  return tasks
    .filter((task) => status === "All" || task.status === status)
    .filter((task) => task.title.toLowerCase().includes(query))
    .sort((left, right) => left.id - right.id);
}

function summarize(tasks: readonly FixtureTask[]): string {
  const completed = tasks.filter((task) => task.status === "Done").length;
  return `${completed} of ${tasks.length} tasks completed`;
}

const visibleTasks = selectTasks(fixtureTasks, "All", "review");
console.log(summarize(fixtureTasks));

for (const task of visibleTasks) {
  const dueLabel = task.dueDate ?? "No due date";
  console.log(`${task.id}: ${task.title} (${task.status}) — ${dueLabel}`);
}
```

### Formatted blockquote

> **Rendering note:** Preserve _emphasis_, ~~removed wording~~, and inline code such as `TaskStatus.Done` inside a quotation.
>
> A second paragraph should remain visibly part of the same blockquote and include a working [relative documentation link](docs/architecture.md).
>
> - Quoted list item with **bold text**.
> - Another quoted item with `inline code`.

### Mixed task list

- [x] Include heading levels H1 through H6.
- [x] Include fenced examples with language identifiers.
- [ ] Check table overflow in the target renderer.
- [ ] Check missing-image presentation in the target renderer.
- [ ] Verify scrolling and readability at narrow widths.

### Escaped syntax and special characters

The table pairs Markdown source with escaped text intended to display literally.

| Syntax being inspected          | Escaped literal example           |
| ------------------------------- | --------------------------------- |
| `*italic*`                      | \*italic\*                        |
| `**bold**`                      | \*\*bold\*\*                      |
| `~~removed~~`                   | \~\~removed\~\~                   |
| `[label](docs/architecture.md)` | \[label\]\(docs/architecture.md\) |
| `# Heading`                     | \# Heading                        |
| Backtick delimiters             | \`inline code\`                   |
| Pipe inside a table cell        | left \| right                     |

Literal punctuation: \<task\>, 3 \> 2, planning & review, "double quotes", 'single quotes', and escaped backticks: \`TaskFlow\`.

Inline code preserves characters literally: `<`, `>`, `&`, `"quoted"`, and `'quoted'`. A code span can also contain backticks: `` `TaskFlow` ``.

The following fenced block preserves raw Markdown characters without interpreting them:

```text
*asterisks* _underscores_ **bold markers** ~~strike markers~~
[link label](docs/getting-started.md)
< > & "double quotes" 'single quotes' `backticks`
\*escaped asterisk\* \[escaped brackets\] \#escaped heading
```

### Horizontal overflow

This intentionally unbroken fictional identifier tests how ordinary text handles horizontal overflow:

TASKFLOW_RENDERING_FIXTURE_ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789

### Missing image and relative navigation

![Intentionally missing image used to inspect fallback alt text and broken-image layout](docs/images/deliberately-missing-renderer-fixture.png)

The image above deliberately references a nonexistent file. Follow the [getting started documentation](docs/getting-started.md) to exercise relative Markdown navigation to an existing scaffold document.

---

End of the TaskFlow Markdown fixture.
