# dairy-flow 🐄

Dairy-Flow is a dairy herd simulation, feeding, inventory, and economic planning
platform designed for multi-farm use with role-based access control (RBAC).

---

## 🔐 Roles & Permissions (RBAC) – Flowchart

```mermaid
flowchart TD

A[User opens app] --> B{Authenticated}
B -->|No| C[Login or Signup]
B -->|Yes| D[Load user profile]

D --> E{Has farm}
E -->|No| F[Create or request farm]
E -->|Yes| G[Farm picker<br/>User farms only]

G --> H[Select farm]
H --> I[Resolve role]
I --> J{Role}

J -->|Dashboard| R1[Dashboard view]
J -->|Viewer| R2[Dashboard limited]
J -->|Feed editor| R3[Edit feeding inventory]
J -->|Farm admin| R4[All farm modules]
J -->|Master admin| R5[All farms access]

%% Route guards
R1 --> K[Route guard]
R2 --> K
R3 --> K
R4 --> K
R5 --> K

K --> L{Action allowed}
L -->|Yes| M[Execute action]
L -->|No| N[Show 403 error]

%% Farm admin scope
R4 --> U1[Farm users]
U1 --> U2[Add user]
U1 --> U3[Remove user]
U1 --> U4[Assign role]

%% Master admin scope
R5 --> V1[Master console]
V1 --> V2[Create delete farms]
V1 --> V3[Create delete users]
V1 --> V4[Assign roles]
