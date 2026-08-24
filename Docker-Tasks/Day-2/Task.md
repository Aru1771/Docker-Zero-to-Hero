Task: Production Multi-Stage Dockerfile

You have a Node.js application with:

myapp/
├── package.json
├── package-lock.json
├── src/
│   ├── server.js
│   └── ...
├── .dockerignore
└── Dockerfile

Your requirements:

Use a multi-stage build
Use node:20-alpine
Build dependencies separately
Optimize Docker layer caching
Use npm ci
Application should run as a non-root user
Production image should contain only production dependencies
Accept BUILD_ENV using ARG
Set NODE_ENV using ENV
Add a HEALTHCHECK
Application listens on port 3000
Do not use latest
Do not use unnecessary packages in the final image
Your task

Write the Dockerfile yourself.

Don't look for the answer yet. Send me your Dockerfile, and I'll review it like a 5+ years DevOps interview/code review and point out every issue.
