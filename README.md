# Dairy-Flow 🐄 – Application Flowcharts

This document describes the **full functional flow** of the Dairy-Flow application,
from entry and access control through simulation, feeding, inventory, economics,
dashboards, and exports.

---

## Diagram 1 — Entry, Access Control, Roles, and Data Onboarding

➡️ **Next:** [Diagram 2 — Parameters, Herd Simulation, and Farm Grouping](#diagram-2--parameters-herd-simulation-and-farm-grouping)

```mermaid
flowchart TD

%% ENTRY AND ACCESS CONTROL
A0[Landing or Entry Page] --> A1{User action}
A1 -->|Login| A2[Auth sign in]
A1 -->|Create account| A3[Sign up name email location animals new or existing]
A3 --> A4[Create farm workspace and default roles]
A2 --> A5[Select farm user belongs to]
A4 --> A5

A5 --> R0{Role check RBAC}
R0 -->|Dashboard only| D0[Main dashboard]
R0 -->|Viewer limited| D0
R0 -->|Feeding and inventory entry| FI0[Feeding and inventory hub]
R0 -->|Farm admin| P0[Parameters hub]
R0 -->|Master admin| MA0[Master admin console]

MA0 --> MA1[View all farms]
MA1 --> MA2[Create delete farms]
MA1 --> MA3[Create delete users]
MA1 --> MA4[Assign roles across farms]
MA0 --> D0

%% DATA ONBOARDING
P0 --> B0[Data onboarding]
D0 --> B0
FI0 --> B0

B0 --> B1{Data source}
B1 -->|Heifers manual| B2[Heifers data entry UI]
B1 -->|Cows manual| B3[Cows data entry UI]
B1 -->|Import Excel| B4[Download Excel template<br/>and choose date format]
B4 --> B5[Upload Excel file]
B5 --> B6[Import validation<br/>and mapping to DB]
B2 --> B6
B3 --> B6

B6 --> B7{Valid}
B7 -->|No| B8[Show errors and hints<br/>fix and reupload]
B7 -->|Yes| B9[Commit to DB<br/>and create baseline snapshot]
