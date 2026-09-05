# Functional Requirements

## 1.0 User/Profile Management

### 1.1 Use Case: Sign Up (Create New User)

**Actor:** Guest

**Brief Description:**
A new user creates an account to access the system and begin playing the game.

**Preconditions:**
- The user is not currently logged in

**Inputs:**
1. Username
2. Email
3. Password
4. Name
5. Avatar

**Successful Flow:**
1. User selects the Sign Up option
2. System displays the Sign-up form with fields for Username, Email, Password, Name, and Avatar
3. User enters required details and clicks Submit
4. System validates input formats and verifies email uniqueness
5. System creates the new user account in the database
6. System displays a confirmation message and provides a link/button to the Login Screen

**Exceptional Scenarios:**
- **Exception 1 - Email Already in Use:**
  - Trigger: Email validation fails (Step 4)
  - Flow: System displays error indicating email is already in use and prompts user to log in or use a different email

- **Exception 2 - Missing Input Fields:**
  - Trigger: Input validation fails (Step 4)
  - Flow: System displays error indicating missing fields and prompts user to fill all required fields

**Post-conditions:**
1. A new account is active in the system database
2. The user is ready to log in

---

### 1.2 Use Case: User Login

**Actor:** Registered User

**Brief Description:**
A registered user logs into the system to access their dashboard and manage their clubs, teams, and leagues.

**Preconditions:**
- User account exists in the database
- User is not currently logged in

**Inputs:**
1. Email
2. Password

**Successful Flow:**
1. User enters email and password on the login form
2. System validates field formats
3. System authenticates credentials against the database
4. System generates a user session
5. System displays the main menu/dashboard

**Exceptional Scenarios:**
- **Exception 1 - Field Validation Errors:**
  - Trigger: Invalid email format or empty fields (Step 2)
  - Flow: System displays field validation errors and prompts user to correct them

- **Exception 2 - Invalid Credentials:**
  - Trigger: Email not found or password mismatch (Step 3)
  - Flow: System displays "Invalid Email or Password" error

**Post-conditions:**
1. User session is created
2. User is authenticated and can access the system
3. Last login timestamp is updated in the database

---

### 1.3 Use Case: Edit User Profile

**Actor:** User (Logged In)

**Brief Description:**
A user views and updates their profile information including name, email, avatar, and password.

**Preconditions:**
- User is logged in

**Inputs:**
1. Name (Optional update)
2. Email (Optional update)
3. Avatar (Optional update)
4. Old Password (Required if changing password or sensitive data)
5. New Password (Optional update)

**Successful Flow:**
1. User accesses the profile management section
2. System displays current user information
3. User modifies desired fields
4. System validates input formats
5. System updates database with new information
6. System confirms changes to the user

**Exceptional Scenarios:**
- **Exception 1 - Invalid Email Format:**
  - Trigger: Email format validation fails (Step 4)
  - Flow: System displays error indicating invalid email format

- **Exception 2 - Password Mismatch:**
  - Trigger: Old password does not match stored password (Step 4)
  - Flow: System displays error indicating incorrect old password

**Post-conditions:**
1. Updated user profile is saved in the database
2. User sees updated information after successful save

---

## 2.0 Player Management

### 2.1 Use Case: Create Player

**Actor:** User (Logged In)

**Brief Description:**
A user creates a new player with customizable PACSS attributes.

**Preconditions:**
- User is logged in
- User has access to player creation interface

**Inputs:**
1. Player name
2. PACSS attributes (Power, Speed, Dexterity, Control, Strength)
3. Team assignment (optional at creation)

**Successful Flow:**
1. User accesses player creation interface
2. System displays form for player creation
3. User enters player name and PACSS values
4. System validates that exactly 10 points are allocated across all attributes
5. System validates minimum value of 1 point per attribute
6. System creates player record in database
7. System assigns player to team if specified
8. System displays confirmation message

**Exceptional Scenarios:**
- **Exception 1 - Invalid PACSS Allocation:**
  - Trigger: Total points not equal to 10 (Step 4)
  - Flow: System displays error indicating incorrect point allocation

- **Exception 2 - Attribute Below Minimum:**
  - Trigger: Any attribute below 1 point (Step 5)
  - Flow: System displays error indicating minimum point requirement

**Post-conditions:**
1. New player record is created in database
2. Player is available for team assignment
3. Player can be assigned to a team or used in behaviors

