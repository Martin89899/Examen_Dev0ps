# --- ETAPA 1: Construcción (Build) ---
FROM maven:3.9-eclipse-temurin-17 AS builder
WORKDIR /app
# Copiamos el archivo de configuración de Maven para aprovechar la caché
COPY pom.xml .
RUN mvn dependency:go-offline -B
# Copiamos el código fuente y compilamos el proyecto
COPY src ./src
RUN mvn clean package -DskipTests

# --- ETAPA 2: Ejecución (Runtime) ---
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
# Copiamos el archivo JAR generado en la etapa anterior
COPY --from=builder /app/target/*.jar app.jar
EXPOSE 8080
CMD ["java", "-jar", "app.jar"]
