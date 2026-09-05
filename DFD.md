```plantuml
@startuml
title "Football game" - DFD Level 0

rectangle "Football game" as R1
rectangle "User (Client)" as U1
rectangle "Database (Data Store)" as D1

' Arrows with annotations added using the colon syntax
U1 --> R1 : User Inputs / Commands
R1 --> D1 : Read / Write Game Data
D1 --> R1 : Stored Data Response
U1 --> R1 : User Credentials / Profile Data
R1 --> U1 : Game State / UI Updates
@enduml
```


```plantuml
title DFD Level 1 - System Overview \n Core Functions

' External Entity
rectangle "User" as User

' Processes (Functions)

usecase "1.0\nUser/Profile Managment" as P1
usecase "2.0\nTeam Management" as P2
usecase "3.0\nLeague and Matches Engine" as P3
usecase "4.0\nBots and Behaviors Engine" as P4

' Data Store 
database "D1: User DB" as DB1
database "D2: Teams &\n Players DB" as DB2
database "D3: Matches DB" as DB3
database "D4: Bots DB" as DB4

User --> P1 : User Credentials\n(e.g., Emails, \nPasswords, Usernames)
P1 <--> DB1 : Read/Write User Data

User --> P2 : Team Data\n(Team names, PACSS, \nPlayers, Team-specific stats)
P2 <--> DB2 : Read/Write Teams \n& Players Data

User --> P3 : League Data\n(League names, Leaderboards, \nFixtures, Matches)
P3 <--> DB3 : Read/Write Leagues \n& Matches Data

User --> P4 : Bot Data\n(Behaviors, Python Scripts, \nPython Syntax Verifier) 
P4 <--> DB4 : Read/Write Bot \n& Behavior Data

P1 --> DB1
P2 --> DB2
P3 --> DB3
P4 --> DB4
```

```plantuml
title "DFD Level 2 - User/Profile Management"

rectangle "User" as User
usecase "1.1\nUser Registration" as UC1
usecase "1.2\nUser Login" as UC2
usecase "1.3\nProfile Edition" as UC3

database "D1: Users DB" as DB

User --> UC1 : Unregistered User Data\n(Username, Email, Password...)
User --> UC2 : User Login Credentials\n(Email, Password)
User --> UC3 : New User Credentials\n (Optional Email, Optional Password,...)
UC1 --> DB
UC2 --> DB
UC3 --> DB
```

```plantuml
@startuml
title DFD Level 2 - Process 1.1 User Registration

rectangle "User" as EntityUser

' Level 2 Sub-Processes
usecase "1.1.1\nValidate Inputs" as P1_1
usecase "1.1.2\nCheck Email\nUniqueness" as P1_2
usecase "1.1.3\nHash Password &\nSave User" as P1_3

database "D1: Users DB" as DB

' Execution Pipeline
EntityUser --> P1_1 : Raw Registration Form Data
P1_1 --> EntityUser : Formatting Errors (Invalid Email/Password format)

P1_1 --> P1_2 : Formatted Inputs
P1_2 --> DB : Query Email Record
DB --> P1_2 : Existing Account Data
P1_2 --> EntityUser : Error: Email Already Taken

P1_2 --> P1_3 : Unique & Validated Data
P1_3 --> DB : Write Encrypted User Record
P1_3 --> EntityUser : Registration Success Response
@enduml
```

```plantuml
@startuml
title DFD Level 2 - Process 1.2 User Login

' External Entity
rectangle "User" as EntityUser

' Sub-Processes (Level 2)
usecase "1.2.1\nValidate Field\nFormats" as P1
usecase "1.2.2\nAuthenticate\nCredentials" as P2
usecase "1.2.3\nGenerate User\nSession" as P3

' Data Store
database "D1: Users DB" as DB

' Data Flows
EntityUser --> P1 : Login Credentials\n(Email, Raw Password)
P1 --> EntityUser : Field Validation Errors\n(e.g., Empty fields, bad email format)

P1 --> P2 : Formatted Credentials

' Database Interaction Loop
P2 --> DB : Query User Record by Email
DB --> P2 : Encrypted Hash & User Data
P2 --> EntityUser : Error: Invalid Email or Password

' Success Pathway
P2 --> P3 : Validated User Identity
P3 --> DB : Update Last Login Timestamp
P3 --> EntityUser : Auth Response

@enduml
```

