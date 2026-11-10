ARG NGINX_RUNTIME_IMAGE=nginx:stable-alpine-slim

# ---- Stage 1: Build ----
FROM node:20-alpine AS build

WORKDIR /app

# Copy package files and install dependencies
COPY package.json package-lock.json* ./
RUN npm ci

# Copy source and build
COPY . .
RUN npm run build

# ---- Stage 2: Serve with Nginx ----
FROM ${NGINX_RUNTIME_IMAGE}

# Copy custom nginx config
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copy build output
COPY --from=build /app/dist /usr/share/nginx/html

RUN chown -R nginx:nginx /var/cache/nginx /var/run

USER nginx

EXPOSE 8080

CMD ["nginx", "-g", "daemon off;"]
