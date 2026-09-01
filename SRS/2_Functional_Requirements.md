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

### 1.3 Use Case: Manage User Profile

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
1. User clicks on their profile avatar in the navigation bar
2. System displays the Profile page with current Name, Username, Email, and Avatar
3. User updates desired fields and clicks "Save Changes"
4. System validates input formats, verifies new email uniqueness (if changed), and confirms old password matches (if password update requested)
5. System updates account details in the database
6. System displays success message and reloads the updated Profile page

**Alternative Scenarios:**
- **Alternative 1 - View Without Changes:**
  - Trigger: User navigates away or clicks "Cancel" (Step 3)
  - Flow: System discards unsaved changes and redirects to Main Menu without modifying database

**Exceptional Scenarios:**
- **Exception 1 - Empty or Invalid Fields:**
  - Trigger: Input validation fails (Step 4)
  - Flow: System highlights invalid fields (e.g., malformed email) and prompts user to correct them

- **Exception 2 - Email Already in Use:**
  - Trigger: Email uniqueness validation fails (Step 4)
  - Flow: System displays error indicating email is already registered to another account

- **Exception 3 - Incorrect Old Password:**
  - Trigger: Password validation fails (Step 4)
  - Flow: System highlights error and prompts user to correct the password and try again

**Post-conditions:**
1. User's updated profile information is saved in the database
2. UI reflects updated account information

---

## 2.0 Team Management

### 2.1 Use Case: Create New Team

**Actor:** User (Logged In)

**Brief Description:**
A user creates a new team using their previously created players to prepare for matches.

**Preconditions:**
1. User is logged in
2. User has at least 6 available players created in their account

**Inputs:**
1. Team name
2. 3 Main players (Center, Upper Defendant, Lower Defendant)
3. 3 Replacement players

**Successful Flow:**
1. User clicks "Create New Team" button
2. System displays Team Creation form with fields for Team name and dropdown menus for main and replacement players
3. User fills out form and clicks "Submit"
4. System validates inputs and verifies that selected players are not assigned to another team
5. System creates the team and adds entry to the database
6. System displays confirmation message and returns to previous page

**Exceptional Scenarios:**
- **Exception 1 - Missing or Invalid Fields:**
  - Trigger: Input validation fails (Step 4)
  - Flow: System highlights missing or invalid fields with error messages and prompts user to correct them

- **Exception 2 - Player Already Assigned:**
  - Trigger: Player verification fails (Step 4)
  - Flow: System displays error listing players already in teams and prompts user to use different players

**Post-conditions:**
1. New team is registered in the database with user as owner
2. Selected players are marked as assigned to the new team
3. Team is ready to play matches

---

### 2.2 Use Case: Delete Team

**Actor:** User (Logged In)

**Brief Description:**
A user removes an existing team from the system.

**Preconditions:**
1. User is authenticated
2. At least one team exists in the system
3. Team is not currently participating in an active league

**Successful Flow:**
1. User selects the team to delete from the list
2. User clicks "Delete Team" button
3. System requests confirmation for the action
4. User confirms the action
5. System deletes the team from the database
6. System displays success message

**Exceptional Scenarios:**
- **Exception 1 - User Cancels Confirmation:**
  - Trigger: User clicks cancel at Step 3
  - Flow: System cancels operation without modifying data and returns to team list

- **Exception 2 - Database Deletion Failure:**
  - Trigger: System fails to delete record (Step 5)
  - Flow: System detects failure, displays error message, and team remains in system

**Post-conditions:**
1. Team is removed from the database
2. Players previously assigned to team are now available for other teams
3. Team no longer appears in user's team list

---

## 3.0 League and Matches Engine

### 3.1 Use Case: Create New League

**Actor:** User (Logged In)

**Brief Description:**
A user creates a new league for teams to compete against each other in a structured tournament format.

**Preconditions:**
1. User is logged in
2. User is on the main menu
3. User has at least one team with minimum 6 players (3 Main + 3 Replacement)
4. User's teams are not currently playing in another league

