# Dairy-Flow 🐄 – Application Flowcharts

This document describes the main functional flow of the Dairy-Flow application.

---

## Diagram 1 — Entry, Access Control, Roles, and Data Onboarding

➡️ **Next:** Next: [Diagram 2 — Parameters, Herd Simulation, and Farm Grouping](diagram-2.md)

```mermaid
flowchart TD

%% ENTRY AND ACCESS CONTROL
A0[Landing or Entry Page] --> A1[User Action]
A1 --> A2[Login]
A1 --> A3[Create Account]

A3 --> A4[Create Farm Workspace]
A2 --> A5[Select Farm]
A4 --> A5

A5 --> R0[Role Check RBAC]

R0 --> D0[Dashboard]
R0 --> FI0[Feeding and Inventory]
R0 --> P0[Parameters]
R0 --> MA0[Master Admin Console]

MA0 --> MA1[View All Farms]
MA1 --> MA2[Create or Delete Farms]
MA1 --> MA3[Create or Delete Users]
MA1 --> MA4[Assign Roles]
MA0 --> D0

%% DATA ONBOARDING
P0 --> B0[Data Onboarding]
D0 --> B0
FI0 --> B0

B0 --> B1[Select Data Source]
B1 --> B2[Manual Heifers Entry]
B1 --> B3[Manual Cows Entry]
B1 --> B4[Excel Import]

B4 --> B5[Upload Excel File]
B5 --> B6[Validate and Map Data]
B2 --> B6
B3 --> B6

B6 --> B7[Data Valid]
B7 --> B8[Fix Errors]
B7 --> B9[Commit to Database]
```
