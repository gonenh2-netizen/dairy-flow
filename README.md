# Dairy-Flow 🐄

Dairy-Flow is a dairy herd simulation, feeding, inventory, and economic planning platform
for multi-farm operations with role-based access control (RBAC).

---

## 🧭 Full Application Flowchart

```mermaid
flowchart TD

%% =========================
%% 0 ENTRY AND ACCESS CONTROL
%% =========================
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

%% =========================
%% 1 DATA ONBOARDING
%% =========================
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

%% =========================
%% 2 PARAMETER SETUP WITH BOUNDARIES
%% =========================
B9 --> P1[Set simulation start year]
P1 --> P2[Reproduction and protocols<br/>sex ratio CI target VWP gestation IPP doses]
P2 --> P3[Herd size and retention<br/>barn capacity heifer mortality heifer sale strategy]
P3 --> P4[Feedlot and bulls optional<br/>male retention sale age weight price per kg]
P4 --> P5[Culling strategy<br/>by lactation and early cull fresh under 60 DIM]
P5 --> P6[Production targets<br/>current avg milk 305d targets waste percent dry period]
P6 --> P7[Economics and costs<br/>milk price semen costs fixed monthly costs cull price calf price]
P7 --> P8[Validation boundaries layer<br/>min max formats warnings blockers]

%% =========================
%% 3 RUN HERD MOVEMENT SIMULATION
%% =========================
P8 --> S0[Run herd movement simulation engine]
S0 --> S1[Monthly loop M1 to M60]
S1 --> S2[Apply herd rules<br/>calvings entries exits mortality culls sales]
S2 --> S3[Update herd counts<br/>milking dry closeup calves heifers groups]
S3 --> S4[Calculate DIM distribution as needed]
S4 --> S5[Generate outputs tables]
S5 --> S6[Store results by month and year<br/>cache for dashboard]

%% =========================
%% 4 FARM GROUPING CONFIGURABLE STRUCTURE
%% =========================
S6 --> G0[Farm group builder]
G0 --> G1[Define groups by rules<br/>age ranges status lactation]
G1 --> G2[Validate groups<br/>no gaps no overlaps coverage]
G2 --> G3[Map simulated animals to groups monthly]
G3 --> G4[Save group structure per farm]

%% =========================
%% 5 FEEDING AND INVENTORY CONFIG
%% =========================
G4 --> FI0

FI0 --> RM0[Raw materials setup]
RM0 --> RM1[Add material form<br/>name type seasonal or monthly start month initial stock]
RM1 --> RM2[Set DM percent loss percent<br/>and optional price]
RM2 --> RM3{Import raw materials from Excel}
RM3 -->|Yes| RM4[Download template<br/>upload validate]
RM3 -->|No| RF0[Ration formulations]

FI0 --> RF0
RF0 --> RF1[Create or edit ration]
RF1 --> RF2[Add ingredients<br/>kg per head per day as fed]
RF2 --> RF3[Effective date year and month]
RF3 --> RF4[Link ration to animal groups]
RF4 --> RF5[Set appetite scaling percent]
RF5 --> RF6{Import rations from Excel}
RF6 -->|Yes| RF7[Download template<br/>upload validate]
RF6 -->|No| C0[Run feed and inventory calculator]

%% =========================
%% 6 FEED USAGE PROCUREMENT INVENTORY SIM
%% =========================
C0 --> C1[Each month animals by group<br/>times ration times appetite]
C1 --> C2[Convert to DM using DM percent]
C2 --> C3[Apply losses]
C3 --> C4[Output monthly material usage matrix]
C4 --> I0[Inventory engine]
I0 --> I1[Closing equals opening<br/>plus purchases minus usage]
I1 --> I2[Procurement planner inputs<br/>manual or rules]
I2 --> I3[Inventory pages]
I3 --> I4[Stock flow matrix<br/>materials by months]
I3 --> I5[Material detail graph<br/>closing usage purchases]
I3 --> I6[Purchase planner view]

%% =========================
%% 7 ECONOMICS IOFC DASHBOARD
%% =========================
C4 --> E0[Economics engine]
E0 --> E1[Feed cost equals usage times price]
E1 --> E2[Milk income equals volume times milk price]
E2 --> E3[Livestock sales income<br/>heifers bulls culls]
E3 --> E4[IOFC equals milk income minus feed cost]
E4 --> E5[Efficiency metrics<br/>kg DM per L milk trends]
E5 --> D0

D0 --> D1[Dashboard KPIs year and five year]
D1 --> D2[Herd dynamics charts<br/>cows heifers culls sales]
D1 --> D3[Milk production forecast charts]
D1 --> D4[Financial charts<br/>feed cost milk income livestock sales net IOFC]
D1 --> D5[Drilldowns filters<br/>by year month group]

%% =========================
%% 8 TIMELINE PAGE EXPORTS
%% =========================
D0 --> T0[Month by month simulation timeline page]
T0 --> T1[Counts cows dry closeup nursery calves]
T1 --> T2[Male vs female calves]
T2 --> T3[Heifers by farm defined groups]
T3 --> X0[Export center]
X0 --> X1[Download Excel simulation tables]
X0 --> X2[Download Excel inventory and usage matrix]
X0 --> X3[Download Excel templates<br/>and date format option]
