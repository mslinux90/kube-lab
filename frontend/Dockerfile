# Stage 1 - Build React Application

FROM node:20 AS builder

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

RUN npm run build

# Stage 2 - Run using Nginx

FROM nginx:latest

COPY --from=builder /app/build /usr/share/nginx/html

EXPOSE 80

CMD ["nginx","-g","daemon off;"]
