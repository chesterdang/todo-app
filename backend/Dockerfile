FROM node:20-alpine AS builder
WORKDIR /app

RUN addgroup --system --gid 1001 mygroup
RUN adduser --system --uid 1001 myuser
RUN chown -R myuser:mygroup .
USER myuser

COPY package.json yarn.lock* ./
RUN yarn --frozen-lockfile
COPY . .
RUN yarn prisma generate
RUN yarn build

EXPOSE 3000

CMD ["yarn", "start"]