**Inputs:**
1. League name
2. Match duration
3. Match start date/time
4. Minimum number of teams
5. Maximum number of teams
6. Password (optional)
7. User's participating team(s)

**Successful Flow:**
1. User clicks "Create New League" button
2. System displays league creation form requiring League name, Match duration, Minimum/Maximum teams, optional Password, and participating team(s)
3. User fills required fields and clicks Submit
4. System validates inputs and verifies:
   - Minimum participating teams is at least 3
   - Maximum participating teams is within system constraints
   - Match duration is ≤ 5 minutes (or equivalent in ticks)
   - Match starting date is greater than 1 minute from current time
   - User's teams are not participating in another league
   - All participating teams have minimum 6 players
5. System creates the league and initializes leaderboard
6. System displays confirmation message and redirects to newly created league page

**Exceptional Scenarios:**
- **Exception 1 - Missing or Invalid Fields:**
  - Trigger: Input verification fails (Step 4)
  - Flow: System highlights missing or invalid fields with error messages and prompts user to correct them

- **Exception 2 - Team Already in Another League:**
  - Trigger: Team participation validation fails (Step 4)
  - Flow: System displays error listing teams already in other leagues and prompts user to use different teams

- **Exception 3 - Insufficient Team Members:**
  - Trigger: Player count validation fails (Step 4)
  - Flow: System displays error indicating which teams don't have minimum required players

**Post-conditions:**
1. New league is created and registered in database
2. League leaderboard is initialized with participating teams
3. League is ready to accept match results
4. All participating teams are marked as unavailable for other leagues

**League Structure:**
- Each league has a local leaderboard tracking per team:
  - Team name
  - Owner (User who created the team)
  - Wins
  - Losses
  - Total Goals
- All teams within a league must play matches against each other
- Leagues may be open (public) or private (password-protected)
- To join a league, each team must have minimum of 6 players
- Users may play friendly matches outside of league structure at any time

---

### 3.2 Match Duration and Constraints

**Match Parameters:**
- Match duration: ≤ 5 minutes (or equivalent in ticks as configured by system)
- Minimum start time offset: > 1 minute from current time
- Teams must complete all fixtures within the league
- Friendly matches can be played outside of league structure at any time

---

## 4.0 Bots and Behaviors Engine

### 4.1 Use Case: Create Behavior

**Actor:** User (Logged In)

**Brief Description:**
A user creates a Python script defining how a player behaves and makes decisions during match execution.

**Preconditions:**
1. User is logged in
2. User is on the Behaviors management screen

**Inputs:**
1. Behavior name
2. Python script (.py file or inline code)

**Successful Flow:**
1. User clicks "Create New Behavior" button
2. System displays behavior creation form with input for behavior name and code editor/file upload
3. User enters behavior name and Python code and clicks Submit
4. System validates that fields are not empty and verifies Python syntax
5. System saves behavior to database
6. System displays confirmation and redirects to Behaviors screen

**Exceptional Scenarios:**
- **Exception 1 - Missing Code or File:**
  - Trigger: Field validation fails (Step 4)
  - Flow: System displays error and prompts user to upload file or write Python script

- **Exception 2 - Code Syntax Errors:**
  - Trigger: Syntax check fails (Step 4)
  - Flow: System displays error with specific syntax error details and prompts user to correct

**Post-conditions:**
1. Behavior is stored in database with unique identifier
2. Behavior is available for assignment to players

---

### 4.2 Use Case: Modify Behavior

**Actor:** User (Logged In)

**Brief Description:**
A user updates an existing player behavior script to alter or fix how a player operates in-game.

**Preconditions:**
1. User is logged in
2. User has selected a specific existing behavior to modify from the Behaviors screen
3. Behavior is not currently assigned to any active players

**Inputs:**
1. Behavior name (optional update)
2. Python script or .py file describing the new behavior

