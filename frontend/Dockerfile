# ---------- 1) Build stage ----------
FROM node:20-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

# 👇 Add build ARG + ENV so Vite gets it
ARG VITE_BACKEND_SERVER
ENV VITE_BACKEND_SERVER=${VITE_BACKEND_SERVER}

RUN npm run build

# ---------- 2) Runtime stage ----------
FROM nginx:1.27-alpine

RUN rm /etc/nginx/conf.d/default.conf
COPY nginx.conf /etc/nginx/conf.d/default.conf

COPY --from=builder /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
