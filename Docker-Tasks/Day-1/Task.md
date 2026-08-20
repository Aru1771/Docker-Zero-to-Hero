🐳 Docker — Day 1 Task
=========================

Scenario: Production Node.js Application

Your company has a Node.js application that currently uses this Dockerfile:

      FROM node:20
      
      WORKDIR /app
      
      COPY . .
      
      RUN npm install
      
      EXPOSE 3000
      
      CMD ["npm", "start"]

The development team reports:
-----------------------------
Docker image is 1.2 GB
Builds are taking 8–10 minutes
Every small source-code change causes a long Docker build
Container is running as root
npm install downloads dependencies repeatedly
Security team wants a smaller production image
Jenkins builds the image for every Git commit

Your Task
----------
Create a production-ready Dockerfile that addresses all of the above problems.

Requirements

Your Dockerfile must:

Use a multi-stage build.
Use an appropriate lightweight Node.js base image.
Use npm ci instead of npm install.
Optimize Docker layer caching.
Use a BuildKit cache mount for npm.
Run the application as a non-root user.
Copy only the required files into the final image.
Expose port 3000.
Start the application correctly.
Keep the final image as small as reasonably possible.

Bonus — 5+ Years Level
------------------------
Also think about:

A. .dockerignore

What files should you exclude from the Docker build context?

For example:

node_modules
.git
...

Add the appropriate entries.

B. Build command

Show me the docker buildx build command you would use to build the image with BuildKit caching.

C. Jenkins

Assume your Jenkins agent is a temporary Docker container.

Explain:

Where would you store the BuildKit cache so that it survives when the Jenkins agent container is deleted?

D. Security

Explain why running:

USER root

in production is undesirable.

🎯 Interview Questions
--------------------------
After completing the task, answer these without looking up the answers:

Why did you use multi-stage builds?
Why is npm ci preferred over npm install in CI/CD?
Why should COPY package*.json come before COPY . .?
What exactly does this do?
RUN --mount=type=cache,target=/root/.npm npm ci
Is /root/.npm a directory on the Jenkins host?
What happens to the BuildKit cache if the Jenkins Docker agent is deleted?
How would you persist that cache?
Why shouldn't the application run as root?
What is the difference between Docker layer cache and npm cache?
How would you reduce the final image size even further?

Your goal
----------
Don't just give me the Dockerfile.

Give me:

1. Dockerfile
2. .dockerignore
3. Build command
4. Jenkins cache strategy
5. Answers to the 10 interview questions


One thing to verify

If your application produces build/ instead of dist/, change:

COPY --from=build /app/dist ./dist

to:

COPY --from=build /app/build ./build
