# syntax=docker/dockerfile:1

# =========================
# 1. Build stage
# =========================
FROM node:20-alpine AS build

WORKDIR /app

# Copy dependency files first.
# This allows Docker to reuse this layer when application
# source code changes but dependencies do not.
COPY package*.json ./

# Cache npm downloads between builds.
RUN --mount=type=cache,target=/root/.npm \
    npm ci

# Copy application source code.
COPY . .

# Build the application.
RUN npm run build


# =========================
# 2. Production dependencies
# =========================
FROM node:20-alpine AS production-deps

WORKDIR /app

COPY package*.json ./

# Install only production dependencies.
RUN --mount=type=cache,target=/root/.npm \
    npm ci --omit=dev


# =========================
# 3. Runtime stage
# =========================
FROM node:20-alpine AS runtime

WORKDIR /app

# Create non-root user and group.
RUN addgroup -S appgroup && \
    adduser -S appuser -G appgroup

# Copy production dependencies.
COPY --from=production-deps /app/node_modules ./node_modules

# Copy package metadata.
COPY --from=build /app/package*.json ./

# Copy only the application build output.
COPY --from=build /app/dist ./dist

# Give application user ownership.
RUN chown -R appuser:appgroup /app

# Run as non-root user.
USER appuser

EXPOSE 3000

# Container health check.
HEALTHCHECK --interval=30s \
            --timeout=5s \
            --start-period=10s \
            --retries=3 \
    CMD wget --quiet --tries=1 --spider \
        http://localhost:3000/health || exit 1

CMD ["npm", "start"]


#docker buildx build \
#  --cache-from=type=local,src=/opt/buildkit-cache \
#  --cache-to=type=local,dest=/opt/buildkit-cache \
#  -t my-node-app:latest \
#  .
