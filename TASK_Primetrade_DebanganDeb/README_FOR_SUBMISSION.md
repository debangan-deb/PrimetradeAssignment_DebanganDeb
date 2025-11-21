# TASK 2 - Submission README

## How to run (local)
Backend:
```
cd server
npm install
# create .env per .env.example
npm start
```

Frontend:
```
cd client
npm install
npm run dev
```

Default backend base URL: http://localhost:5000

## Provided files
- postman_collection_exact.json - Postman collection generated from server code.
- API_DOCS.md - Human-readable API docs (previously generated).
- .env.example - Example environment variables (do NOT include secrets).
- README_FOR_SUBMISSION.md - this file.

## Notes to reviewers
- The server uses JWT for authentication and bcrypt for password hashing (see server files).
- Remove any real `.env` before publishing.
- For production: build client (`npm run build`) and serve static files via CDN or Nginx, host backend in Docker behind a load balancer, use managed DB (Mongo Atlas), caching (Redis), and secret management.