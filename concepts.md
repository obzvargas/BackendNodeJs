---
title: "Concepts Explanation Summary"
description: "Explaining all concepts of in BackendNodeJs summary"
icon: "alarm-plus"
---

### 1. Node.js

**Simple definition:**

> Node.js lets you run JavaScript on a computer/server instead of only inside a web browser.

Normally, JavaScript runs here:

```text
Browser
   ↓
JavaScript
```

With Node.js:

```text
Client / Browser
       ↓
   Node.js Server
       ↓
    Database
```

### Why do we use Node.js?

Imagine you build a website where users can register.

The browser sends:

```text
"I want to create an account"
```

Node.js receives the request, processes it, possibly saves the user in a database, and sends back:

```text
"Account created successfully"
```

Node.js is designed to handle many operations efficiently using an **event-driven, non-blocking** approach. 

### Example

```text
console.log("Hello from Node.js");
```

Save it as:

```text
app.js
```

Then run:

```text
node app.js
```

You get:

```text
Hello from Node.js
```

So the key idea is:

> **Node.js = JavaScript running outside the browser, commonly used to build backend/server applications.**

---

### 2. NPM

**NPM = Node Package Manager**

NPM helps you **install and manage packages** that your Node.js project needs. 

For example, instead of building a web server completely yourself, you can install **Express**:

```text
npm install express
```

Think of NPM like a **toolbox/store**:

```text
NPM
 ├── Express
 ├── Mongoose
 ├── JWT
 ├── Nodemailer
 └── Many other packages
```

---

### 3. Dependency

A **dependency** is a package that your project depends on.

For example:

```text
npm install express
```

Now your project depends on Express.

You can see dependencies inside:

```text
package.json
```

Example:

```text
{
  "dependencies": {
    "express": "^5.1.0"
  }
}
```

Simple way to remember:

> **NPM = manages packages**<br /><br />**Dependency = package your project needs**

---

### 4. Backend

The **backend** is the part of an application that works behind the scenes.

For example, when you log in:

```text
You enter:
Email + Password
       ↓
    Frontend
       ↓
    Backend
       ↓
   Database
       ↓
    Backend
       ↓
    Frontend
       ↓
"Login successful"
```

The backend is responsible for things like:

- Processing requests<br />
- Working with databases<br />
- Authentication<br />
- Business logic<br />
- Sending data to the frontend

---

### 5. Route

A **route** tells the backend:

> "When someone sends this type of request to this URL, execute this code."

Example:

```text
app.get("/students", (req, res) => {
    res.send("All students");
});
```

This means:

```text
GET /students
      ↓
Run this function
      ↓
"All students"
```

Another example:

```text
app.post("/students", (req, res) => {
    res.send("Student created");
});
```

So:

```text
GET  /students → Get students
POST /students → Create student
```

Routes connect **URLs and HTTP requests to specific backend logic**. 

# 6. Class

A **class** is a blueprint for creating objects.

Think about a school.

You might have a blueprint for a student:

```text
Student
 ├── name
 ├── age
 └── course
```

In JavaScript:

```text
class Student {
    constructor(name, age, course) {
        this.name = name;
        this.age = age;
        this.course = course;
    }
}
```

Now we can create students:

```text
const student1 = new Student("Obed", 24, "Computer Science");
const student2 = new Student("John", 22, "Software Engineering");
```

The **class** is the blueprint.

The actual students are **objects**.

Your material describes a class as a blueprint used to create objects with shared properties and methods. 

---

# 7. Object

An **object** is a collection of related information.

Example:

```text
const student = {
    name: "Obed",
    age: 24,
    course: "Computer Science"
};
```

Think:

```text
student
   │
   ├── name → Obed
   ├── age → 24
   └── course → Computer Science
```

The things inside the object are called **properties**.

---

# 8. Properties

A **property** describes something about an object.

For example:

```text
const student = {
    name: "Obed",
    age: 24
};
```

Here:

```text
name → property
age  → property
```

Their values are:

```text
name → "Obed"
age  → 24
```

You can access them:

```text
console.log(student.name);
```

Output:

```text
Obed
```

---

# 9. Method

A **method** is a function that belongs to an object.

Example:

```text
const student = {
    name: "Obed",

    greet() {
        console.log("Hello!");
    }
};

student.greet();
```

Here:

```text
greet()
```

is a method.

Easy rule:

```text
Property → describes something

Method → performs an action
```

For example:

```text
const user = {
    name: "Obed",

    login() {
        console.log("User logged in");
    }
};
```

`name` → property<br /><br />`login()` → method

---

# 10. Express.js

Now we get into the **important Node.js backend part**.

**Express.js** is a framework built for Node.js that makes it much easier to create web servers and APIs. 

Without Express, creating a server with Node.js involves more low-level code.

