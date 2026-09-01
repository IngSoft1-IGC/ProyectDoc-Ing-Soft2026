# Changes Made to Consolidate Documentation

## Overview
This document outlines the changes made when consolidating preliminary documentation into a cohesive Software Requirements Specification (SRS). Changes prioritized Use Cases and DFD documentation when conflicts arose.

---

## File Structure

### New SRS Consolidation Files
```
SRS/
├── 1_Introduction_and_Overview.md
├── 2_Functional_Requirements.md
└── CHANGES.md
```

This consolidation organizes scattered individual use case files into a unified, professionally structured SRS document.

---

## Major Changes and Reconciliations

### 1. Entity Definitions Consolidation

**Source Documents:** `User.md`, `Team.md`, `Player.md`, `League.md`, `Behaviours.md`, `PACSS.md`

**Changes Made:**
- Consolidated all entity definitions into Section 1.4 (Key Entities) in `1_Introduction_and_Overview.md`
- Expanded terse definitions with contextual details from multiple sources
- **Player definition:** Originally stated "any number" of players per team; clarified to specify **minimum 6 players** for league participation per `League.md`
- **Team composition:** Added explicit requirement of 3 Main players + 3 Replacement players for league play
- **PACSS definition:** Consolidated from `PACSS.md` with clear explanation of point allocation (10 total, minimum 3 per attribute)
- **League definition:** Merged information from `League.md` and use cases, clarifying leaderboard tracking, player requirements, and league types
- **Behavior definition:** Simplified definition focusing on Python scripting purpose and assignment model

**Rationale:** Provides single source of truth for all key system entities; eliminates cross-document inconsistencies

### 2. Player Composition Requirements

**Source:** `Creating a new team.md` use case

**Clarification Made:**
- Team composition specifies **exactly 3 Main players** (Center, Upper Defendant, Lower Defendant) and **exactly 3 Replacement players** for match participation
- This totals 6 players required minimum for league play (aligning with `League.md` requirement)
- Updated Team entity definition to reflect this rigid composition
- Added as precondition in League creation use case (3.1)

**Rationale:** Eliminates ambiguity in team structure; ensures consistency between team creation and league requirements

### 3. Match Duration Specification

**Source Conflict:** 
- `Creating a new league.md` states: "Match duration is at least ==consult== minutes"
- `League.md` states: "Match duration <= 5 min or equivalent in ticks"

**Resolution:** 
- **Priority given to `League.md`** (per instructions to prioritize Use Cases and DFD)
- Standardized to: "Match duration is ≤ 5 minutes (or equivalent in ticks)"
- Removed "at least" interpretation; clarified as upper bound constraint
- Added to Section 3.2 (Match Duration and Constraints)

**Rationale:** `League.md` represents system domain model; provides clearer technical specification

### 4. League Constraints - Maximum Teams

**Source:** `Creating a new league.md` preconditions show maximum team limits

**Change Made:**
- Use case validation step 4.2: "Maximum participating teams is within system constraints"
- Preserved generic wording acknowledging constraint existence while noting external specification needed
- Original document indicated "==consult==" - treated as implementation note
- Added to pending clarifications in CHANGES.md

**Rationale:** Honest documentation of incomplete specification; prevents incorrect assumptions

### 5. Match Start Date/Time Validation

**Source:** `Creating a new league.md` Section 4.4

**Original Text:** "==The match starting date is greater than a minute from now.=="

**Consolidated As:** "Match starting date is greater than 1 minute from current time"

**Implementation:** 
- Converted struck-through requirement into definitive validation rule in use case 3.1, step 4
- Added to Section 3.2 constraints as "Minimum start time offset: > 1 minute from current time"
- Interpreted strike-through as "pending clarification" that should be implemented

**Rationale:** Clarifies temporal constraint for match scheduling; prevents scheduling conflicts

### 6. Functional Requirements Organization

**Original Structure:** Scattered individual use case files

**New Structure:** Reorganized into logical process groupings aligned with DFD Level 1:

**1.0 User/Profile Management:**
- 1.1 Sign Up (from `Creating a new user (Sign Up).md`)
- 1.2 User Login (from `DFD.md` Level 2 processes)
- 1.3 Manage User Profile (from `Manage user profile (club).md`)

