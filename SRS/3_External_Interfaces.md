# External Interfaces and System Architecture

## 3.1 User Interfaces

### 3.1.1 Authentication Interface
- **Sign Up Form:** Username, Email, Password, Name, Avatar upload
- **Login Form:** Email, Password fields
- **Profile Management:** Editable fields for Name, Email, Avatar, Password change
- **Session Management:** Logout button, Session timeout notifications

### 3.1.2 Team Management Interface
- **Team Creation Form:** Team name input, player dropdown selectors (3 Main + 3 Replacement)
- **Team List View:** Team cards displaying name, owner, player count, status
- **Team Deletion Confirmation:** Warning dialog with confirmation button
- **Player Selection Dropdowns:** Filtered lists showing available players only

### 3.1.3 League Management Interface
- **League Creation Form:** 
  - League name, Match duration, Start date/time
  - Min/Max teams configuration
  - Password field (optional)
  - Team selection checkboxes
- **League List View:** League cards with status (Inscription/In Progress/Completed)
- **League Leaderboard:** Table showing team rankings, points, goals for/against
- **League Start Confirmation:** Dialog requiring owner confirmation

### 3.1.4 Behavior Management Interface
- **Behavior Editor:** Code editor for Python script input
- **Behavior List View:** List of created behaviors with metadata
- **Syntax Validation Display:** Error messages with line numbers
- **Behavior Assignment:** Dropdown for selecting behavior to assign to players

### 3.1.5 Match Viewing Interface
- **Match Scoreboard:** Live score display (3v3 format)
- **Player Status Panel:** Shows current players on field
- **Substitution Controls:** Dropdown to select replacement players (only during breaks)
- **Match Time Display:** Current time and quarter information
- **Match Results Screen:** Final score, statistics, leaderboard update confirmation

### 3.1.6 Rankings and Statistics
- **Global Ranking View:** Table of all users sorted by points
- **League-specific Leaderboard:** Table within league showing per-team statistics
- **Player Statistics:** Individual player performance metrics

---

## 3.2 Hardware Interfaces

### Server Side
- **Database Server:** SQL database for persistent storage (compatible with SQLAlchemy)
- **Application Server:** FastAPI server for backend logic
- **Python Runtime:** Python 3.x environment for behavior execution
- **Sandbox Environment:** Isolated execution environment for user-submitted Python code

### Client Side
- **Web Browser:** HTML5-compatible browser with WebSocket support
- **JavaScript Engine:** ECMAScript 2015+ compatible for React execution
- **Network Interface:** Stable internet connection (WebSocket for real-time updates)

---

## 3.3 Software Interfaces

### Backend APIs
- **User Management API:** 
  - POST /auth/signup - User registration
  - POST /auth/login - User authentication
  - PUT /auth/profile - Profile updates
  - POST /auth/logout - Session termination

- **Team Management API:**
  - POST /teams - Create team
  - GET /teams - List user's teams
  - DELETE /teams/{id} - Delete team
  - GET /players - List available players

- **League Management API:**
  - POST /leagues - Create league
  - GET /leagues - List leagues
  - PUT /leagues/{id}/start - Start league
  - GET /leagues/{id}/leaderboard - Get standings

- **Behavior Management API:**
  - POST /behaviors - Create behavior
  - PUT /behaviors/{id} - Modify behavior
  - DELETE /behaviors/{id} - Delete behavior
  - POST /behaviors/{id}/validate - Syntax validation

- **Match Engine API:**
  - POST /matches - Create/start match
  - GET /matches/{id} - Get match state
  - POST /matches/{id}/substitute - Process substitution
  - GET /matches/{id}/results - Get match results

### Database Interfaces (SQLAlchemy ORM)
- **User DB (D1):** User credentials, profiles, session tokens
- **Teams DB (D2):** Team definitions, player assignments, team ownership
- **Matches DB (D3):** League definitions, match schedules, match results, leaderboards
- **Bots DB (D4):** Behavior definitions, behavior-player assignments, Python code storage

### Communication Protocols
- **HTTP/HTTPS:** RESTful API endpoints
- **WebSocket:** Real-time match updates and bidirectional communication
- **JSON:** Data serialization format for all requests/responses

---

## 3.4 Communication Interfaces

### Client-Server Communication
- **Protocol:** WebSocket (primary) for real-time updates, HTTP POST/GET for standard requests
- **Frequency:** 
  - Match updates: Every game tick (determined by system configuration)
  - League leaderboard updates: Immediately upon match completion
  - User actions: On-demand (immediate response)
- **Format:** JSON-encoded messages
- **Error Handling:** HTTP status codes (200, 400, 401, 403, 404, 500) with error message details

### Data Flow Architecture

**DFD Level 0 - System Context:**
```
User (Client) ↔ Football Game System ↔ Database (Data Store)
```

**DFD Level 1 - Core Processes:**
```
1.0 User/Profile Management (D1: User DB)
2.0 Team Management (D2: Teams DB)
3.0 League and Matches Engine (D3: Matches DB)
4.0 Bots and Behaviors Engine (D4: Bots DB)
```

### Data Stores (External Entity Dependencies)

| Data Store | Owner | Contents | Access Pattern |
|------------|-------|----------|-----------------|
| **D1: User DB** | Process 1.0 | User credentials, profiles, sessions | Read/Write by authentication processes |
| **D2: Teams DB** | Process 2.0 | Team definitions, player stats, PACSS | Read/Write by team management processes |
| **D3: Matches DB** | Process 3.0 | Leagues, match schedules, results, leaderboards | Read/Write by match engine |
| **D4: Bots DB** | Process 4.0 | Behavior scripts, Python code, behavior assignments | Read by match engine, Write by behavior management |

