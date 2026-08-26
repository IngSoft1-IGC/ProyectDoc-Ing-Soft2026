╔════════════════════════════════════════════════════════════════════════════╗
║                     FOOTBALL LEAGUE SYSTEM - DFD OVERVIEW                  ║
╚════════════════════════════════════════════════════════════════════════════╝

┌─────────────────────────────────────────────────────────────────────────────┐
│                         CONTEXT DIAGRAM (LEVEL 0)                           │
└─────────────────────────────────────────────────────────────────────────────┘

                            ┌───────────────────────┐
                            │ FOOTBALL LEAGUE       │
                            │ MANAGEMENT SYSTEM     │
                            │                       │
                            │ • User Management     │
                            │ • League Management   │
                            │ • Team Management     │
                            │ • Match Engine        │
                            │ • Leaderboard        │
                            └───────────────────────┘
                              ▲                   ▼
                              │                   │
                    ┌─────────┴──┐          ┌─────┴──────────┐
                    │             │          │                 │
                ┌───▼────────┐   ┌───▼──────┐
                │   USER     │   │ DATABASE │
                │  (Client)  │   │ (Storage)│
                │            │   │          │
                └────────────┘   └──────────┘


┌─────────────────────────────────────────────────────────────────────────────┐
│              DETAILED DFD (LEVEL 1) - MAIN PROCESSES & DATA FLOWS           │
└─────────────────────────────────────────────────────────────────────────────┘

                              ╔════════════════════╗
                              ║      USERS         ║
                              ║  (External Actor)  ║
                              ╚════════════════════╝
                                  ▲           ▼
                ┌───────────────────┼───────────┴─────────────┬──────────────┐
                │                   │                         │              │
                ▼                   ▼                         ▼              ▼
        ┌──────────────┐     ┌──────────────┐     ┌──────────────┐  ┌──────────────┐
        │   P1.0       │     │   P2.0       │     │   P3.0       │  │   P4.0       │
        │ User Mgmt    │     │ Auth System  │     │ League Mgmt  │  │ Team Mgmt    │
        │              │     │              │     │              │  │              │
        │ • Validate   │     │ • Login      │     │ • Validate   │  │ • Add Players│
        │   Input      │     │ • Generate   │     │   League     │  │ • Verify Min │
        │ • Check Email│     │   Token      │     │ • Set Params │  │   Players(6) │
        │ • Create     │     │ • Verify     │     │ • Create     │  │ • Join League│
        │   Account    │     │   Password   │     │   League     │  │              │
        └──────┬───────┘     └──────┬───────┘     └──────┬───────┘  └──────┬───────┘
               │                    │                    │                  │
               └────────────────┬───┴────────────────┬───┴──────────────┬───┘
                                │                    │                  │
                     (credentials & tokens)  (league data)     (team data)
                                │                    │                  │
                                ▼                    ▼                  ▼
                        ┌────────────────┐ ┌────────────────┐ ┌────────────────┐
                        │  D1: User DB   │ │ D2: League DB  │ │ D3: Team DB    │
                        │                │ │                │ │                │
                        │ • Username     │ │ • Name         │ │ • Team Name    │
                        │ • Email        │ │ • Duration     │ │ • Owner        │
                        │ • Password     │ │ • Min/Max Tmz  │ │ • Players (≥6) │
                        │ • Name         │ │ • Password     │ │ • Stats        │
                        │ • Avatar       │ │                │ │                │
                        └────────────────┘ └────────────────┘ └────────────────┘
                                                                                
                ┌────────────────────────────────────────────────────────────┐
                │                                                            │
                ▼                                                            ▼
        ┌──────────────┐                                        ┌──────────────┐
        │   P5.0       │                                        │   P6.0       │
        │ Match Engine │                                        │ Leaderboard  │
        │              │                                        │              │
        │ • Track Score│                                        │ • Update     │
        │ • Record Dur │                                        │   Stats      │
        │ • Calculate  │                                        │ • Calculate  │
        │   Results    │                                        │   Standings  │
        │ • Apply PACSS│                                        │ • Sort Rank  │
        │   Stats      │                                        │              │
        └──────┬───────┘                                        └──────┬───────┘
               │                                                       │
               ▼                                                       ▼
        ┌────────────────┐                                   ┌────────────────┐
        │ D4: Match DB   │                                   │D5: Leaderboard │
        │                │                                   │                │
        │ • Teams        │                                   │ • Team Stats   │
        │ • Goals        │                                   │ • Wins         │
        │ • Duration     │                                   │ • Losses       │
        │ • Timestamp    │                                   │ • Total Goals  │
        │ • Players Stats│                                   │ • Rankings     │
        └────────────────┘                                   └────────────────┘
                                                                      ▲
                                                                      │
                                                         (leaderboard results)
                                                                      │
                                                            ┌─────────┴────────┐
                                                            │                  │
                                                    ╔════════════════════╗
                                                    ║      USERS         ║
                                                    ║ (View Leaderboard) ║
                                                    ╚════════════════════╝


