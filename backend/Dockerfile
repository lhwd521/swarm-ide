FROM oven/bun:1-alpine AS builder

WORKDIR /app

# 复制所有文件
COPY . .

# 安装依赖
RUN bun install

# 构建
RUN bun run build

# 运行阶段 - 不用standalone，直接跑
FROM oven/bun:1-alpine AS runner

WORKDIR /app

ENV NODE_ENV=production
ENV PORT=3017
ENV HOSTNAME=0.0.0.0

# 复制所有需要的文件
COPY --from=builder /app/package.json ./
COPY --from=builder /app/bun.lock ./
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/next.config.ts ./
COPY --from=builder /app/next.config.mjs ./

EXPOSE 3017

CMD ["bun", "run", "start"]