```plantuml
@startuml
title DFD Level 2 - Process 1.3 Edit User Profile

rectangle "User" as Us
database "D1: Users DB" as DB1

usecase "1.3.1\nValidate Field\nFormats & Matching" as US1
usecase "1.3.2\nVerify Current\nPassword" as US2
usecase "1.3.3\nHash Password\n& Update User" as US3

' Step 1: Format Checks
Us --> US1 : Profile Update Inputs\n(Name, Email, Avatar, Passwords)
US1 --> Us : Error: Invalid Email Format\nor New/Repeat Password Mismatch
US1 --> US2 : Formatted Profile Data\n& Credentials Check Request

' Step 2: Authentication Check
US2 --> DB1 : Query Existing User Hash
DB1 --> US2 : Current Password Hash
US2 --> Us : Error: Incorrect Current Password
US2 --> US3 : Validated Profile Updates\n& New Password Data

' Step 3: Hash and Save
US3 --> DB1 : Update User Record
US3 --> Us : Profile Updated Success Response

@enduml
```
---
# Player Management
```plantuml
@startuml
title DFD Level 2 - Process 2.1 Create Player

rectangle "User" as Us
database "D2: Teams & Players DB" as DB2

usecase "2.1.1\nValidate Name &\nPACSS Rules" as P1
usecase "2.1.2\nSave Player &\nAssign Team" as P2

' Step 1: Input & PACSS Validation
Us --> P1 : Player Data\n(Name, PACSS Attributes)
P1 --> Us : Error: Invalid PACSS Allocation\n(Total Must Equal 10 Points)
P1 --> Us : Error: Attribute Below Minimum\n(Min 1 Point Per Attribute)
P1 --> P2 : Validated Player Profile

' Step 2: Database Persistence & Output
P2 --> DB2 : Write New Player Record
P2 --> Us : Player Creation Success Response

@enduml
```

```plantuml
@startuml
title DFD Level 2 - Process 2.2 Update Player

rectangle "User" as Us
database "D2: Teams & Players DB" as DB2

usecase "2.2.1\nValidate Match\nEligibility" as P1
usecase "2.2.2\nValidate PACSS\nRe-allocation" as P2
usecase "2.2.3\nUpdate Player\nRecord" as P3

' Step 1: Match Lock Check
Us --> P1 : Update Request\n(Player ID, Updated Name, PACSS)
P1 --> DB2 : Query Active\n Match Status
DB2 --> P1 : Player Match\n Status
P1 --> Us : Error: Cannot Edit Player\n(Currently In Active Match)
P1 --> P2 : Approved Update\n Payload

' Step 2: PACSS Validation
P2 --> Us : Error: Invalid PACSS Allocation\n(Total Must Equal\n 10 Points)
P2 --> P3 : Validated Updated\n Attributes

' Step 3: Database Persistence & Output
P3 --> DB2 : Update Player Entry \n& Team Mapping
P3 --> Us : Player Update \nSuccess Response

@enduml
```

```plantuml
@startuml
title DFD Level 2 - Process 2.3 Delete Player

rectangle "User" as Us
database "D2: Teams & Players DB" as DB2

usecase "2.3.1\nValidate Deletion\nEligibility" as US1
usecase "2.3.2\nExecute Player\n& Roster Removal" as US2

' Step 1: Eligibility Check
Us --> US1 : Delete Player Request \n(Player ID)
US1 --> DB2 : Query Active League\n & Team Status
DB2 --> US1 : Player Assignment\n Records
US1 --> Us : Error: Cannot Delete \n(Active League)
US1 --> Us : Warning: Player in Team \n(Requires Proceed Confirmation)

' Step 2: Execution & Cascade Update
Us --> US2 : Confirmed Deletion\n Request
US2 --> DB2 : Delete Player\n Record
US2 --> DB2 : Cascade Update\n Team Roster
US2 --> Us : Player Deleted\n Success Response

@enduml
```
---
# Team Management

