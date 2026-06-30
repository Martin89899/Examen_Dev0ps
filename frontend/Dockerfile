# --- ETAPA 1: Construcción (Build) ---
FROM maven:3.9-eclipse-temurin-17 AS builder
WORKDIR /app
# Copiamos el archivo de configuración de Maven para aprovechar la caché
COPY pom.xml .
RUN mvn dependency:go-offline -B
# Copiamos el código fuente y compilamos el proyecto
COPY src ./src
RUN mvn clean package -DskipTests
# Ejecutamos el generador para crear los archivos estáticos
RUN java -cp target/classes:$(mvn dependency:build-classpath -Dmdep.outputFile=/dev/stdout -q) com.eval3.frontend.StaticPageGenerator

# --- ETAPA 2: Producción (Servidor Web) ---
FROM nginx:1.25-alpine
# Copiamos los archivos estáticos generados por Java hacia el directorio de Nginx
COPY --from=builder /app/output /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
