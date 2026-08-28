# Software Requirements Specification (SRS)
## Futbot - Football Game Management System

### 1. Introduction and Overview

#### 1.1 Purpose
This Software Requirements Specification (SRS) defines the functional and non-functional requirements for Futbot, a football game management system. The system enables users to create and manage player profiles, organize teams, establish leagues, and simulate football matches with AI-driven behaviors.

#### 1.2 Project Scope
Futbot is a software application designed as an educational project for a Software Engineering course. It serves as a real-world example of a professional working environment and provides practical experience in software development methodologies.

The system will provide:
- User registration and profile management
- Player creation with customizable statistics (PACSS)
- Team organization and management
- League creation and administration
- Behavior scripting for AI-driven player actions
- Match simulation and leaderboard tracking

#### 1.3 System Environment

**Primary Actor:**
- **User (Club Owner):** Creates and manages leagues, owns and manages multiple teams, creates and configures players

**Secondary Actors:**
- **System:** Stores data, validates inputs, executes match simulations, manages behaviors

#### 1.4 Key Entities

**User:**
- A person interacting with the system
- Can create multiple teams and leagues
- Manages player profiles and behaviors
- Owns clubs and teams

**Player:**
- Members of a team
- Have configurable PACSS statistics
- Can be assigned to teams (not multiple teams simultaneously)
- Execute behaviors during matches
- Require at least 6 players per team for league participation

**Team:**
- Consists of multiple players (minimum 6 players required for league participation)
- Composed of 3 Main players (Center, Upper Defendant, Lower Defendant) and 3 Replacement players for matches
- Owned by a user
- Can participate in leagues and friendly matches
- Cannot participate in multiple leagues simultaneously

**League:**
- Collection of teams competing together
- Has a local leaderboard tracking team statistics
- Contains scheduled and completed matches
- Can be open (public) or private (password-protected)
- Minimum 3 teams required to create a league
- All teams within a league play matches against each other

**Behavior:**
- Python script defining how a player acts during a match
- Created and managed by users
- Assigned to players for match execution
- Can be modified or deleted (with restrictions on active assignments)

**PACSS:**
- Player attribute system with 5 statistics:
  - **Power:** Amount of force a player can hit the ball with
  - **Speed:** Movement speed of the player
  - **Dexterity:** Time required for the player to kick the ball again (recovery time)
  - **Control:** Maximum distance from which the player can kick the ball
  - **Strength:** Probability of winning a duel against another player
- Players allocate exactly 10 PACSS points during creation
- Minimum of 3 points required per attribute

---

### 2. System Architecture Overview

The system is organized into four core functional areas based on DFD Level 1 processes:

| Process | Responsibility | Data Store |
|---------|-----------------|------------|
| **1.0 User/Profile Management** | User registration, login, profile management | D1: User DB |
| **2.0 Team Management** | Team creation, player assignment, team deletion | D2: Teams DB |
| **3.0 League and Matches Engine** | League creation, match scheduling, leaderboard management | D3: Matches DB |
| **4.0 Bots and Behaviors Engine** | Behavior creation, modification, deletion, Python syntax validation | D4: Bots DB |

---

### 3. Data Flow Overview

**External Entity:**
- User (Client)

**System Inputs:**
- User Credentials (Emails, Passwords, Usernames)
- Team Data (Team names, PACSS stats, Players, Team-specific stats)
- League Data (League names, Leaderboards, Fixtures, Matches)
- Bot Data (Behaviors, Python Scripts, Python Syntax Verification)

**System Outputs:**
- Game State Updates
- UI Updates
- Validation Messages
- Confirmation/Error Messages

---

### 4. Definitions and Terminology

**Main Player:** One of three essential players required for team participation in matches (Center, Upper Defendant, Lower Defendant)

**Replacement Player:** Substitute players (3 required) available to enter matches when main players are unavailable

**Leaderboard:** Ranking system within a league tracking team performance metrics (Wins, Losses, Total Goals, Owner name)

**Friendly Match:** Unofficial matches between teams that are not part of a league structure

**Behavior Assignment:** The act of linking a specific behavior script to a player who will execute that behavior during matches

**Python Syntax Validation:** System verification that ensures behavior scripts contain valid Python code before saving

---

### 5. Constraints and Assumptions

**Constraints:**
- Each team must have exactly 3 Main players and 3 Replacement players for league participation
- Match duration is limited to ≤ 5 minutes or equivalent in ticks
- Minimum match start time must be greater than 1 minute from current time
- Teams cannot participate in multiple leagues simultaneously
- Players cannot be assigned to multiple teams simultaneously
- Behaviors cannot be modified if actively assigned to players (must be unassigned first)
- PACSS point allocation is fixed at 10 points total per player

**Assumptions:**
- Users will have stable internet connectivity
- Python syntax validation will use Python 3.x standards
- All user credentials will be validated and securely stored
- League leaderboards will update in real-time as matches complete
- System will maintain data consistency across all databases