---

### 2.2 Use Case: Update Player

**Actor:** User (Logged In)

**Brief Description:**
A user modifies existing player information including PACSS attributes.

**Preconditions:**
- User is logged in
- Player exists in the database
- Player has not been assigned to an active match

**Inputs:**
1. Player name (optional)
2. Updated PACSS attributes
3. Team assignment (optional)

**Successful Flow:**
1. User accesses player management interface
2. System displays current player information
3. User modifies desired fields
4. System validates PACSS allocation rules
5. System updates database with new information
6. System confirms changes to the user

**Exceptional Scenarios:**
- **Exception 1 - Invalid PACSS Allocation:**
  - Trigger: Total points not equal to 10 (Step 4)
  - Flow: System displays error indicating incorrect point allocation

**Post-conditions:**
1. Updated player record is saved in the database
2. Player can be used with updated attributes

---

## 3.0 Team Management

### 3.1 Use Case: Create Team

**Actor:** User (Logged In)

**Brief Description:**
A user creates a new team for organizing players and participating in leagues.

**Preconditions:**
- User is logged in
- User has access to team creation interface

**Inputs:**
1. Team name
2. Player assignments (minimum 6 players required)
3. Team captain selection

**Successful Flow:**
1. User accesses team creation interface
2. System displays form for team creation
3. User enters team name and selects players
4. System validates minimum of 6 players requirement
5. System creates team record in database
6. System assigns players to the team
7. System displays confirmation message

**Exceptional Scenarios:**
- **Exception 1 - Insufficient Players:**
  - Trigger: Less than 6 players selected (Step 4)
  - Flow: System displays error indicating minimum player requirement

- **Exception 2 - Player Already Assigned:**
  - Trigger: Attempt to assign player already in another team (Step 4)
  - Flow: System displays error indicating player is already assigned

**Post-conditions:**
1. New team record is created in database
2. Team is available for league participation
3. Players are assigned to the team

---

### 3.2 Use Case: Manage Team Players

**Actor:** User (Logged In)

**Brief Description:**
A user adds or removes players from an existing team.

**Preconditions:**
- User is logged in
- Team exists in database
- User owns the team

**Inputs:**
1. Player to add/remove
2. Action type (add or remove)

**Successful Flow:**
1. User accesses team management interface
2. System displays current team players
3. User selects player and action
4. System validates action (add/remove)
5. System updates team-player relationships
6. System confirms changes

**Exceptional Scenarios:**
- **Exception 1 - Adding Player Already in Team:**
  - Trigger: Attempt to add player already assigned to this team (Step 4)
  - Flow: System displays error indicating player is already on team

- **Exception 2 - Removing Player from Team:**
  - Trigger: Attempt to remove player not assigned to team (Step 4)
  - Flow: System displays error indicating player is not on team

**Post-conditions:**
1. Team-player relationships are updated in database
2. Team composition is reflected in system

---

## 4.0 League Management

### 4.1 Use Case: Create League

**Actor:** User (Logged In)

**Brief Description:**
A user creates a new league with specific parameters for team competition.

**Preconditions:**
- User is logged in
- User has access to league creation interface
- Minimum 3 teams available for league creation

**Inputs:**
1. League name
2. League type (public/private)
3. Password (for private leagues)
4. Duration parameters
5. Team limits
6. Competition rules

**Successful Flow:**
1. User accesses league creation interface
2. System displays form for league creation
3. User enters league details
4. System validates minimum team requirement
5. System creates league record in database
6. System assigns teams to league
7. System displays confirmation message

**Exceptional Scenarios:**
- **Exception 1 - Insufficient Teams:**
  - Trigger: Less than 3 teams provided (Step 4)
  - Flow: System displays error indicating minimum team requirement

- **Exception 2 - Invalid League Name:**
  - Trigger: Empty or duplicate league name (Step 3)
  - Flow: System displays error indicating invalid name

**Post-conditions:**
1. New league record is created in database
2. League is ready for match scheduling
3. Teams are assigned to the league

---
### 4.2 Use Case: Join League
### Todo...

---

### 4.3 Use Case: Schedule Matches

**Actor:** User (Logged In)

**Brief Description:**
A user schedules matches between teams within a league.

**Preconditions:**
- User is logged in
- League exists in database
- Teams are assigned to league
- Match scheduling interface is accessible

