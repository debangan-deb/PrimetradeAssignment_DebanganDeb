# API Documentation - TASK 2

Base URL (local): `{{baseUrl}}` (default: `http://localhost:5000`)

## Authentication
### POST /api/auth/signup
- **Description:** Register a new user.
- **Request headers:** `Content-Type: application/json`
- **Request body example:**
```json
{
  "name": "Test User",
  "email": "test@example.com",
  "password": "Password123"
}
```
- **Success response:** `201 Created`
```json
{
  "message": "User created",
  "user": {
    "_id": "userId",
    "name": "Test User",
    "email": "test@example.com"
  }
}
```
- **Errors:** `400 Bad Request` (validation), `409 Conflict` (email exists)

### POST /api/auth/login
- **Description:** Login and receive JWT token.
- **Request body example:**
```json
{
  "email": "test@example.com",
  "password": "Password123"
}
```
- **Success response:** `200 OK`
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```
- **Errors:** `401 Unauthorized` (bad credentials)

## Profile
### GET /api/profile
- **Auth required:** Yes (Authorization: Bearer &lt;token&gt;)
- **Description:** Get current user's profile.
- **Success response:** `200 OK`
```json
{
  "user": {
    "_id": "userId",
    "name": "Test User",
    "email": "test@example.com",
    "bio": "..."
  }
}
```

### PUT /api/profile
- **Auth required:** Yes
- **Request body example:**
```json
{
  "name": "New Name",
  "bio": "Short bio"
}
```
- **Success response:** `200 OK` with updated user object.

## Tasks (sample entity)
### GET /api/tasks
- **Auth required:** Yes
- **Description:** List tasks belonging to the user. Supports query params `?q=search` for searching or `?completed=true`.
- **Success response:** `200 OK`
```json
[
  {
    "_id": "taskId",
    "title": "Buy groceries",
    "description": "Milk, eggs",
    "completed": false,
    "owner": "userId"
  }
]
```

### POST /api/tasks
- **Auth required:** Yes
- **Request body example:**
```json
{
  "title": "Buy groceries",
  "description": "Milk, eggs, bread",
  "completed": false
}
```
- **Success response:** `201 Created` with created task object.

### GET /api/tasks/:id
- **Auth required:** Yes
- **Description:** Get a specific task by ID.

### PUT /api/tasks/:id
- **Auth required:** Yes
- **Request body:** fields to update (title, description, completed)

### DELETE /api/tasks/:id
- **Auth required:** Yes
- **Description:** Delete the specified task.

## Notes for reviewer / how to use
1. Start the backend server (example):
```bash
cd server
npm install
npm start
# default assumes PORT=5000, MONGO_URI set in .env
```
2. Start the client:
```bash
cd client
npm install
npm run dev
```
3. In Postman, import `postman_collection_task2.json`, set the `baseUrl` variable (e.g., http://localhost:5000), signup a user, login to get the `token`, then set `jwt_token` variable.

## Environment variables (.env)
Create a `.env` file in the server folder with:
```
PORT=5000
MONGO_URI=your_mongo_connection_string
JWT_SECRET=your_jwt_secret
```

## Errors & status codes
- `400 Bad Request` - validation error
- `401 Unauthorized` - missing/invalid token
- `403 Forbidden` - access denied
- `404 Not Found` - resource not found
- `500 Internal Server Error` - server error

## Minimal scaling notes (short)
- Serve the frontend static files from a CDN and host backend behind a load balancer.
- Use managed DB (Mongo Atlas), enable connection pooling, and add caching (Redis) for heavy read traffic.
- Use stateless JWTs with short expiry and refresh tokens; store secrets in a vault and deploy via CI/CD pipelines.