```plantuml
@startuml
title DFD Level 2 - Process 3.1 Create Team

rectangle "User" as Us
database "D2: Teams & Players DB" as DB2

usecase "3.1.1\nValidate Roster\nCount & Inputs" as P1
usecase "3.1.2\nVerify Player\nAvailability" as P2
usecase "3.1.3\nSave Team &\nAssign Players" as P3

' Step 1: Input & Size Validation
Us --> P1 : Team Creation Data\n(Team Name, 6+ Players)
P1 --> Us : Error: Insufficient Players\n(Minimum 6 Required)
P1 --> P2 : Formatted Team Data\n& Valid Roster Count

' Step 2: Database Assignment Check
P2 --> DB2 : Query Player\n Assignment Status
DB2 --> P2 : Current Player/Team Mappings
P2 --> Us : Error: Player(s) Already Assigned\nTo Another Team

P2 --> P3 : Fully Validated\n Team Payload

' Step 3: Database Persistence
P3 --> DB2 : Write New\n Team Record
P3 --> DB2 : Map Players to\n Team
P3 --> Us : Team Creation\n Success Response

@enduml
```

```plantuml
@startuml
title DFD Level 2 - Process 3.2 Manage Team Players

rectangle "User" as Us
database "D2: Teams &\n Players DB" as DB2

usecase "3.2.1\nValidate Roster\nAction" as P1
usecase "3.2.2\nUpdate Roster\nRelationship" as P2

' Step 1: Input & Roster Validation
Us --> P1 : Roster Action\n(Team, Player,\n Add/Remove)
P1 --> DB2 : Query Current\n Team Roster
DB2 --> P1 : Existing Roster\n List

P1 --> Us : Error: Player Already\n in Team (Add Action)
P1 --> Us : Error: Player Not\n in Team (Remove Action)

P1 --> P2 : Validated Roster\n Update Payload

' Step 2: Database Persistence
P2 --> DB2 : Add/Remove Player-Team\n Mapping
P2 --> Us : Roster Update\n Success Response

@enduml
```

```plantuml
@startuml
title DFD Level 2 - Process 3.3 Modify Team

rectangle "User" as Us
database "D2: Teams & Players DB" as DB2

usecase "3.3.1\nValidate Name\nUniqueness" as P1
usecase "3.3.2\nUpdate Team\nMetadata" as P3

' Step 1: Name Uniqueness Check
Us --> P1 : Team Modification Data\n(Team, New Name)
P1 --> DB2 : Query Existing Team Names
DB2 --> P1 : Name Availability Response
P1 --> Us : Error: Duplicate\n Team Name

P1 --> P3 : Fully Validated Team

' Step 3: Database Persistence
P3 --> DB2 : Update Team Name
P3 --> Us : Team Modification\n Success Response

@enduml
```
--- 
# Leagues Management

```plantuml
@startuml
title DFD Level 2 - Process 4.1 Create League

rectangle "User" as Us
database "D3: Leagues & Matches DB" as DB3

usecase "4.1.1\nValidate Formats\n& Team Count" as P1
usecase "4.1.2\nSave League &\nAssign Teams" as P3

' Step 1: Input & Size Validation
Us --> P1 : League Data\n(Name, Type, \nPassword, Configs, Teams)
P1 --> Us : Error: Invalid League Name\n(Name field is empty)
P1 --> Us : Error: Insufficient Teams\n(Minimum 3 Required)
P1 --> P3 : Formatted League Data\n& Valid Team Count


' Step 3: Database Persistence
P3 --> DB3 : Write New League Record\n(Configs, Rules, Type)
P3 --> DB3 : Assign Selected\n Teams to League
P3 --> Us : League Creation \nSuccess Response

@enduml
```
