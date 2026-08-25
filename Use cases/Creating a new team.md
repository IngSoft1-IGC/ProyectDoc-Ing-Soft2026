## Diagram

```plantuml
:User: -(Create Team)
```
### Actor
* User
## Brief Description

The user creates a new team using their created players to prepare for matches.
## Preconditions

The user is logged in.
The user has at least 6 available players created in their account.
### Inputs
1. Team name
2. 3 Main players (Center, upper defendant, lower defendant).
3. 3 Replacement players.
## Successful Scenarios

1. The user clicks the "Create new team" button.
2. The system displays a Team Creation form, showing an input field for the Team name, 3 drop-down menus for the main players, and 3 drop-down menus for the Replacement players.
3. The user fills out the form and clicks the "submit" button.
4. The system validates the inputs and verifies that the players are not assigned to another team.
5. The system creates the team and adds the entry in the database.
6. The system displays a confirmation message and returns to the previous page.
## Exceptional Scenarios

#### Exception 1: Missing or invalid field(s)
* Trigger: Step 4 of the Main Flow (input validation fails)
* Flow: The system highlights the missing or invalid fields with error messages and prompts the user to correct them and try again.
#### Exception 3: A player(s) is/are already assigned to a team.
* Trigger: Step 4 of the Main Flow (player verification fails)
* Flow: The system displays an error message indicating a list of players that already are in a team, and prompts the user to either try with another player or remove that player from the team.
#### Post-conditions
1. A new team is registered in the database with the user assigned as the owner.
2. The selected players are marked as assigned to the new team.
3. The team is ready to play a match.