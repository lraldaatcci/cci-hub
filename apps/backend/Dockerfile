FROM oven/bun:1.2.0

WORKDIR /app

COPY ../../packages/ts-schemas ./packages/ts-schemas

RUN cd ./packages/ts-schemas && bun link

COPY . .

RUN bun link @repo/ts-schemas

RUN bun install

EXPOSE 9000

CMD ["bun", "index.ts"]