With Express:

```text
const express = require("express");

const app = express();

app.get("/", (req, res) => {
    res.send("Hello World");
});

app.listen(5000);
```

Now you have a server.

You can think of it like:

```text
Node.js
   ↓
Express.js
   ↓
Backend Server
   ↓
Routes
   ↓
API
```

### Important distinction

> **Node.js is the runtime.**

> **Express.js is a framework that runs on Node.js.**

---

# 11. Postman

**Postman** is a tool used to **test APIs**. 

Suppose your backend has:

```text
GET /students
```

Instead of building a frontend just to test it, you can use Postman.

You send:

```text
GET http://localhost:5000/students
```

And your server might respond:

```text
[
    {
        "name": "Obed",
        "age": 24
    },
    {
        "name": "John",
        "age": 22
    }
]
```

So:

> **Postman = a tool for sending requests to your API and checking the responses.**

---

# 12. Nodemon

Normally, when you change your Node.js code, you have to restart the server:

```text
node server.js
```

Then change code → stop server → run again.

**Nodemon** does this automatically. 

Instead of:

```text
node server.js
```

you can use:

```text
nodemon server.js
```

Now:

```text
Change code
    ↓
Nodemon detects change
    ↓
Restart server automatically
```

Very useful during development.

---

# 13. API

This is one of the **most important concepts**.

**API = Application Programming Interface.**

An API allows different applications to communicate with each other. 

For example:

```text
React Frontend
      ↓
      API
      ↓
Node.js Backend
      ↓
   Database
```

The frontend doesn't normally communicate directly with the database.

Instead:

```text
Frontend → API → Backend → Database
```

---

# 🍔 Easy API Example

Think about a restaurant.

```text
Customer
   ↓
Waiter
   ↓
Kitchen
```

The customer doesn't walk into the kitchen and cook the food.

The **waiter** communicates between the customer and kitchen.

In an API:

```text
Customer      = User
Waiter        = API
Kitchen       = Backend
Food          = Data
```

Your material uses essentially this restaurant analogy for explaining APIs. 

---

# 14. API Request & Response

APIs work mainly through a **request → response** process. 

Example:

```text
Frontend
   │
   │ GET /students
   ↓
Backend
   │
   │ Find students
   ↓
Database
   │
   │ Students
   ↓
Backend
   │
   │ JSON response
   ↓
Frontend
```

### Request

The client asks:

```text
GET /students
```

### Response

The server sends:

```text
[
    {
        "name": "Obed"
    },
    {
        "name": "John"
    }
]
```

---

# 15. API Endpoint

An **endpoint** is a specific URL where you can access a resource. 

For example:

```text
GET /students
```

The endpoint is:

```text
/students
```

You could have:

```text
GET    /students
GET    /students/10
POST   /students
PUT    /students/10
DELETE /students/10
```

Each endpoint performs a particular operation.

---

# 16. HTTP Methods

HTTP methods tell the server **what you want to do**.

The most important ones are:

| Method | Meaning |
| :-- | :-- |
| `GET` | Get/read data |
| `POST` | Create data |
| `PUT` | Update data |
| `PATCH` | Partially update data |
| `DELETE` | Delete data |

For example:

```text
GET /students
```

Means:

> Give me students.

```text
POST /students
```

Means:

> Create a new student.

```text
DELETE /students/5
```

Means:

> Delete student 5.

The material explains that HTTP methods represent operations such as retrieving, creating, updating, and deleting resources. 

---

# 17. API Request Components

A request can contain several important things.

### Endpoint

```text
/students
```

### Method

```text
GET
```

### Headers

Extra information about the request.

Example:

```text
Authorization: Bearer token
Content-Type: application/json
```

### Body

The actual data you're sending.

For example, when creating a student:

```text
{
    "name": "Obed",
    "age": 24,
    "course": "Computer Science"
}
```

### Parameters

Additional information used by the server.

For example:

```text
/students/24
```

or:

```text
/students?course=computer-science
```

Your material identifies endpoint, method, parameters, headers, and body as common parts of an API request. 

---

# 18. API Response

After the server receives a request, it sends a **response**.

A response normally contains:

```text
Status Code
Headers
Body
```

For example:

```text
HTTP/1.1 200 OK
```

and:

```text
{
    "message": "Students retrieved successfully"
}
```

---

# 19. HTTP Status Codes

These are **very important for backend development**.

They tell us what happened with a request.

## 🟢 2xx — Success

### 200 OK

Everything worked.

```text
GET /students
→ 200 OK
```

### 201 Created

Something was successfully created.

```text
POST /students
→ 201 Created
```

### 204 No Content

The request worked, but there's nothing to return.

Often used for successful deletes.

---

# 🔵 3xx — Redirection

The resource has moved or the client needs to take another step.

