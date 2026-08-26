### Diagram
```plantuml
:User: -(Profile)
```
### Actor
* User
### Brief description
The user wants to see their profile, in order to inspect its attributes or make changes to its profile.
### Preconditions
1. The user is logged in
### Inputs
1. Name (Optional update)
2. Email (Optional update)
3. Avatar (Optional update) 
4. Old Password (Required only if changing password or sensitive data)
5. New Password (Optional update)
### Successful scenarios
1. The user clicks on their profile avatar on the navigation var.
2. The system displays the Profile page populated with the user's current Name, Username, Email, and Avatar, along with empty fields for changing the password.
3. User updates desired fields (e.g., Name, Email, Avatar, or Password details) and clicks "Save Changes".
4. System validates that the input formats are correct, verifies that the new Email (if changed) is unique, and confirms that `Old Password` matches the current password if a password update is requested.
5. System updates the account details in the database.
6. System displays a success message and reloads the updated Profile page.
### Alternative scenarios
#### Alternative scenario 1: User views profile without making changes
* Trigger: Step 3 of Main Flow (User navigates away or clicks "Cancel").
- Flow: System discards any unsubmitted input changes and redirects the user back to the Main Menu without modifying the database.
### Exceptional scenarios
#### Exception 1: Empty or invalid input field(s)
* Trigger: Step 5 of the Main Flow(input validation fails)
* Flow: The system highlights the missing or invalid fields (e.g., malformed email) and prompts the user to correct them and try again.
#### Exception 2: The email is already in use
* Trigger: Step 5 of Main Flow (email validation fails)
* Flow: System displays an error message indicating the email is already registered to another account.
#### Exception 3: Incorrect old password
* Trigger: Step 5 of the Main Flow(input validation fails)
* Flow: The system highlights the error and prompts the user to correct the password and try again.
### Post-conditions
1. The user's updated profile information is saved in the database.
2. The user interface reflects the updated account information in their profile.