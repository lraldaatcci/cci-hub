FROM oven/bun:1.2.2 AS base

WORKDIR /usr/src/app

FROM base AS builder 
WORKDIR /app

RUN bun add -g turbo

COPY . .

RUN turbo prune @repo/landing --docker


FROM base AS installer
WORKDIR /app
COPY --from=builder /app/out/json/ .
RUN bun install

COPY --from=builder /app/out/full/ .
RUN cd apps/landing && bun add astro && bun run build

FROM base AS runner
WORKDIR /app

# Test step
# ENV NODE_ENV=TEST
# RUN bun test


USER bun
ENV PORT=4321
ENV HOST=0.0.0.0
EXPOSE 4321/tcp
ENTRYPOINT ["bun", "run", "apps/landing/dist/server/entry.mjs"]