TaskBoard Pro

📋 Project Overview

TaskBoard Pro is an advanced task collaboration platform designed to streamline project management workflows. The application enables teams to create projects, manage tasks across customizable status boards, assign responsibilities, and implement powerful automation rules that trigger actions based on task changes.

🚀 Key Features

Project Management
    Create and manage multiple projects with detailed descriptions
    Invite team members via email for seamless collaboration
    Role-based permissions system for project access control


Task Management
   Intuitive Kanban-style board interface for task visualization
   Create tasks with titles, descriptions, due dates, and assignees
   Drag-and-drop functionality for moving tasks between status columns
   Customizable status columns per project (beyond default To Do, In Progress, Done)


Workflow Automation Engine
    Create conditional automation rules that trigger based on task changes
    
Rule examples:
    When task status changes to "Done" → Award badge to assignee
    When task is assigned to specific user → Move to "In Progress"
    When due date passes → Send notification to project members


Multi-step automation chains for complex workflows

  Real-time Collaboration
      WebSocket implementation for instant updates across all connected users
      Real-time notifications for task assignments and status changes
      Activity feed showing recent actions across projects



  🛠️ Tech Stack
      
      Frontend
         React.js with functional components and hooks
         Redux for state management
         Material UI for responsive component design
         React DnD for drag-and-drop functionality
         Socket.io client for real-time updates

      Backend
         Node.js with Express
         MongoDB for database storage
         Mongoose for data modeling
         Socket.io for WebSocket implementation
         JWT for secure API authentication

      📊 Database Schema
                 


 📝 API Documentation
            Projects API
               Get All Projects (for authenticated user)
               GET /api/projects
      Response:
