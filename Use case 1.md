# Creating a new User (Sign Up)

### Diagram

![[Pasted image 20260820142530.png|432]]
### Brief Description

The user accesses the website for the first time and wants to create a New User in order to play the game

### Preconditions
The user has to have valid credentials, namely
	1. Username
	2. Email
	3. Password
	4. Name
	5. Avatar
Email must be unique, this means, there mustn't be an existing user that shares the same email.

### Successful Scenarios
1. The user fills all the input fields with its credentials and clicks submit
2. The system responds with a success message and displays a button that redirects to the Login Screen
### Exceptional Scenarios
#### Exception 1: The email is already in use
1. The user fills all the input fields with its credentials and clicks submit.
2. The system respond with failure screen because the email already has a User assigned to it and a button that redirects to the Login Screen.