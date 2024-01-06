# Проєктування бази даних

## Модель бізнес-об'єктів

@startuml

entity User

entity User.nickname
entity User.email
entity User.photo
entity User.password
entity User.isBanned

User.nickname -d-* User
User.email -u-* User
User.photo -l-* User
User.password -d-* User
User.isBanned -u-* User

entity Collaborator
Collaborator "0,*" -r- "1,1" User

entity Role
Collaborator "0,*" -d- "1,1" Role

entity Grant
Grant "0,*" -u- "1,1" Role

entity Permission
Permission "1,1" -r- "0,*" Grant

entity Team
Collaborator "0,*" -l- "1,1" Team

entity Project
Project "1,1" -r- "0,*" Team

entity Project.name
entity Project.description

Project.name -r-* Project
Project.description -u-* Project

entity Task
entity Assignment

Task "1,1" -d- "0,*" Assignment
Collaborator "1,1" -u- "0,*" Assignment

entity Task.name
entity Task.description
entity Task.deadline
entity Tag
entity Label
entity Task.comment

Task.name -d-* Task
Task.description -d-* Task
Task.deadline -d-* Task

Label "0,*" -l- "1,1" Task
Tag "1,1" -l- "0,*" Label

Task.comment "0,*" -l- "1,1" Task
Task.comment "0,*" - "1,1" Collaborator :author

entity Sprint

Sprint "1,1" -r- "0,*" Task
Sprint "0,*" -d- "1,1" Project


entity Sprint.name
entity Sprint.goal
entity Sprint.startdate
entity Sprint.enddate

Sprint.name -d-* Sprint
Sprint.goal -d-* Sprint
Sprint.startdate -d-* Sprint
Sprint.enddate -d-* Sprint

entity Action
entity Action.date

Action.date -r-* Action

Action "0,*" -r- "0,1" Assignment
Action "0,*" -d- "0,1" Collaborator
Action "0,*" -u- "0,1" Sprint
Action "0,*" -u- "0,1" Task


@enduml

## ER-модель

@startuml

skinparam entity {
BackgroundColor #f7f711
}

package UserAdministration {
entity User {
    id: UUID
    nickname: TEXT
    email: TEXT
    password: TEXT
    photo: IMAGE
    isBanned: TINYINT
}
}

entity Collaborator {
    id: NUMBER
}

entity Action {
    id: NUMBER
    datetime: DATETIME
}

entity Project {
    id: UUID
    name: TEXT
    description: TEXT
}

entity Team {
    id: NUMBER
    name: TEXT
    motto: TEXT
}

package AccessControl {
enum Role <<ENUMERATION>> #f7f711 {
    id: NUMBER
    name: TEXT
    description: TEXT
}

entity Permission {
    id: NUMBER
    action: TEXT
}

entity Grant {
}

object developer
object teamlead
object administrator

developer .u.> Role :instanceOf
teamlead .u.> Role :instanceOf
administrator .u.> Role :instanceOf
}

package TaskManagement {
entity Assignment {
    id: NUMBER
    datetime: DATETIME
}

entity Task {
    id: NUMBER
    name: TEXT
    description: TEXT
    creationDate: DATETIME
    deadline: DATETIME
}

enum Tag {
    id: NUMBER
    name: TEXT
    description: TEXT
}

entity Label {
}

entity Task_comment {
    id: NUMBER
    subject: TEXT
    text: TEXT
    datetime: DATETIME
}

entity Sprint {
    id: NUMBER
    name: TEXT
    goal: TEXT
    startdate: DATE
    enddate: DATE
}
}

Action "0,*" -r-> "0,1" Collaborator
Action "0,*" -u-> "0,1" Sprint
Action "0,*" -u-> "0,1" Task
Action "0,*" -u-> "0,1" Assignment

Collaborator "0,*" -d-> "1,1" User
Team "1,1" -u-> "0,*" Collaborator
Team "0,*" -u-> "1,1" Project
Collaborator "0,*" --> "1,1" Role
Grant "0,*" -r-> "1,1" Role
Grant "0,*" -l-> "1,1" Permission
Assignment "0,*" -d-> "1,1" Collaborator
Assignment "0,*" -u-> "1,1" Task
Label "0,*" -d-> "1,1" Task
Label "0,*" -u-> "1,1" Tag
Task_comment "0,*" -d-> "1,1" Collaborator :author
Task_comment "0,*" -u-> "1,1" Task
Task "0,*" -l-> "1,1" Sprint
Sprint "0,*" -d-> "1,1" Project

@enduml

## Реляційна схема

![Реляційна схема](https://github.com/Yana-Koroliuk/databases-course-work/assets/59470968/9d5df165-067f-4f71-964a-47c2e14893e7)
