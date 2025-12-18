# dairy-flow
flowchart TD

A[User opens app] --> B{Authenticated?}
B -->|No| C[Login / Signup]
B -->|Yes| D[Load user profile + memberships]

D --> E{Has farm memberships?}
E -->|No| F[Create / Request access to a farm]
E -->|Yes| G[Farm Picker: show only farms user belongs to]

G --> H[Select farm] --> I[Resolve role for selected farm]
I --> J{Role?}

J -->|Dashboard Only| R1[Allow: Dashboard read-only]
J -->|Viewer Limited| R2[Allow: Dashboard + limited pages read-only]
J -->|Feeding/Inventory Data Entry| R3[Allow: Inventory + Rations edit; Block: Herd Params]
J -->|Farm Admin| R4[Allow: All farm modules + manage users in this farm]
J -->|Master Admin| R5[Allow: All farms + delete farms/users + global settings]

%% Guards at route/module level
R1 --> K[Route Guards]
R2 --> K
R3 --> K
R4 --> K
R5 --> K

K --> L{User tries to open page/action}
L -->|Allowed| M[Render page / Execute action]
L -->|Blocked| N[Show 403 + explain permission needed]

%% Farm user management scope
R4 --> U1[Farm User Admin]
U1 --> U2[Add user to farm]
U1 --> U3[Remove user from farm]
U1 --> U4[Assign farm roles]
U1 --> U5[Cannot touch other farms]

R5 --> V1[Master Admin Console]
V1 --> V2[Create/Delete farms]
V1 --> V3[Create/Delete users]
V1 --> V4[Assign roles across farms]
V1 --> V5[Impersonate/Support mode (optional)]
