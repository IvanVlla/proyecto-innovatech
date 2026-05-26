# Etapa 1: Construcción
FROM node:18-alpine AS build
WORKDIR /app

# Copiar archivos de dependencias para aprovechar la caché de Docker
COPY package*.json ./

# Instalar dependencias de forma limpia
RUN npm install

# Copiar el resto del código fuente del frontend
COPY . .

# Compilar la aplicación para producción (genera la carpeta dist)
RUN npm run build

# Etapa 2: Servidor Nginx seguro (Corre como usuario no-root por defecto)
FROM nginxinc/nginx-unprivileged:alpine

# Copiar los archivos estáticos compilados desde la etapa de construcción
COPY --from=build /app/dist /usr/share/nginx/html

# Puerto interno en el que escucha esta imagen unprivileged de Nginx
EXPOSE 8080

CMD ["nginx", "-g", "daemon off;"]