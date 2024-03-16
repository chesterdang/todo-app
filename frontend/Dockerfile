FROM node:18-alpine AS builder
WORKDIR /app

COPY package.json yarn.lock* ./
RUN yarn --frozen-lockfile
COPY . .
RUN yarn build


FROM nginx:1.25.4-alpine AS runner
COPY --from=builder /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]