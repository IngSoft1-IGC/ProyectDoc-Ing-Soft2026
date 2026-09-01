@startuml
rectangle User
circle Leagues
database Matches

User --> Leagues : teams, matches
Leagues --> Matches : other teams positioning
Matches --> Leagues : other teams matches 
Leagues --> User : ranking





@enduml