FROM oven/bun:1.3.11 AS bun

FROM node:22-slim AS builder

COPY --from=bun /usr/local/bin/bun /usr/local/bin/bun

# install git to install plugins
RUN apt-get update && apt-get install -y git && rm -rf /var/lib/apt/lists/*

WORKDIR /usr/src/app
COPY package.json bun.lock ./
COPY quartz/ ./quartz/
COPY quartz.lock.json* .
RUN bun install --frozen-lockfile && bun run install-plugins

FROM node:22-slim
COPY --from=bun /usr/local/bin/bun /usr/local/bin/bun
WORKDIR /usr/src/app
COPY --from=builder /usr/src/app/ /usr/src/app/
COPY . .
CMD ["bun", "run", "quartz", "build", "--serve"]
