# 📋 TaskBoard Pro



TaskBoard Pro is an advanced task collaboration platform designed to streamline project management workflows. The application enables teams to create projects, manage tasks across customizable status boards, assign responsibilities, and implement powerful automation rules that trigger actions based on task changes.

---

## 🚀 Key Features

### 🔧 Project Management
- Create and manage multiple projects with detailed descriptions  
- Invite team members via email for seamless collaboration  
- Role-based permissions system for project access control  

### ✅ Task Management
- Intuitive **Kanban-style** board interface for task visualization  
- Create tasks with titles, descriptions, due dates, and assignees  
- Drag-and-drop functionality for moving tasks between status columns  
- Customizable status columns per project (beyond default *To Do, In Progress, Done*)  

### ⚙️ Workflow Automation Engine
- Create conditional automation rules triggered by task changes  

**Examples:**
- When task status changes to `"Done"` → Award badge to assignee  
- When task is assigned to a specific user → Move to `"In Progress"`  
- When due date passes → Send notification to project members  
- Support for **multi-step automation chains**  

### 🧑‍🤝‍🧑 Real-time Collaboration
- WebSocket implementation for instant updates  
- Real-time notifications for task assignments and status changes  
- Activity feed showing recent actions across projects  

---

## 🛠️ Tech Stack

### Frontend
- React.js (Functional components + Hooks)  
- Redux for state management  
- Material UI for responsive UI design  
- React DnD for drag-and-drop  
- Socket.io-client for real-time sync  

### Backend
- Node.js with Express  
- MongoDB with Mongoose ORM  
- JWT for secure authentication  
- Socket.io for WebSocket support  

---

## 📊 Database Schema

![image](https://github.com/user-attachments/assets/a1dd7507-54a6-437f-96ec-d6eda5ec0f78)

---

   
### Projects Collection
    { 
    "_id": "ObjectId",
     "title": "String",
     "description": "String",
    "owner": "ObjectId (ref: Users)",
     "members": [
    {
      "userId": "ObjectId (ref: Users)",
      "role": "String (owner, editor, viewer)",
      "joinedAt": "Date"
    }
    ],
    "statuses": [
    {
      "id": "String",
      "name": "String",
      "color": "String",
      "order": "Number"
    }
    ],
    "createdAt": "Date",
    "updatedAt": "Date"
    }

  ### Tasks Collection
      {
     "_id": "ObjectId",
    "projectId": "ObjectId (ref: Projects)",
    "title": "String",
    "description": "String",
    "status": "String",
    "dueDate": "Date",
    "assignee": "ObjectId (ref: Users)",
    "createdBy": "ObjectId (ref: Users)",
    "attachments": [
    {
      "name": "String",
      "url": "String",
      "type": "String",
      "size": "Number",
      "uploadedAt": "Date"
    }
    ],
     "comments": [
    {
      "userId": "ObjectId (ref: Users)",
      "text": "String",
      "createdAt": "Date"
    }
    ],
    "createdAt": "Date",
    "updatedAt": "Date"
     }

### Automations Collection
          {
    "_id": "ObjectId",
    "projectId": "ObjectId (ref: Projects)",
    "name": "String",
    "trigger": {
    "event": "String (task.status.changed, task.assigned, task.dueDate.passed)",
    "conditions": [
      {
        "field": "String",
        "operator": "String (equals, notEquals, contains)",
        "value": "Any"
      }
    ]
    },
    "actions": [
    {
      "type": "String (move, assign, notify, badge)",
      "parameters": {
        "key1": "value1",
        "key2": "value2"
      }
    }
    ],
    "createdBy": "ObjectId (ref: Users)",
    "isActive": "Boolean",
    "createdAt": "Date",
    "updatedAt": "Date"
    }  

### Notifications Collection
        {
    "_id": "ObjectId",
    "userId": "ObjectId (ref: Users)",
    "type": "String",
    "content": "String",
    "relatedTo": {
    "type": "String (project, task)",
    "id": "ObjectId"
    },
    "isRead": "Boolean",
    "createdAt": "Date"
    }

---

## 📝API Documentation

 ### Projects API 
  #### Get All Projects 
       GET /api/projects
 #### Create Project 
     POST /api/projects
 #### Add Member to Project
    POST /api/projects/:projectId/members

### Tasks API
 #### Get Project Tasks
    GET /api/projects/:projectId/tasks
 #### Create Task
     POST /api/projects/:projectId/tasks
 #### Update Task Status
     PATCH /api/tasks/:taskId

### Automations API
 #### Get Project Automations
    GET /api/projects/:projectId/automations
 #### Create Automation
     GET /api/projects/:projectId/automations
   

---

## 🔄Automation Rule Example 

#### Auto-move to "In Progress" on assignment
       {
    name": "Auto-move to In Progress",
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

#### Due Date Reminder
     {
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

#### Award badge on task completion 
      {
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

---

## 🧪Installation & Setup

### Prerequisites
    Node.js (v16+)
    MongoDB
    npm or yarn

### Steps
 #### Clone the repository
        git clone https://github.com/your-username/taskboard-pro.git
         cd taskboard-pro

  #### Install dependencies
     #Backend
        cd server
        npm install
    #Frontend
       cd ../client
       npm install

 ### Configure environment variables
         # In the server directory
          cp .env.example .env
         # Then edit .env with your MongoDB URI and other secrets

### Start development servers
    #Start backend
          cd server
         npm run dev
    #Start frontend
            cd ../client
           npm start


### Access the app
#### Frontend: http://localhost:3000
#### Backend API: http://localhost:5000


---

## 🧪Testing

#### Backend tests
     cd server
     npm test
     
 #### Frontend tests
       cd client
       npm test

---

## 📸Screenshots




---

## 🤝Contributing

#### Fork the repository
#### Create your branch
    git checkout -b feature/amazing-feature
#### Commit your changes
    git commit -m "Add some amazing feature"
#### Push to the branch
    git push origin feature/amazing-feature
#### Open a pull request

---

## 📃License
 #### This project is licensed under the MIT License — see the LICENSE file for details.

---

## 🙏Acknowledgements
#### Material UI — UI components
#### React DnD — Drag and drop
#### MongoDB — Document database











