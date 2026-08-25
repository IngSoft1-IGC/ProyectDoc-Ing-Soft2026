### Diagram

````plantuml
:Guest: - (Sign up)
`````

### Actor
* Guest
### Brief Description
The user accesses the website for the first time and wants to create a New User in order to play the game
### Preconditions
1. The user is currently not logged in.
### Inputs
1. Username
2. Email
3. Password
4. Name
5. Avatar
### Successful Scenarios
1. The user selects the option to Sign up
2. System displays the Sign-up form, requiring Username, Email, Password, Name and Avatar.
3. User enters required details and clicks submit.
4. System validates the inputs and verifies that the email is unique.
5. System creates the new user account in the system database.
6. System displays a confirmation message and provides a link/button to the Login Screen.
### Exceptional Scenarios
#### Exception 1: The email is already in use
* Trigger: Step 4 of Main Flow (email validation fails)
* Flow: System displays an error message indicating the email is already in use, and prompting the user to log in or to use a different email.
#### Exception 2: Missing input fields
* Trigger: Step 4 of Main Flow (input validation fails)
* Flow: System displays an error message indicating the missing input fields, and prompts the user to fill out all the fields.
### Post-conditions:
1. A new account is active in the system database.
2. The user is ready to log in into the application.