# Stage 1: Build de la aplicación
FROM maven:3.9.6-eclipse-temurin-17 AS build
WORKDIR /app

# Copiar el archivo de configuración de dependencias
COPY pom.xml .

# Descargar dependencias en caché
RUN mvn dependency:go-offline -B

# Copiar el código fuente y compilar el archivo JAR
COPY src ./src
RUN mvn clean package -DskipTests

# Stage 2: Runtime seguro para producción
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app

# Crear usuario no-root por seguridad (Exigencia pauta IE1)
RUN addgroup -S innovatechgroup && adduser -S innovatechuser -G innovatechgroup

# Copiar el JAR generado desde el stage de build
COPY --from=build /app/target/*.jar app.jar

# Asignar permisos al usuario no-root
RUN chown -R innovatechuser:innovatechgroup /app
USER innovatechuser

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]