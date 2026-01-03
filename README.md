# SLA Engine Backend

Enterprise-grade backend service for **Service Level Agreement (SLA) evaluation, tracking, and breach analysis**.

This project is designed with an **industry-first mindset**, focusing on clean structure, backend reliability, and extensibility for enterprise and industrial systems.

---

## Overview

The SLA Engine Backend provides APIs to:
- Define and manage SLAs
- Evaluate SLA compliance against operational events
- Detect SLA breaches
- Support reporting and downstream escalation workflows

The system is intentionally backend-focused and avoids UI or automation hype.  
AI/ML, where explored, is treated as **decision support**, not autonomous control.

---

## Tech Stack

- **.NET 8 / ASP.NET Core Web API**
- **C#**
- **xUnit** (unit testing)
- **Docker** (local orchestration)
- **GitHub Actions** (CI)
- SQL Server (planned / via docker-compose)

---

## Repository Structure
````
sla-engine-backend/
│
├── src/ # Application source code
│ └── Sla-engine-backend/ # ASP.NET Core Web API
│
├── tests/ # Automated tests
│ └── SLA.Engine.UnitTests/
│
├── docs/ # Architecture & design documentation
│ ├── architecture.md
│ ├── sla-logic.md
│ └── api-spec.md
│
├── build/ # Build & deployment assets
│ ├── docker/
│ └── scripts/
│
├── .github/workflows/ # CI pipelines
│ └── ci.yml
│
├── docker-compose.yml # Local environment setup
├── sla-engine-backend.sln
└── README.md
````

---

## Current State

- Single Web API project under `src/`
- Unit test project using xUnit
- CI pipeline scaffolded via GitHub Actions
- Documentation placeholders for architecture, SLA logic, and API contracts

This structure is **intentional** and allows future evolution into:
- Clean Architecture layering
- Multiple microservices
- Shared SLA evaluation libraries

---

## Running the Application (Local)

### Prerequisites
- .NET SDK 8+
- Docker (optional, for dependencies)

### Run API
```bash
dotnet restore
dotnet run --project src/Sla-engine-backend


