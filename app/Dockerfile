FROM node:18-alpine AS deps
WORKDIR /app

COPY package.json package-lock.json ./

RUN npm install

FROM node:18-alpine AS builder
WORKDIR /app

COPY --from=deps /app/node_modules ./node_modules
COPY . .

RUN npm run build

FROM node:18-alpine AS dev
WORKDIR /app

ARG USER=1000
ARG GROUP=1000

USER $USER:$GROUP

CMD npm install && npm run dev

FROM nginx:1.23-alpine AS prod
WORKDIR /app

COPY --from=builder /app/dist /usr/share/nginx/html

EXPOSE 80
ENV PORT 80
