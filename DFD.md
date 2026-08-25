┌─────────────────────────────────────────────────────────────────────────────┐
│                    FOOTBALL LEAGUE SYSTEM - DFD LEVEL 0                     │
│                          (Context Diagram)                                   │
└─────────────────────────────────────────────────────────────────────────────┘

                    ┌───────────────────────────────────────┐
                    │     FOOTBALL LEAGUE SYSTEM            │
                    │  (User/League/Match Management)       │
                    └───────────────────────────────────────┘
                              ▲              ▼
                              │              │
                    ┌─────────┴──┐       ┌─────┴─────────┐
                    │             │       │                │
                ┌───▼─────┐   ┌───▼─────┐
                │  USER    │   │DATABASE │
                │ (Client) │   │ (Data   │
                │          │   │  Store) │
                └──────────┘   └────────┘


┌─────────────────────────────────────────────────────────────────────────────┐
│                    FOOTBALL LEAGUE SYSTEM - DFD LEVEL 1                     │
│                      (Main Processes & Data Flows)                          │
└─────────────────────────────────────────────────────────────────────────────┘

    ┌──────────────┐                                   ┌──────────────┐
    │    USER      │                                   │   DATABASE   │
    │             │                                   │              │
    │ • Username  │                                   │ • User Store │
    │ • Email     │                                   │ • League DB  │
    │ • Password  │                                   │ • Match DB   │
    │ • Name      │                                   │ • Team DB    │
    │ • Avatar    │                                   │ • Player DB  │
    └──────┬───────┘                                   └──────┬───────┘
           │                                                   │
           │ 1. Credentials                                    │
           ├──────────────────────────────────────────────────►│
           │ (Username, Email, Password, Name, Avatar)         │
           │                                                   │
           │ 2. Validation Response                            │
           │◄──────────────────────────────────────────────────┤
           │ (Success/Failure, Error Messages)                 │
           │                                                   │
           │ 3. League Data                                    │
           ├──────────────────────────────────────────────────►│
           │ (Name, Duration, Min/Max Teams, Password)         │
           │                                                   │
           │ 4. Team Data                                      │
           ├──────────────────────────────────────────────────►│
           │ (Team Name, Owner, Players)                       │
           │                                                   │
           │ 5. Match Results                                  │
           ├──────────────────────────────────────────────────►│
           │ (Teams, Goals, Timestamp)                         │
           │                                                   │
           │ 6. Leaderboard Data                               │
           │◄──────────────────────────────────────────────────┤
           │ (Wins, Losses, Total Goals per Team)              │
           │                                                   │
           └───────────────────────────────────────────────────┘


┌─────────────────────────────────────────────────────────────────────────────┐
│              FOOTBALL LEAGUE SYSTEM - DFD LEVEL 2 (Detailed)                │
│                    (Process Breakdown & Data Stores)                        │
└─────────────────────────────────────────────────────────────────────────────┘

┌─────────┐
│  USER   │
└────┬────┘
     │
     │ Sign Up Request
     ▼
   ┌─────────────────────┐
   │  1.0 USER MGMT      │
   │ • Validate Input    │
   │ • Check Email       │
   │ • Create Account    │
   └─────────────────────┘
     │
     ├──► ┌──────────────┐
     │    │ D1: Users DB │
     │    │ (Store user  │
     │    │  credentials)│
     │    └──────────────┘
     │
     │ Login Request
     ▼
   ┌─────────────────────┐
   │  2.0 AUTH SYSTEM    │
   │ • Verify Email      │
   │ • Check Password    │
   │ • Generate Token    │
   └─────────────────────┘
     │
     │ Create League Form
     ▼
   ┌─────────────────────┐
   │  3.0 LEAGUE MGMT    │
   │ • Validate Input    │
   │ • Set Parameters    │
   │ • Create League     │
   └─────────────────────┘
     │
     ├──► ┌──────────────────┐
     │    │D2: League Store  │
     │    │(Name, Duration,  │
     │    │ Min/Max Teams,   │
     │    │ Password)        │
     │    └──────────────────┘
     │
     │ Create/Join Team
     ▼
   ┌─────────────────────┐
   │  4.0 TEAM MGMT      │
   │ • Add Players       │
   │ • Validate Min (6)  │
   │ • Allow Join League │
   └─────────────────────┘
     │
     ├──► ┌──────────────────┐
     │    │ D3: Teams DB     │
     │    │ (Team Name,      │
     │    │  Owner, Players) │
     │    └──────────────────┘
     │
     │ Play Matches
     ▼
   ┌─────────────────────┐
   │  5.0 MATCH ENGINE   │
   │ • Track Score       │
   │ • Record Duration   │
   │ • Calculate Results │
   └─────────────────────┘
     │
     ├──► ┌──────────────────┐
     │    │ D4: Matches DB   │
     │    │ (Teams, Goals,   │
     │    │  Date, Duration) │
     │    └──────────────────┘
     │
     │ Match Result
     ▼
   ┌─────────────────────┐
   │  6.0 LEADERBOARD    │
   │ • Update Stats      │
   │ • Calculate Wins/   │
   │   Losses/Goals      │
   │ • Sort Rankings     │
   └─────────────────────┘
     │
     ├──► ┌──────────────────┐
     │    │ D5: Leaderboard  │
     │    │ (Team Stats:     │
     │    │  Wins, Losses,   │
     │    │  Total Goals)    │
     │    └──────────────────┘
     │
     │ Display Leaderboard
     ▼
   ┌─────────┐
   │  USER   │
   └─────────┘
