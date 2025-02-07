# Base image with Bun installed
FROM oven/bun:1.2.2 AS base

# Builder stage - Prepare and prune dependencies
FROM base AS builder
# Check https://github.com/oven-sh/bun#docker to understand why libc6-compat might be needed.
#RUN apk update && apk add --no-cache libc6-compat

# Set working directory
WORKDIR /app

# Install turbo globally (if you're using it)
RUN bun add -g turbo

# Copy all files from the current directory
COPY . .

# Prune dependencies if you're using TurboRepo
RUN turbo prune @repo/backend --docker

# Installer stage - Install dependencies and build the project
FROM base AS installer
#RUN apk update && apk add --no-cache libc6-compat

WORKDIR /app

# First install dependencies (as they change less often)
COPY --from=builder /app/out/json/ .
RUN bun install

# Build the project and its dependencies
COPY --from=builder /app/out/full/ .

# Uncomment and use build args to enable remote caching
# ARG TURBO_TEAM
# ENV TURBO_TEAM=$TURBO_TEAM
# ARG TURBO_TOKEN
# ENV TURBO_TOKEN=$TURBO_TOKEN

# Runner stage - Run the application in production mode
FROM base AS runner
WORKDIR /app

# Copy the built application from the installer stage
COPY --from=installer /app .

# Command to start the BunJS server
ENTRYPOINT ["bun", "run", "apps/backend/index.ts"]