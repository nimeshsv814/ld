# FROM node:20-alpine
# WORKDIR /usr/src/app
# COPY package*.json ./
# RUN npm install
# COPY . .
# EXPOSE 5173
# CMD ["npm", "run", "dev", "--", "--host"]

# Build stage
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./

# ← KEY FIX: remove lock file and reinstall fresh
RUN rm -f package-lock.json && npm install

COPY . .
RUN npm run build

# Serve stage
FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]