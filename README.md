# Identity & Access Management (IAM) System

A user management system with login, JWT sessions, and role-based access control, built with Node.js and MySQL. I wrote the authentication flow and access control by hand instead of using an auth service like Auth0 or Firebase Auth, so every step is visible in the code.

## Overview

The app handles registration, login, and user management. Passwords are hashed with bcrypt, and sessions are stateless JWTs. There are two roles, user and admin, and Express middleware checks the role before letting a request through to a protected endpoint.

There's no full-stack framework. The frontend is vanilla JavaScript with Tailwind, and the backend is plain Express, so I control the whole request/response cycle and state handling myself.

## Features

- Password hashing with bcrypt and stateless sessions with JWT
- User and admin roles, enforced by custom Express middleware on each protected endpoint
- A REST API for registration, login, and user management, with proper HTTP status codes
- A MySQL connection pool instead of opening a new connection per query
- A client-side rendered UI in Tailwind and vanilla JavaScript that talks to the API with async requests

## Stack

**Backend**

![Node.js](https://img.shields.io/badge/-Node.js-339933?style=flat&logo=node.js&logoColor=white)
![Express](https://img.shields.io/badge/-Express-000000?style=flat&logo=express&logoColor=white)
![MySQL](https://img.shields.io/badge/-MySQL-4479A1?style=flat&logo=mysql&logoColor=white)
![JWT](https://img.shields.io/badge/-JWT-000000?style=flat&logo=jsonwebtokens&logoColor=white)

**Frontend**

![JavaScript](https://img.shields.io/badge/-JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)
![Tailwind CSS](https://img.shields.io/badge/-Tailwind_CSS-06B6D4?style=flat&logo=tailwindcss&logoColor=white)
![HTML5](https://img.shields.io/badge/-HTML5-E34F26?style=flat&logo=html5&logoColor=white)

## Architecture

The code follows a loose MVC pattern in three layers:

1. **Routes and controllers** take the HTTP request, validate the input, and dispatch it.
2. **Middleware** checks the JWT (`auth.js`) and the user's role.
3. **Data access** runs SQL directly through the `mysql2` driver, with no ORM in between.

## Getting started

You need Node.js 16 or later and a MySQL server.

Clone the repo and install dependencies:

```bash
git clone https://github.com/yourusername/iam-system-core.git
cd iam-system-core
npm install
```

Create the database and tables. The script creates a database called `train_booking`; change the config if you want a different name.

```bash
# Run the initialization script
node config/db-init.js
```

You can also run the SQL in `config/db-init.js` by hand in your MySQL client.

Create a `.env` file based on `.env.example`:

```env
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=your_password
DB_NAME=train_booking
JWT_SECRET=your_secure_secret_key
PORT=3000
```

Start the app:

```bash
# Development mode with auto-reload
npm run dev

# Production start
npm start
```

Then open `http://localhost:3000`.

## API endpoints

| Method | Endpoint | What it does | Access |
|---|---|---|---|
| `POST` | `/api/auth/register` | Create an account | Public |
| `POST` | `/api/auth/login` | Log in and get a JWT | Public |
| `GET` | `/api/auth/me` | Get the current user | Logged in |
| `GET` | `/api/users` | List all users | Admin |
| `DELETE` | `/api/users/:id` | Delete a user | Admin |