**Successful Flow:**
1. User clicks "Modify Behavior" button for a selected behavior
2. System displays modification form populated with current behavior name and Python code
3. User edits code (or uploads new .py file) and clicks "Save Changes"
4. System validates that fields are not empty, checks that script is not currently assigned to active players, and verifies Python syntax
5. System displays confirmation prompt: "Are you sure you want to modify this behavior? This action cannot be undone."
6. User clicks "I Understand"
7. System updates behavior entry in database
8. System displays success notification and redirects to Behaviors screen

**Alternative Scenarios:**
- **Alternative 1 - User Rejects Changes:**
  - Trigger: User clicks "Cancel" at Step 3 or 6
  - Flow: System discards unsaved changes and redirects to Behaviors screen without modifying database

**Exceptional Scenarios:**
- **Exception 1 - Missing Code:**
  - Trigger: Field validation fails (Step 4)
  - Flow: System displays error and prompts user to upload file or write script

- **Exception 2 - Code Syntax Errors:**
  - Trigger: Syntax check fails (Step 4)
  - Flow: System displays error with specific syntax error and prompts user to correct

- **Exception 3 - Behavior Assigned to Active Players:**
  - Trigger: Dependency check fails (Step 4)
  - Flow: System displays error listing players using this behavior and prompts user to unassign them first

**Post-conditions:**
1. Behavior record is updated with new Python code
2. Updated behavior logic applies to any subsequent matches
3. All players using this behavior will execute the new logic

---

### 4.3 Use Case: Delete Behavior

**Actor:** User (Logged In)

**Brief Description:**
A user removes an existing behavior from the system.

**Preconditions:**
1. User is authenticated
2. At least one behavior exists in the system
3. Behavior is not currently assigned to any active players

**Successful Flow:**
1. User selects the behavior to delete from the list
2. User clicks "Delete Behavior" button
3. System requests confirmation for the action
4. User confirms the action
5. System deletes the behavior from the database
6. System displays success message

**Exceptional Scenarios:**
- **Exception 1 - User Cancels Confirmation:**
  - Trigger: User clicks cancel at Step 3
  - Flow: System cancels operation without modifying data and returns to behaviors list

- **Exception 2 - Database Deletion Failure:**
  - Trigger: System fails to delete record (Step 5)
  - Flow: System detects failure, displays error message, and behavior remains in system

- **Exception 3 - Behavior in Use by Active Players:**
  - Trigger: Behavior is linked to active players (Step 5)
  - Flow: System detects linkage, displays error with list of affected players, and behavior remains in system

**Post-conditions:**
1. Behavior is removed from database
2. Behavior is no longer available for assignment
3. Players that had this behavior assigned remain unchanged (can be manually updated separately)

---

## Summary of Functional Areas

| Process | Use Cases | Primary Responsibility |
|---------|-----------|------------------------|
| **1.0 User/Profile Management** | Sign Up, Login, Manage Profile | User registration, authentication, profile maintenance |
| **2.0 Team Management** | Create Team, Delete Team | Team organization and player assignment |
| **3.0 League and Matches** | Create League | League administration and match scheduling |
| **4.0 Bots and Behaviors** | Create Behavior, Modify Behavior, Delete Behavior | AI behavior scripting and management |

---

## Non-Functional Requirements (Preliminary)

**Performance:**
- System should respond to user actions within 2 seconds
- Database queries should complete within 5 seconds
- League leaderboards should update in real-time as matches complete

**Security:**
- All passwords must be encrypted using industry-standard algorithms
- User sessions should timeout after 30 minutes of inactivity
- Email validation must prevent duplicate accounts

**Data Integrity:**
- System must maintain consistency across all four databases (User DB, Teams DB, Matches DB, Bots DB)
- No team can be deleted while participating in an active league
- No behavior can be modified while assigned to active players

**Usability:**
- All forms must provide clear error messages indicating what fields are invalid
- All confirmation prompts must be explicit and require intentional user action
- System should provide feedback for all operations (creation, update, deletion)
