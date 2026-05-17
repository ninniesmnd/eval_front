# build
FROM node:20-alpine AS builder
WORKDIR /app

# dependencias
COPY package*.json ./

# node
RUN npm install

# codigo del front
COPY . .

# compilar
RUN npm run build

# run
FROM nginx:alpine
WORKDIR /usr/share/nginx/html

# eliminar archivos de nginx
RUN rm -rf ./*

# archivos estáticos del build
COPY --from=builder /app/dist .

# cambio de permisos para nginx}
RUN chown -R nginx:nginx /usr/share/nginx/html && \
    chmod -R 755 /usr/share/nginx/html && \
    chown -R nginx:nginx /var/cache/nginx && \
    chown -R nginx:nginx /var/log/nginx && \
    touch /var/run/nginx.pid && \
    chown -R nginx:nginx /var/run/nginx.pid

USER nginx

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]