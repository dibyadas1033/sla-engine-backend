# SR-SLA System — High-Level Architecture

## Overview

The Service Request & SLA Enforcement System (SR-SLA) is designed to enforce accountability, monitor SLAs, and provide audit-ready transparency for government and enterprise service requests.

This document describes the high-level architecture, key components, and how SLA enforcement works using background processing and caching.



## Architectural Layers

```
Client / UI
    └── Requester Dashboards
    └── Officer / Department Head Dashboards
        │ HTTP / API
        ▼
API Layer (.NET Core)
    └── Controllers
    └── RBAC / Authorization
    └── Input Validation
        │
        ▼
Application Layer
    └── SLA Orchestration
    └── Request State Machine
    └── Escalation Logic
    └── DTO Mapping
        │
        ▼
Domain Layer
    └── Request / SLA Entities
    └── SLA Policy Rules
    └── Escalation Conditions
        │
        ▼
Infrastructure Layer
    └── SQL Server / EF Core
    └── Repositories
    └── Redis / In-memory cache
        │
        ▼
Background Processing
    └── Worker / Hangfire / Quartz
    └── Cron / Timer Jobs
    └── SLA Timer Monitoring
    └── Escalation & Notifications
```



## Component Details

### 1. Client / UI

* Role-based dashboards for Requester, Officer, Department Head.
* Provides transparency and real-time SLA visibility.
* Only exposes allowed actions per user role.

### 2. API Layer

* Handles HTTP requests and responses.
* Enforces authentication & authorization.
* Validates input data.
* Passes control to Application Layer.

### 3. Application Layer

* Orchestrates SLA timers and state transitions.
* Executes business use-cases: Acknowledge, Assign, Progress Update, Complete, Close.
* Triggers escalation or notifications via background workers.

### 4. Domain Layer

* Contains core business rules.
* Models Request entity, SLA policies, state machine, and escalation rules.
* All SLA calculations and state validations reside here.

### 5. Infrastructure Layer

* Database (SQL Server) stores persistent data: requests, audit logs, SLA history.
* Repositories abstract DB access.
* Redis stores SLA timers for fast access and distributed lock support.

### 6. Background Processing

* Worker Service (Hangfire, Quartz, or .NET Worker) monitors SLA timers.
* Sends escalation notifications and logs breaches.
* Runs recurring jobs for SLA checks without impacting API performance.



## SLA Timer Flow

```
Request Created
       │
       ▼
SLA Timer started → stored in Redis
       │
       ▼
Background Worker monitors timers
       │
       ├── SLA 75% consumed → Warning Notification
       └── SLA Breached → Escalation + Audit Log
       │
       ▼
Updates SQL DB + triggers dashboard updates
```

### Notes:

* SLA timers are measured per state transition.
* Pausing or overriding SLA is restricted and audited.
* Redis ensures fast, in-memory tracking of SLA deadlines.
* Worker jobs ensure system enforces SLAs even if users are inactive.


This architecture ensures **real-time SLA enforcement, auditability, and operational transparency**, while remaining **scalable**.