**Inputs:**
1. Match date and time
2. Home team
3. Away team
4. Match duration
5. Location (optional)

**Successful Flow:**
1. User accesses match scheduling interface
2. System displays available teams in league
3. User selects teams for match
4. System validates match parameters
5. System schedules match in database
6. System displays confirmation message

**Exceptional Scenarios:**
- **Exception 1 - Teams Not in Same League:**
  - Trigger: Selected teams not both in the same league (Step 3)
  - Flow: System displays error indicating teams must be in same league

- **Exception 2 - Overlapping Schedule:**
  - Trigger: Scheduled match conflicts with existing match (Step 4)
  - Flow: System displays error indicating schedule conflict

**Post-conditions:**
1. Match is scheduled in database
2. Match appears in league calendar
3. Teams are notified of upcoming match

---
### 4.4 Use Case: Leave League
### Todo...

---

## 5.0 Behavior Management

### 5.1 Use Case: Create Behavior

**Actor:** User (Logged In)

**Brief Description:**
A user creates a new behavior script for player AI actions during matches.

**Preconditions:**
- User is logged in
- User has access to behavior creation interface
- Valid Python syntax knowledge required

**Inputs:**
1. Behavior name
2. Python script content
3. Description of behavior purpose

**Successful Flow:**
1. User accesses behavior creation interface
2. System displays form for behavior creation
3. User enters behavior details and script
4. System validates Python syntax
5. System saves behavior to database
6. System displays confirmation message

**Exceptional Scenarios:**
- **Exception 1 - Invalid Python Syntax:**
  - Trigger: Script fails Python syntax validation (Step 4)
  - Flow: System displays syntax error and prompts user to correct

- **Exception 2 - Security Violation:**
  - Trigger: Script contains restricted operations (Step 4)
  - Flow: System displays security error and rejects script

**Post-conditions:**
1. New behavior record is created in database
2. Behavior is available for player assignment
3. Behavior can be executed in sandboxed environment

---

### 5.2 Use Case: Assign Behavior to Player

**Actor:** User (Logged In)

**Brief Description:**
A user assigns a behavior script to a specific player for match execution.

**Preconditions:**
- User is logged in
- Behavior exists in database
- Player exists in database
- Player is not currently in an active match

**Inputs:**
1. Player selection
2. Behavior selection
3. Assignment confirmation

**Successful Flow:**
1. User accesses behavior assignment interface
2. System displays available players and behaviors
3. User selects player and behavior
4. System validates assignment compatibility
5. System assigns behavior to player
6. System displays confirmation message

**Exceptional Scenarios:**
- **Exception 1 - Player in Active Match:**
  - Trigger: Attempt to assign behavior to player in active match (Step 4)
  - Flow: System displays error indicating player cannot be modified during match

**Post-conditions:**
1. Behavior is assigned to player
2. Player will execute this behavior during matches
3. Assignment is recorded in database
---
### 5.3 Use Case: Delete Behavior
### Todo...

---

## 6.0 Match Execution

### 6.1 Use Case: Execute Match

**Actor:** System

**Brief Description:**
The system executes a scheduled match with AI-driven player behaviors.

**Preconditions:**
- Match is scheduled and active
- Teams have players assigned
- Players have behaviors assigned
- System has access to WebSocket for real-time updates

**Inputs:**
1. Match start time
2. Team compositions
3. Player behaviors
4. Match parameters

**Successful Flow:**
1. System identifies scheduled match
2. System initializes match environment
3. System loads player behaviors
4. System starts match simulation
5. System executes match quarters with real-time updates
6. System updates match results in database
7. System sends final results via WebSocket

**Exceptional Scenarios:**
- **Exception 1 - Missing Player Behaviors:**
  - Trigger: Player without assigned behavior (Step 3)
  - Flow: System assigns default behavior or displays error

- **Exception 2 - Match Execution Failure:**
  - Trigger: System error during simulation (Step 4)
  - Flow: System logs error and stops match execution

**Post-conditions:**
1. Match results are stored in database
2. League standings are updated
3. Spectators receive final results via WebSocket

---
### 6.2 Use Case: Spectate Match
### Todo...

---

### 6.3 Use Case: Switch Player

### Todo...

---

### 6.4 Use Case: Switch Player Behavior

### Todo...