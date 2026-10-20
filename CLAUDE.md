# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

**veni** is a web application for managing waitlists in cafes and shops. It is one project inside a program called "cert" and is licensed under GPL-3.0 (see `LICENSE`).

## Current state: requirements only, no code yet

The repository holds no application code, solution file, or build configuration. The two Markdown files at the root are the source of truth for scope and constraints; read them before scaffolding anything:

- `technical-requirements.md` — the mandated stack (summarised below).
- `project-management-requirements.md` — how the project is run (summarised below).

Once a solution exists, replace the "After scaffolding" section with the real project paths and any scripts that get added.

## Mandated tech stack

`technical-requirements.md` fixes the stack; do not substitute alternatives:

- C# on **.NET 8.0** with **ASP.NET Core** for the web app.
- **Entity Framework Core** for data access, **SQL Server** as the database.
- Frontend is server-rendered: **Bootstrap** plus plain HTML/CSS for responsive layout and vanilla **JavaScript** for interactivity. The requirements do not call for a SPA framework, TypeScript, or a Node build step, so don't introduce one.

### Local environment (verified on this machine)

- .NET SDKs 6.0, 7.0, **8.0.414**, 9.0 and 10.0 are all installed. `dotnet` defaults to the newest SDK, so pin the target explicitly (`-f net8.0` when scaffolding, `<TargetFramework>net8.0</TargetFramework>` in project files) and add a root `global.json` selecting the 8.0 SDK so builds match the requirement.
- A SQL Server LocalDB instance named `MSSQLLocalDB` exists and `sqlcmd` is on PATH, so a `Server=(localdb)\MSSQLLocalDB;Database=veni;Trusted_Connection=True;` style connection string works for development without a separate SQL Server install.
- The `dotnet-ef` global tool (10.0.8) is already installed, so `dotnet ef` commands work once a project references `Microsoft.EntityFrameworkCore.Design`.

## After scaffolding: commands

Standard .NET CLI, run from the repository root once a solution/project exists:

```bash
dotnet build                                     # compile
dotnet run --project <web-project>               # run the site locally
dotnet test                                      # run all tests
dotnet test --filter "FullyQualifiedName~<TestClassOrMethod>"   # run a single test
dotnet format                                    # formatter + analyzers
dotnet ef migrations add <Name> --project <data-project> --startup-project <web-project>
dotnet ef database update      --project <data-project> --startup-project <web-project>
```

## Project management constraints that affect the work

`project-management-requirements.md` puts the project under PMI governance. The parts that change how engineering work is done:

- Delivery is in four phases: feature planning, development, testing, deployment, each closed by a phase review. Building the app is the project; running it afterwards is operations and out of project scope.
- The life cycle is iterative and change-driven, so requirements are expected to evolve; prefer small increments over big-bang changes.
- A risk register and issue log are maintained for the life of the project. When work surfaces a new risk or issue, record it there rather than only in a commit message or chat.
- Any PM documents (plans, registers, logs, phase reviews) should follow the PMI sample templates and include the principles, constraints and phase reviews that file calls for.

## Git

- Default branch is `main`. The `origin` remote points at GitHub (`mabubakarriaz/veni`) through the SSH host alias `github-personal`, so pushes depend on that alias existing in `~/.ssh/config`. If `gh` cannot resolve the remote, pass `--repo mabubakarriaz/veni`.
