FROM oven/bun:1-alpine AS builder

WORKDIR /app

# 复制依赖文件
COPY package.json bun.lock ./

# 安装依赖
RUN bun install --frozen-lockfile

# 复制源码
COPY . .

# 构建 Next.js (standalone模式)
RUN bun run build

# 运行阶段
FROM oven/bun:1-alpine AS runner

WORKDIR /app

ENV NODE_ENV=production
ENV PORT=3017
ENV HOSTNAME=0.0.0.0

# 复制 standalone 输出
COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/.next/static ./.next/static

EXPOSE 3017

CMD ["node", "server.js"]
