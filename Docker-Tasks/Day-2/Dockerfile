# =========================
# Build Stage
# =========================
FROM node:20-alpine AS build

WORKDIR /app

COPY package*.json ./

RUN --mount=type=cache,target=/root/.npm \
    npm ci

COPY . .

ARG NODE_ENV=production
ENV NODE_ENV=${NODE_ENV}

RUN npm run build


# =========================
# Production Dependencies
# =========================
FROM node:20-alpine AS production-deps

WORKDIR /app

COPY package*.json ./

RUN --mount=type=cache,target=/root/.npm \
    npm ci --omit=dev


# =========================
# Runtime Stage
# =========================
FROM node:20-alpine AS runtime

WORKDIR /app

RUN addgroup -S appgroup && \
    adduser -S appuser -G appgroup

COPY --from=production-deps --chown=appuser:appgroup \
    /app/node_modules ./node_modules

COPY --from=build --chown=appuser:appgroup \
    /app/dist ./dist

COPY --from=build --chown=appuser:appgroup \
    /app/package*.json ./

USER appuser

ENV NODE_ENV=production

EXPOSE 3000

HEALTHCHECK \
    --interval=30s \
    --timeout=5s \
    --start-period=10s \
    --retries=3 \
    CMD ["node", "dist/healthcheck.js"]

CMD ["node", "dist/main.js"]



docker buildx build \
  --build-arg NODE_ENV=production \
  --cache-from=type=local,src=/opt/buildkit-cache \
  --cache-to=type=local,dest=/opt/buildkit-cache \
  -t my-node-app:1.0.0 \
  --load \
  .


we can bypass the production-dependencies stage by using "npm prune --omit=dev" after build the application in the build stage. 
This will remove the dev dependencies from the node_modules folder, 
allowing us to use the same node_modules for production without needing a separate stage.

and we can directly copy the node_modules folder from the build stage to the runtime stage, 
which will reduce the number of layers in the final image and make it smaller.