---

## 3.5 Class Diagram Overview

### Core Entity Relationships

**User (Principal Entity)**
- Owns exactly 1 Club/Team account (1:1 relationship)
- Creates multiple Behaviors (1:* relationship)
- Associated with Session tokens for authentication

**Equipo (Team)**
- Belongs to 1 User (owned by) (*:1 relationship)
- Composes exactly 6 Players (3 Main + 3 Replacement) (*:6 relationship)
- Participates in 0 or 1 League at a time (*:1 relationship)
- Disputes 2 Teams per Match (*:2 relationship)

**Jugador (Player)**
- Belongs to exactly 1 Team (*:1 relationship)
- Executes exactly 1 Behavior assignment per match (can be different per match)
- Has immutable PACSS statistics (created at player instantiation)
- Identified by dorsal (jersey number) within team

**Comportamiento (Behavior)**
- Created by 1 User (*:1 relationship)
- Assigned to multiple Players via behavior-player junction (1:* relationship)
- Contains Python code stored as string in database
- Validated before storage (syntax, security, sandboxing)

**Liga (League)**
- Created by 1 User (owner) (*:1 relationship)
- Inscribes multiple Teams (minimum 3) (1:* relationship)
- Manages exactly 1 TablaPosiciones (1:1 relationship)
- Programs multiple PartidoLiga fixtures (1:* relationship)

**Partido (Match - Abstract Base)**
- Disputes 2 Teams (per match rule) (*:2 relationship)
- Contains exactly 1 Ball (Pelota) (1:1 relationship)
- Has Match state (playing, paused, completed)
- Subtypes:
  - PartidoLiga: Associated with specific League
  - PartidoAmistoso: Standalone friendly match

**TablaPosiciones (Leaderboard)**
- Tracks standings for exactly 1 League (1:1 relationship)
- Maintains per-team statistics:
  - Puntos (Points): 3 for win, 1 for draw, 0 for loss
  - Wins, Losses, Draws count
  - Goles Favor / Goles Contra
  - Diferencia Goles (goal differential)

**RankingGlobal (Global Rankings)**
- Aggregates TablaPosiciones from all Leagues
- Accumulates user points across all league participations
- Updates in real-time as matches complete

---

## 3.6 Design Patterns and Constraints

### Authentication Pattern
- **Session-based:** User logs in → Session token generated → Token validated on each request
- **Password Security:** Hashing with salt (password_ruido in schema)
- **Unique Email:** Database constraint on email field prevents duplicates

### Behavior Validation Pipeline
1. **Syntax Validation:** Python 3.x AST parsing to detect syntax errors
2. **Security Validation:** Scanning for forbidden operations (file I/O, system calls)
3. **Sandbox Isolation:** Restricted execution environment for behavior execution
4. **Type Safety:** Behavior accepts specific input signatures from match engine

### Team Composition Constraint
- **Rigid Structure:** Exactly 3 Main + 3 Replacement = 6 players per team
- **PACSS Validation:** Sum of 5 attributes must equal exactly 300 points per player
- **Attribute Bounds:** Each attribute between 20-100 inclusive

### League Constraints
- **Minimum Teams:** At least 3 teams required to form a valid league
- **Match Duration:** ≤ 5 minutes (system configurable, represented in ticks)
- **Start Time:** Must be > 1 minute from creation time
- **Fixture Structure:** Round-robin (all teams play each other)

### Match Physics
- **Field Boundaries:** Ball bounces off edges (no out of bounds)
- **Team Composition:** 3 players per team on field (no 11v11 traditional format)
- **No Referee:** No offside, no fouls, no goal kicks, corners, throw-ins
- **Substitution Rules:** Only during cooling breaks or halftime
- **Tick-based Simulation:** Discrete time steps for deterministic behavior

---

## 3.7 Technology Stack Constraints

**Backend (Required)**
- Framework: FastAPI (Python web framework)
- ORM: SQLAlchemy (database abstraction)
- Database: SQL-compatible (PostgreSQL, MySQL, SQLite)
- Runtime: Python 3.8+

**Frontend (Required)**
- Framework: React (JavaScript/TypeScript)
- Real-time Communication: WebSocket client
- Styling: CSS3-compatible
- Build: Node.js/npm ecosystem

**Communication**
- ✅ WebSocket: REQUIRED for real-time match updates
- ❌ Polling: FORBIDDEN (explicitly prohibited in requirements)
- REST APIs: Standard HTTP methods for non-real-time operations

**Python Sandbox**
- Environment: Isolated from system OS
- Libraries: Restricted to game engine API only
- File System: No access to host filesystem
- Network: No external network access
- Process Creation: No subprocess or system calls allowed

---

## 3.8 Non-Functional Interface Requirements

### Performance
- **API Response Time:** < 2 seconds for most operations
- **Database Query Time:** < 5 seconds including complex leaderboard updates
- **WebSocket Latency:** < 100ms for match state propagation

### Reliability
- **Uptime:** System must recover from server crashes without data loss
- **Data Persistence:** All match results, team data, behaviors persisted immediately
- **Connection Recovery:** WebSocket reconnection on network interruption

### Security
- **HTTPS/WSS:** All communication encrypted in transit
- **CORS:** Restricted cross-origin access
- **Input Validation:** All user inputs sanitized and validated
- **SQL Injection Prevention:** Parameterized queries via SQLAlchemy
- **XSS Prevention:** React's built-in HTML escaping

### Scalability
- **Concurrent Users:** System must support multiple simultaneous matches
- **Database Indexing:** Efficient queries for leaderboard updates
- **Caching:** User data, behavior definitions cached to reduce DB load
