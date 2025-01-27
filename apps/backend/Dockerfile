FROM oven/bun:1.2.0 as base

WORKDIR /usr/src/app

FROM base AS install 
RUN mkdir -p /temp/dev 
COPY bun.lockb package.json /temp/dev/
RUN cd /temp/dev && bun install

RUN mkdir -p /temp/prod
COPY bun.lockb package.json /temp/prod/
COPY package.json /temp/prod/
RUN cd /temp/prod && bun install

FROM base AS prerelease
COPY --from=install /temp/dev/node_modules node_modules
COPY . .

# Test step
# ENV NODE_ENV=TEST
# RUN bun test
# RUN bun build

USER bun
EXPOSE 9000/tcp
ENTRYPOINT ["bun","run","apps/backend/index.ts"]