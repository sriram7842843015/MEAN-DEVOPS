FROM node:18-alpine AS build
WORKDIR /app

COPY package*.json ./

# Install dependencies
RUN npm install

# Copy source code
COPY . .

# Building Angular app (Angular 15 compatible)
RUN npm run build -- --configuration production

# -------- Stage 2: Serve with Nginx --------
FROM nginx:alpine

# Remove default nginx config
#RUN rm -rf /etc/nginx/conf.d/default.conf

# Copy built Angular files
COPY --from=build /app/dist /usr/share/nginx/html

# Expose port 80
EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]