json{
  "success": true,
  "data": [
    {
      "_id": "60d21b4667d0d8992e610c85",
      "title": "Website Redesign",
      "description": "Complete overhaul of company website",
      "owner": {
        "_id": "60d21b4667d0d8992e610c80",
        "name": "John Doe"
      },
      "members": [
        {
          "userId": {
            "_id": "60d21b4667d0d8992e610c81",
            "name": "Jane Smith"
          },
          "role": "editor"
        }
      ],
      "createdAt": "2023-06-22T15:24:22.000Z"
    }
  ]
}
Create Project
POST /api/projects
Request Body:
json{
  "title": "Mobile App Development",
  "description": "Build iOS and Android apps for our product"
}
Response:
json{
  "success": true,
  "data": {
    "_id": "60d21b4667d0d8992e610c86",
    "title": "Mobile App Development",
    "description": "Build iOS and Android apps for our product",
    "owner": "60d21b4667d0d8992e610c80",
    "members": [
      {
        "userId": "60d21b4667d0d8992e610c80",
        "role": "owner",
        "joinedAt": "2023-06-22T15:30:22.000Z"
      }
    ],
    "statuses": [
      {
        "id": "todo",
        "name": "To Do",
        "color": "#E0E0E0",
        "order": 1
      },
      {
        "id": "in-progress",
        "name": "In Progress",
        "color": "#BBDEFB",
        "order": 2
      },
      {
        "id": "done",
        "name": "Done",
        "color": "#C8E6C9",
        "order": 3
      }
    ],
    "createdAt": "2023-06-22T15:30:22.000Z",
    "updatedAt": "2023-06-22T15:30:22.000Z"
  }
}
Add Member to Project
POST /api/projects/:projectId/members
Request Body:
json{
  "email": "team@example.com",
  "role": "editor"
}
Response:
json{
  "success": true,
  "message": "Invitation sent to team@example.com"
}
Tasks API
Get Project Tasks
GET /api/projects/:projectId/tasks
Response:
json{
  "success": true,
  "data": [
    {
      "_id": "60d21b4667d0d8992e610c90",
      "title": "Design homepage mockup",
      "description": "Create responsive design for the new homepage",
      "status": "in-progress",
      "dueDate": "2023-07-05T00:00:00.000Z",
      "assignee": {
        "_id": "60d21b4667d0d8992e610c81",
        "name": "Jane Smith"
      },
      "createdAt": "2023-06-22T16:00:00.000Z"
    }
  ]
}
Create Task
POST /api/projects/:projectId/tasks
Request Body:
json{
  "title": "Implement authentication",
  "description": "Set up user login and registration",
  "status": "todo",
  "dueDate": "2023-07-10T00:00:00.000Z",
  "assignee": "60d21b4667d0d8992e610c82"
}
Response:
json{
  "success": true,
  "data": {
    "_id": "60d21b4667d0d8992e610c91",
    "projectId": "60d21b4667d0d8992e610c86",
    "title": "Implement authentication",
    "description": "Set up user login and registration",
    "status": "todo",
    "dueDate": "2023-07-10T00:00:00.000Z",
    "assignee": "60d21b4667d0d8992e610c82",
    "createdBy": "60d21b4667d0d8992e610c80",
    "createdAt": "2023-06-22T16:15:00.000Z",
    "updatedAt": "2023-06-22T16:15:00.000Z"
  }
}
Update Task Status
PATCH /api/tasks/:taskId
Request Body:
json{
  "status": "in-progress"
}
Response:
json{
  "success": true,
  "data": {
    "_id": "60d21b4667d0d8992e610c91",
    "status": "in-progress",
    "updatedAt": "2023-06-23T10:00:00.000Z"
  }
}
Automations API
Get Project Automations
GET /api/projects/:projectId/automations
Response:
json{
  "success": true,
  "data": [
    {
      "_id": "60d21b4667d0d8992e610d00",
      "name": "Auto-move to In Progress",
      "trigger": {
        "event": "task.assigned",
        "conditions": [
          {
            "field": "assignee",
            "operator": "equals",
            "value": "60d21b4667d0d8992e610c82"
          }
        ]
      },
      "actions": [
        {
          "type": "move",
          "parameters": {
            "status": "in-progress"
          }
        }
      ],
      "isActive": true,
      "createdAt": "2023-06-22T17:00:00.000Z"
    }
  ]
}
Create Automation
POST /api/projects/:projectId/automations
Request Body:
json{
  "name": "Award badge on completion",
  "trigger": {
    "event": "task.status.changed",
    "conditions": [
      {
        "field": "status",
        "operator": "equals",
        "value": "done"
      }
    ]
  },
  "actions": [
    {
      "type": "badge",
      "parameters": {
        "badgeId": "task-master"
      }
    }
  ]
}
Response:
json{
  "success": true,
  "data": {
    "_id": "60d21b4667d0d8992e610d01",
    "projectId": "60d21b4667d0d8992e610c86",
    "name": "Award badge on completion",
    "trigger": {
      "event": "task.status.changed",
      "conditions": [
        {
          "field": "status",
          "operator": "equals",
          "value": "done"
        }
      ]
    },
    "actions": [
      {
        "type": "badge",
        "parameters": {
          "badgeId": "task-master"
        }
      }
    ],
    "createdBy": "60d21b4667d0d8992e610c80",
    "isActive": true,
    "createdAt": "2023-06-22T17:30:00.000Z",
    "updatedAt": "2023-06-22T17:30:00.000Z"
  }
}
🔄 Automation Rules Examples
1. Move task to "In Progress" when assigned
json{
  "name": "Auto-move to In Progress",
  "trigger": {
    "event": "task.assigned",
    "conditions": []
  },
  "actions": [
    {
      "type": "move",
      "parameters": {
        "status": "in-progress"
      }
    }
  ]
}
2. Send notification when due date approaches
json{
  "name": "Due date reminder",
  "trigger": {
    "event": "task.dueDate.approaching",
    "conditions": [
      {
        "field": "daysRemaining",
        "operator": "equals",
        "value": 1
      }
    ]
  },
  "actions": [
    {
      "type": "notify",
      "parameters": {
        "recipients": ["assignee", "projectOwner"],
        "message": "Task '{{taskTitle}}' is due tomorrow!"
      }
    }
  ]
}
3. Award badge on task completion
json{
  "name": "Productivity Star Badge",
  "trigger": {
    "event": "task.status.changed",
    "conditions": [
      {
        "field": "status",
        "operator": "equals",
        "value": "done"
      },
      {
        "field": "dueDate",
        "operator": "afterNow",
        "value": null
      }
    ]
  },
  "actions": [
    {
      "type": "badge",
      "parameters": {
        "badgeId": "productivity-star",
        "recipient": "assignee"
      }
    }
  ]
}
🚀 Installation & Setup
Prerequisites

Node.js (v16+)
MongoDB
npm or yarn

Steps

Clone the repository

bashgit clone https://github.com/your-username/taskboard-pro.git
cd taskboard-pro

Install dependencies

bash# Install server dependencies
cd server
npm install

# Install client dependencies
cd ../client
npm install

Configure environment variables

bash# In server directory, create .env file
cp .env.example .env
# Update .env with your MongoDB connection string and other configurations

Start development servers

bash# Start backend server (from server directory)
npm run dev

# Start frontend server (from client directory)
npm start

Access the application

Frontend: http://localhost:3000
Backend API: http://localhost:5000
📸 Screenshots
Show Image
Project Dashboard with Activity Feed and Task Statistics
Show Image
Kanban Board with Task Cards and Status Columns
Show Image
Intuitive Automation Rule Creation Interface
🧪 Testing
Run tests with:
bash# Run backend tests
cd server
npm test

# Run frontend tests
cd client
npm test
🤝 Contributing

Fork the repository
Create your feature branch (git checkout -b feature/amazing-feature)
Commit your changes (git commit -m 'Add some amazing feature')
Push to the branch (git push origin feature/amazing-feature)
Open a Pull Request

📃 License
This project is licensed under the MIT License - see the LICENSE file for details.
🙏 Acknowledgments

Material UI for the component library
React DnD for drag-and-drop functionality
MongoDB for the flexible document database