Examples:

```text
301 → Permanently moved
302 → Temporarily moved
304 → Not modified
```

For normal API development, you will encounter **2xx, 4xx and 5xx** much more often.

---

# 🔴 4xx — Client Error

This usually means **something is wrong with the request from the client**.

### 400 Bad Request

The request is invalid.

```text
400
```

Example:

```text
{
    "message": "Email is required"
}
```

### 401 Unauthorized

The user needs valid authentication.

```text
401
```

Think:

> "You haven't proved who you are."

### 403 Forbidden

The server knows who you are, but you **don't have permission**.

```text
403
```

Think:

> "I know who you are, but you're not allowed."

### 404 Not Found

The requested resource doesn't exist.

```text
404
```

Example:

```text
GET /students/9999
```

but student 9999 doesn't exist.

### 409 Conflict

The request conflicts with existing data.

Example:

```text
Register user with email that already exists
→ 409 Conflict
```

### 422 Unprocessable Entity

The server understands the request but cannot process it, commonly because of validation problems.

### 429 Too Many Requests

The client is sending too many requests.

---

# 🔥 The 4 Status Codes You Should Memorize First

For your Node.js/Express work, remember these first:

```text
200 → Success
201 → Created
400 → Bad request
401 → Not authenticated
403 → Not allowed
404 → Not found
500 → Server error
```

---

# 🔴 20. 5xx — Server Error

These mean something went wrong **on the server**.

### 500 Internal Server Error

Something unexpected happened in your backend.

```text
500
```

Example:

```text
Database crashed
Unexpected code error
Server exception
```

### 502 Bad Gateway

A server acting as a gateway/proxy received an invalid response.

### 503 Service Unavailable

The server is currently unavailable.

### 504 Gateway Timeout

A server didn't receive a response from another server in time.

---

# 21. DBMS

**DBMS = Database Management System.**

It is software used to manage databases. 

Examples:

```text
MySQL
PostgreSQL
MongoDB
Microsoft SQL Server
Oracle
Redis
```

There are two major categories discussed in your material:

```text
Database
   │
   ├── SQL
   │     ├── MySQL
   │     └── PostgreSQL
   │
   └── NoSQL
         ├── MongoDB
         └── Redis
```

---

# 22. SQL Database

SQL databases organize data into **tables**.

For example:

```text
STUDENTS

┌────┬────────┬─────┐
│ ID │ Name   │ Age │
├────┼────────┼─────┤
│ 1  │ Obed   │ 24  │
│ 2  │ John   │ 22  │
└────┴────────┴─────┘
```

Examples:

- MySQL<br />
- PostgreSQL<br />
- SQL Server<br />
- Oracle<br />

SQL databases generally use predefined schemas and tables consisting of rows and columns. 

---

# 23. NoSQL Database

NoSQL databases don't necessarily store data in traditional tables.

MongoDB, for example, uses **documents**.

Example:

```text
{
    "name": "Obed",
    "age": 24,
    "course": "Computer Science"
}
```

Another student could have different fields:

```text
{
    "name": "John",
    "age": 22,
    "phone": "078..."
}
```

This makes NoSQL databases more flexible for certain applications.

Examples from your material include:

- MongoDB<br />
- Redis<br />
- Cassandra<br />

---

# 🧠 The Big Picture

Now connect everything we've learned:

```text
                    CLIENT
                 React / Mobile
                       │
                       │ HTTP Request
                       ↓
                    EXPRESS
                       │
                       ↓
                    ROUTE
                       │
                       ↓
                BACKEND LOGIC
                       │
                       ↓
                    DATABASE
                  /          \
               SQL          NoSQL
             MySQL        MongoDB
                  \          /
                       ↓
                    RESPONSE
                       │
                       ↓
                     CLIENT
```

And the tools around it:

```text
Node.js
   ↓
Runs JavaScript on the server

Express.js
   ↓
Helps build the backend/API

NPM
   ↓
Installs packages

Nodemon
   ↓
Automatically restarts server

Postman
   ↓
Tests APIs

Database
   ↓
Stores application data
```

### ⭐ The most important concepts to master

If you're learning **Node.js + Express backend development**, focus especially on this order:

```text
1. Node.js
      ↓
2. NPM & Dependencies
      ↓
3. Express.js
      ↓
4. Routes
      ↓
5. HTTP Methods
      ↓
6. Request & Response
      ↓
7. REST APIs
      ↓
8. Status Codes
      ↓
9. Middleware
      ↓
10. Database
      ↓
11. Authentication
      ↓
12. CRUD APIs
```

This material covers the foundation through **APIs, HTTP responses/status codes, and SQL/NoSQL databases**; topics such as middleware and authentication are not covered in the supplied section, so I haven't attributed those to the document. 
