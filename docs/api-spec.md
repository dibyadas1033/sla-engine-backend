# SR-SLA System — API Specification (Tables)

This document contains a complete, **API specification** for the Service Request & SLA Enforcement System (SR-SLA), organized in tables.


## 1️⃣ Request Lifecycle APIs

| API Endpoint              | Method | Role       | Purpose              | Reason                                                       |
| ------------------------- | ------ | ---------- | -------------------- | ------------------------------------------------------------ |
| /api/requests             | POST   | Requester  | Create a new request | Starts SLA tracking, ensures audit entry                     |
| /api/requests/{requestId} | GET    | All (RBAC) | Get request details  | Unified view for all stakeholders, supports timeline & audit |
| /api/requests             | GET    | All (RBAC) | List requests        | Populate dashboards without duplicate endpoints              |


## 2️⃣ State Transition APIs

| API Endpoint                          | Method | Role            | Purpose                 | Reason                                               |
| ------------------------------------- | ------ | --------------- | ----------------------- | ---------------------------------------------------- |
| /api/requests/{requestId}/acknowledge | POST   | Department Head | Start SLA-1             | Prevents silent ownership; accountability            |
| /api/requests/{requestId}/assign      | POST   | Department Head | Assign officer (SLA-2)  | Officer responsibility explicit; governance          |
| /api/requests/{requestId}/progress    | POST   | Officer         | Update task progress    | Maintains audit trail; prevents premature completion |
| /api/requests/{requestId}/complete    | POST   | Officer         | Mark execution complete | Starts closure SLA; officer cannot self-close        |
| /api/requests/{requestId}/close       | POST   | Department Head | Close request           | Ensures SLA compliance; final audit entry            |


## 3️⃣ Escalation & SLA Control APIs

| API Endpoint                        | Method | Role                     | Purpose               | Reason                                                  |
| ----------------------------------- | ------ | ------------------------ | --------------------- | ------------------------------------------------------- |
| /api/requests/{requestId}/escalate  | POST   | Department Head / System | Trigger escalation    | Enforces rules; reason mandatory; audit logged          |
| /api/requests/{requestId}/pause-sla | POST   | Department Head          | Pause SLA temporarily | Handles legal/dependency delays; justification required |


## 4️⃣ SLA Visibility APIs

| API Endpoint      | Method | Role                    | Purpose                 | Reason                               |
| ----------------- | ------ | ----------------------- | ----------------------- | ------------------------------------ |
| /api/sla/overview | GET    | Department Head / Admin | High-level SLA overview | Powers KPI dashboards; read-only     |
| /api/sla/at-risk  | GET    | Department Head         | Preemptive intervention | SLA 75% consumed warning             |
| /api/sla/breaches | GET    | Admin / Audit           | Track breached requests | Audit readiness; SLA breach tracking |


## 5️⃣ Audit & Compliance APIs

| API Endpoint                    | Method | Role                 | Purpose                   | Reason                                       |
| ------------------------------- | ------ | -------------------- | ------------------------- | -------------------------------------------- |
| /api/requests/{requestId}/audit | GET    | Authorized read-only | Immutable request history | Legal defensibility; prevents blame-shifting |


## 6️⃣ Configuration APIs (Admin Only)

| API Endpoint             | Method     | Purpose                    | Reason                                        |
| ------------------------ | ---------- | -------------------------- | --------------------------------------------- |
| /api/config/sla-policies | POST / GET | Define SLA rules centrally | Decouple policy from code; supports updates   |
| /api/config/departments  | POST       | Configure departments      | Prevents hardcoding; multi-department support |
| /api/config/roles        | POST       | Configure roles            | Prevents hardcoding; multi-department support |


## 7️⃣ System & Operations APIs

| API Endpoint     | Method | Purpose                                     |
| ---------------- | ------ | ------------------------------------------- |
| /health          | GET    | Infrastructure & container health check     |
| /api/system/time | GET    | SLA accuracy; time consistency; testability |


## 🔒 Excluded / Forbidden APIs

| API Endpoint   | Reason                          |
| -------------- | ------------------------------- |
| /updateStatus  | Avoid bypassing state machine   |
| /forceClose    | Enforces audit compliance       |
| /editRequest   | Prevents SLA & audit corruption |
| /deleteRequest | Maintains historical integrity  |


## 🧠 Architectural Defense

> Each API maps to a **legally allowed business action**. State transitions are explicit, role-restricted, and audited. SLA timing is per state, ensuring accountability at every stage.