┌─────────────────────────────────────────────────────────────────────────────┐
│                    DATA STORES SUMMARY & CONTENTS                           │
└─────────────────────────────────────────────────────────────────────────────┘

D1 - USER DATABASE
├── User ID (Primary Key)
├── Username
├── Email
├── Password (hashed)
├── Full Name
├── Avatar/Profile Picture
├── Created Timestamp
└── Modified Timestamp

D2 - LEAGUE DATABASE
├── League ID (Primary Key)
├── Name
├── Owner (User ID)
├── Match Duration (≤ 5 min)
├── Minimum Teams (≥ 3)
├── Maximum Teams
├── Password (optional, for private leagues)
├── Status (active/inactive)
└── Created Timestamp

D3 - TEAM DATABASE
├── Team ID (Primary Key)
├── Team Name
├── Owner (User ID)
├── League ID (Foreign Key)
├── Players (≥ 6 minimum)
│  ├── Player ID
│  ├── Player Name
│  └── PACSS Stats
│      ├── Power (1-10)
│      ├── Speed (1-10)
│      ├── Dexterity (1-10)
│      ├── Control (1-10)
│      └── Strength (1-10)
└── Created Timestamp

D4 - MATCH DATABASE
├── Match ID (Primary Key)
├── Team A ID
├── Team B ID
├── League ID (Foreign Key)
├── Match Duration (in ticks/minutes)
├── Match Status (scheduled/in-progress/completed)
├── Home Team Goals
├── Away Team Goals
├── Match Timestamp
├── Player Performances
└── Match Records (goals, fouls, etc.)

D5 - LEADERBOARD DATABASE
├── Leaderboard ID (Primary Key)
├── League ID (Foreign Key)
├── Team ID (Foreign Key)
├── Wins (count)
├── Losses (count)
├── Total Goals (sum)
├── Matches Played (count)
├── Last Updated Timestamp
└── Ranking Position


┌─────────────────────────────────────────────────────────────────────────────┐
│                       DATA FLOW DESCRIPTIONS                                │
└─────────────────────────────────────────────────────────────────────────────┘

1. SIGN UP / LOGIN FLOW
   User → P1.0 (User Mgmt) → D1 (User DB)
   • Data: Username, Email, Password, Name, Avatar
   • Response: Validation success/failure, error messages

2. AUTHENTICATION FLOW
   User → P2.0 (Auth System) → D1 (User DB)
   • Data: Credentials (email, password)
   • Response: Auth token, session info

3. LEAGUE CREATION FLOW
   User → P3.0 (League Mgmt) → D2 (League DB)
   • Data: League Name, Duration, Min/Max Teams, Password
   • Response: League created, League ID

4. TEAM MANAGEMENT FLOW
   User → P4.0 (Team Mgmt) → D3 (Team DB)
   • Data: Team Name, Owner, Player List (≥6 players with PACSS stats)
   • Response: Team validated, join confirmation

5. MATCH EXECUTION FLOW
   P4.0/P5.0 (Team & Match) → D4 (Match DB)
   • Data: Teams, Goals, Duration, Player Stats, Match Results
   • Response: Match outcome recorded

6. LEADERBOARD UPDATE FLOW
   P5.0 (Match Engine) → P6.0 (Leaderboard) → D5 (Leaderboard DB)
   • Data: Match Results, Team Stats, Goal Tallies
   • Response: Updated rankings, standings

7. DISPLAY FLOW
   D5 (Leaderboard DB) → P6.0 (Leaderboard) → User
   • Data: Team Rankings, Wins, Losses, Total Goals
   • Response: Leaderboard display


┌─────────────────────────────────────────────────────────────────────────────┐
│                     KEY SYSTEM CHARACTERISTICS                              │
└─────────────────────────────────────────────────────────────────────────────┘

EXTERNAL ACTORS:
• Users (Players, League Creators, Team Owners)

MAIN PROCESSES:
1. P1.0: User Management (registration & account creation)
2. P2.0: Authentication System (login & token generation)
3. P3.0: League Management (create & manage leagues)
4. P4.0: Team Management (create teams & manage players)
5. P5.0: Match Engine (execute matches, track scores)
6. P6.0: Leaderboard (update & display statistics)

DATA STORES:
• D1: User Database
• D2: League Database
• D3: Team Database
• D4: Match Database
• D5: Leaderboard Database

KEY CONSTRAINTS:
• Minimum 3 teams required per league
• Minimum 6 players per team
• Match duration ≤ 5 minutes
• PACSS stat distribution: 10 points per player (min 3 per stat)
• League can be public or private (password-protected)
• All teams in a league must play each other
