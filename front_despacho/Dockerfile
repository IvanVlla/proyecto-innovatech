# Etapa 1: Construcción
FROM node:18-alpine AS build
WORKDIR /app

# Declarar los argumentos de construcción que enviará GitHub Actions
ARG VITE_API_BASE_URL_VENTAS
ARG VITE_API_BASE_URL_DESPACHOS

# Convertirlos en variables de entorno accesibles para Vite
ENV VITE_API_BASE_URL_VENTAS=$VITE_API_BASE_URL_VENTAS
ENV VITE_API_BASE_URL_DESPACHOS=$VITE_API_BASE_URL_DESPACHOS

# Copiar archivos de dependencias
COPY package*.json ./

# Instalar dependencias
RUN npm install

# Copiar el código fuente del proyecto
COPY . .

# Compilar la aplicación (Vite inyectará las variables ENV en el empaquetado)
RUN npm run build

# Etapa 2: Servidor Nginx seguro (No-root por defecto)
FROM nginxinc/nginx-unprivileged:alpine

# Copiar el build estático generado
COPY --from=build /app/dist /usr/share/nginx/html

# Puerto interno por defecto de nginx-unprivileged
EXPOSE 8080

CMD ["nginx", "-g", "daemon off;"]