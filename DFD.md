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
database "D2: Teams DB" as DB2
database "D3: Matches DB" as DB3
database "D4: Bots DB" as DB4

User --> P1 : User Credentials\n(e.g., Emails, Passwords, Usernames)
P1 <--> DB1 : Read/Write User Data

User --> P2 : Team Data\n(Team names, PACSS, Players, Team-specific stats)
P2 <--> DB2 : Read/Write Teams & Players Data

User --> P3 : League Data\n(League names, Leaderboards, Fixtures, Matches)
P3 <--> DB3 : Read/Write Leagues & Matches Data

User --> P4 : Bot Data\n(Behaviors, Python Scripts, Python Syntax Verifier) 
P4 <--> DB4 : Read/Write Bot & Behavior Data

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


