# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

**veni** is a web application for managing waitlists in cafes and shops. It is one project inside a program called "cert" and is licensed under GPL-3.0 (see `LICENSE`).

## Current state: requirements and PM templates, no code yet

The repository holds no application code, solution file, or build configuration. Three things exist and are the source of truth; read them before scaffolding or drafting anything:

- `technical-requirements.md` — the mandated stack (summarised below).
- `project-management-requirements.md` — how the project is run (summarised below).
- `docs/pmi/` — the PMI document templates and a filled sample project (described below).

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
- For reading the PDFs in `docs/pmi/`: `pdftotext` (poppler) is on PATH and Python 3.13 has `pypdf` installed. `pdftotext -layout "<file>.pdf" -` prints a template's text to stdout.

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

`project-management-requirements.md` puts the project under PMI governance (PMBOK 8th edition). The parts that change how engineering work is done:

- Delivery is in four phases: feature planning, development, testing, deployment, each closed by a phase review. Building the app is the project; running it afterwards is operations and out of project scope.
- The life cycle is iterative and change-driven, so requirements are expected to evolve; prefer small increments over big-bang changes. Scope changes go through the change-request documents, not just a commit.
- A risk register and issue log are maintained for the life of the project. When work surfaces a new risk or issue, record it there rather than only in a commit message or chat.
- Governance details that the PM documents must reflect: a **controlling PMO**, a **projectized** organisation, **servant leadership**, all **six constraints** (scope, schedule, cost, quality, resources, risk), all **six PMBOK 8 principles** written into the project management plan, and coverage of every focus area and performance domain in `docs/pmi/PMBOK 8 Process Chart.pdf`.

## PMI project documents (`docs/pmi/`)

The project manager owns these documents and keeps them current; Claude drafts and updates them on request. The requirements file says AI will be used to fill them.

### Layout

- One subfolder per PMBOK 8 process, named after the process in `PMBOK 8 Process Chart.pdf` (for example `Initiate Project or Phase/`, `Develop Schedule/`, `Identify Risk and Plan risk Respone/`).
- Each subfolder holds pairs of PDFs: `<Document>_BLANK.pdf` is the empty template to fill for veni; `<Document>_FILLED.pdf` is the same template completed for a **dummy project, "Mary's Consulting - New Company Website"**. The sample exists only to show how a document should be filled. It has nothing to do with veni.
- `Marys_Consulting_PM_Plan.pdf` (60 pages, 18 sections: subsidiary management plans, the scope/schedule/cost/performance baselines, life-cycle description, development approach) and `Marys_Consulting_Project_Documents.pdf` (92 pages, 33 supporting documents) are the compiled sample. Use them as the model for the structure, ordering and level of detail of veni's own compiled plan and documents.
- `PMBOK 8 Process Chart.pdf` maps the five focus areas (initiating, planning, executing, monitoring and controlling, closing) against the seven performance domains (governance, scope, schedule, finance, stakeholders, resources, risk). The requirement to cover "all focus areas and domains" means every cell in that chart.

### Rules for filling a template

- Mirror the template's headings, tables and field order exactly; the FILLED sample shows the expected depth and tone. Never copy Mary's Consulting facts (people, dates, budgets, vendors, website scope) into a veni document.
- Some BLANK templates carry residue from the sample (for example `Source_Selection_Criteria_BLANK.pdf` names the sample's three vendors). Treat anything in a BLANK that is not a generic heading as sample content and replace it.
- Content for veni comes from the two requirements files, from this file, and from the user. Where a fact is unknown (names, dates, budget figures), ask or mark it clearly as TBD rather than inventing it.
- `Lead the team/` holds one individual assessment per sample team member (Bill, Bob, Christine). For veni, produce one per actual team member and name the files after them.
- The PDFs are static text, not fillable forms (no AcroForm fields), so a veni document is authored fresh from the template's structure, not by filling fields.

### Known irregularities (leave as-is unless asked)

- Several folder names are misspelled: `Identifiy Stakeholers`, `Estimate Ressoruce`, `Manage Project Knoowledge`, `Monitor and Contorl Project Performance`, `Plan Sourcing stratey`, `Plan Stakehler Engagement Plan`, `Identify Risk and Plan risk Respone`. Use the paths verbatim and do not rename them without the user's say-so.
- `Plan Communication Management/` has only a filled sample (`Communication_management_plan_filled.pdf`), no BLANK; derive the veni version from the sample's headings.
- `Plan Resources Management/Resource_Management_Plan_FILLED (2).pdf` is the filled resource management plan despite the odd name.

### Where veni's filled documents live (decided 2026-10-20)

veni's documents are Markdown, one file per template, in `docs/veni/`, a tree parallel to `docs/pmi/`:

- Path: `docs/veni/<process folder>/<Document>.md`, where `<process folder>` is the exact folder name from `docs/pmi/` (misspellings included, so the two trees stay in lockstep) and `<Document>` is the template filename without `_BLANK.pdf`. Example: `docs/pmi/Initiate Project or Phase/Project_Charter_BLANK.pdf` becomes `docs/veni/Initiate Project or Phase/Project_Charter.md`.
- Individual assessments are `docs/veni/Lead the team/Individual_Assessment_<Name>.md`, one per real team member. The communications plan, which has no BLANK, is `docs/veni/Plan Communication Management/Communication_Management_Plan.md`, derived from the filled sample's headings.
- Each file opens with the template's header fields (Project Title, Date Prepared, and any others the template has, such as Project Manager or Sponsor) as a short Markdown table, then follows the template's section headings in the template's order. Template tables become Markdown tables. Dates are ISO format (`YYYY-MM-DD`).
- Living documents (risk register, issue log, change log, assumption log, lessons learned register) are updated in place: append rows, never renumber existing IDs, and refresh the Date Prepared field on each update.
- The compiled plan and document set that mirror the two Mary's Consulting PDFs are `docs/veni/Project_Management_Plan.md` and `docs/veni/Project_Documents.md`. They are assembled from the per-template files once those exist, following the sample's section order, and the project management plan is where the six PMBOK 8 principles and the phase reviews are written in.
- The PDFs in `docs/pmi/` stay read-only reference material. PDF export of a veni document happens only when the PM asks for it.

## Git

- Default branch is `main`. The `origin` remote points at GitHub (`mabubakarriaz/veni`) through the SSH host alias `github-personal`, so pushes depend on that alias existing in `~/.ssh/config`. If `gh` cannot resolve the remote, pass `--repo mabubakarriaz/veni`.
- `docs/pmi/` is about 11 MB of binary PDFs committed directly; don't regenerate or re-save them, since every edit re-adds the whole file to history.