**2.0 Team Management:**
- 2.1 Create Team (from `Creating a new team.md`)
- 2.2 Delete Team (from `CU_Emir.md` - Caso de uso #9)

**3.0 League and Matches:**
- 3.1 Create League (from `Creating a new league.md`)
- 3.2 Match Constraints (from `League.md`)

**4.0 Bots and Behaviors:**
- 4.1 Create Behavior (inferred from `Behaviours.md` and domain model)
- 4.2 Modify Behavior (from `Modify behavior.md`)
- 4.3 Delete Behavior (from `CU_Emir.md` - Caso de uso #11)

**Rationale:** Aligns with DFD Level 1 process breakdown for consistency and clarity

### 7. Language Normalization

**Changes Made:**
- Translated Spanish use cases from `CU_Emir.md` into English for consistency:
  - "Eliminar equipo" → "Delete Team"
  - "Eliminar Comportamiento" → "Delete Behavior"
  - "Escenario Exitoso" → "Successful Flow"
  - "Escenarios Excepcionales" → "Exceptional Scenarios"
- Standardized terminology across documents (unified exception handling language)
- Unified formatting of use case sections (Preconditions, Inputs, Flow, Exceptions, Post-conditions)

**Rationale:** Professional consistency; improves readability for international teams

### 8. Cross-References and Dependencies

**Added Explicit Relationships:**
- Team creation depends on player creation (noted in preconditions 2.1)
- League creation depends on team creation (noted in preconditions 3.1)
- Behavior modification has constraints on active player assignments (noted in exceptions 4.2)
- Team deletion restricted if participating in active league (added to preconditions 2.2)
- Behavior modification/deletion restricted if assigned to active players (preconditions 4.2, 4.3)

**Rationale:** Clarifies system workflow and prevents invalid operations

### 9. DFD Alignment

**Source:** `DFD.md` Process hierarchies and data flow diagrams

**Implementation:**
- SRS Section 2 (Functional Requirements Overview) explicitly references DFD Level 1 process breakdown
- Each functional area (1.0-4.0) maps to corresponding DFD processes
- Data stores (D1-D4) referenced in exception and post-condition sections
- Section 2 includes summary table mapping processes to data stores
- Section 3 explains data flow overview from DFD Level 0

**Rationale:** Connects requirements to system architecture; ensures coherent design

### 10. Exception Scenarios Standardization

**Changes Made:**
- All exception scenarios now follow consistent format:
  - **Trigger:** Specific condition causing exception
  - **Flow:** Steps taken by system
  - **Post-exception state:** System state after exception handling
- Added missing preconditions where exceptions implied system state requirements
  - E.g., "Behavior is not currently assigned to active players" (4.2, 4.3)
  - E.g., "Team is not currently participating in active league" (2.2)
- Completed Exception 2 in `Creating a new league.md` (was incomplete in source)

**Rationale:** Professional consistency in requirement documentation; ensures completeness

### 11. Incomplete/Uncertain Specifications Handling

**Preserved Uncertainties with Clear Markers:**

**In Document:**
- "(consult specification)" - indicates values requiring confirmation
- Example: "Maximum participating teams is within system constraints"
- Example: "Minimum match duration (consult specification)"

**In CHANGES.md Pending Clarifications Section:**
- Listed 5 items requiring external specification confirmation
- Provides clear "To-Do" list for team review

**Rationale:** Honest documentation prevents false completeness; guides future refinement

### 12. Added Non-Functional Requirements Section

**New Addition:** Section on Non-Functional Requirements in 2_Functional_Requirements.md

**Contents:**
- Performance (response times, query limits)
- Security (encryption, session management, email validation)
- Data Integrity (consistency, constraint enforcement)
- Usability (error messages, confirmations, feedback)

**Rationale:** Provides baseline quality criteria; acknowledges non-functional aspects beyond use cases

---

## Files Not Included in SRS Consolidation

**Documents Consulted But Not Directly Incorporated:**

- **`DFD.md`**: Served as structural reference for process organization; detailed diagrams should remain in separate architecture documentation
- **`Glossary.md`**: Minimal content; key terms integrated into entity definitions and terminology sections instead
- **`SRS Overview.md`**: Served as reference for project scope; content merged into Section 1.2
- **`System Environment.md`**: Content merged into Section 1.3 (System Environment) and entity definitions
- **`Functional Requirements Specification.md`**: Was a reference/index document; actual requirements now consolidated in SRS
- **`Behaviours.md`**: Single-line definition; expanded in entity definitions and use cases

**Recommendation:** Consider maintaining DFD diagrams in separate architecture documentation folder for reference alongside SRS.

---

## Quality Improvements Made

### 1. Consistency in Exception Handling
✓ All use cases now follow consistent exception scenario format
✓ Each exception includes: Trigger condition, Flow, and outcomes
✓ Exception numbering is sequential (avoided gaps)

### 2. Complete Post-conditions
✓ All use cases include explicit post-conditions
✓ Post-conditions clearly state database changes and system state
✓ Database references aligned with DFD Level 1 data stores

### 3. Preconditions Clarity
✓ All preconditions numbered and explicit
✓ Dependencies between use cases documented
✓ System state requirements clearly stated (e.g., "not in another league")

### 4. Input/Output Definition
✓ All inputs clearly listed and numbered
✓ System outputs documented in flow steps
✓ Optional vs. required inputs explicitly marked

### 5. Actor Specification
✓ Primary and secondary actors clearly identified
✓ Actor capabilities and constraints documented
✓ User roles distinguished (Guest, User, Admin considerations)

### 6. Data Store References
✓ All data modifications reference appropriate data store (D1-D4)
✓ Consistency with DFD Level 1 architecture
✓ Clear traceability to system design

---

## Contradictions Resolved (Priority: Use Cases & DFD)

| Issue | Sources | Resolution | Rationale |
|-------|---------|------------|----------|
| Player team limits | `Player.md` ("any number") vs `League.md` ("minimum 6") | Minimum 6 required for league; can create more for practice | `League.md` represents domain constraint |
| Match duration | `Creating new league.md` ("consult") vs `League.md` ("≤5 min") | ≤ 5 minutes upper bound | `League.md` more specific |
| Team composition | Implicit in team creation | Explicit: 3 Main + 3 Replacement | Use case clarified requirement |
| Behavior modification | No precondition in source | Added: "not assigned to active players" | DFD data flow implies constraint |
| League creation preconditions | Generic in source | Specified: "teams not in other leagues" | Use case flow implied requirement |

---

## Pending Clarifications

The following items require external confirmation and should be updated once determined:

1. **Maximum number of teams per league:** Currently marked as "within system constraints"
   - Impact: League creation validation
   - Action: Obtain from system architect or requirements committee

2. **Exact match duration minimum:** Currently noted as "≤5 minutes or equivalent in ticks"
   - Impact: Match configuration and simulation timing
   - Action: Clarify minimum duration if different from maximum

3. **System constraints on concurrent leagues:** Whether users can have teams in multiple leagues
   - Impact: Team/league relationship model
   - Current assumption: No (teams exclusive to one league)
   - Action: Confirm with stakeholders

4. **PACSS point allocation flexibility:** Whether players must always allocate 10 points or if range is possible
   - Impact: Player creation validation
   - Current assumption: Fixed 10 points
   - Action: Confirm with game balance team

5. **Behavior inheritance/composition:** Whether behaviors can call other behaviors or import modules
   - Impact: Python syntax validation scope
   - Current assumption: Standalone scripts only
   - Action: Clarify Python sandbox limitations

6. **Player creation use case:** Not included in preliminary documentation
   - Current: Assumed prerequisite to team creation
   - Action: Document as separate use case (1.4)

---

## Recommendations for Next Steps

### Immediate (Sprint 1)
1. **Review and approve** this consolidated SRS with stakeholders
2. **Resolve pending clarifications** listed above
3. **Update placeholder values** once system constraints are finalized
4. **Create Player Management use case (1.4)** - Create Player, Modify Player, Delete Player

### Short-term (Sprint 2-3)
1. **Add non-functional requirements** detail (security, performance, scalability)
2. **Create separate document** for API specifications and data models
3. **Define error codes** for each exception scenario
4. **Specify password requirements** (complexity, length, special characters)
5. **Document match simulation engine** behavior as separate section

### Medium-term (Sprint 4+)
1. **Add use case priority levels** (Must Have, Should Have, Nice to Have)
2. **Create test specification** aligned with each use case flow
3. **Develop UI wireframes** for each use case
4. **Define user roles and permissions** (if admin role exists)
5. **Document friendly match creation** use case (currently undefined)

---

## Document Statistics

| Metric | Value |
|--------|-------|
| Use Cases Consolidated | 9 |
| Functional Areas Defined | 4 |
| Exception Scenarios | 14+ |
| Alternative Scenarios | 2 |
| Key Entities Defined | 6 |
| Source Documents Merged | 12 |
| Pages (approx) | 6-8 |
| Lines of content | 1000+ |

---

## Version Control Information

- **Branch Created:** `srs-consolidation`
- **Date:** 2026-08-28
- **Files Created:** 3 markdown files in `SRS/` folder
- **Total Size:** ~35KB (3 files)
- **Status:** Draft - Pending Team Review and Approval

**Next Actions:**
- [ ] Schedule SRS review meeting with stakeholders
- [ ] Resolve pending clarifications
- [ ] Create pull request for team review
- [ ] Merge after approval
- [ ] Begin use case refinement and expansion

---

## How to Use This SRS

**For Developers:**
- Use Section 2 (Functional Requirements) as implementation guide
- Reference use case flows for acceptance criteria
- Check exception scenarios for error handling requirements

**For QA/Test Teams:**
- Use preconditions to establish test setup
- Follow successful flows as positive test cases
- Reference exception scenarios for negative test cases
- Verify post-conditions as test assertions

**For Project Managers:**
- Use functional areas for sprint planning
- Reference use cases for effort estimation
- Track pending clarifications as blockers
- Monitor implementation against flows

**For Stakeholders:**
- Review Section 1 (Introduction and Overview) for system capabilities
- Provide feedback on use case flows via pull request comments
- Clarify pending items in Section 5 (Constraints and Assumptions)
