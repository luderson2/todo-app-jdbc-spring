# -------- Étape 1 : Build Maven (Image officielle avec Java 21) --------
FROM maven:3.9-eclipse-temurin-21-alpine AS build

WORKDIR /build

# Copie des fichiers
COPY pom.xml .
COPY src src

# Compilation (Maven est déjà installé dans cette image, ça ne plantera pas)
RUN mvn clean package -DskipTests

# -------- Étape 2 : Image Finale (Exécution sur Java 21) --------
FROM eclipse-temurin:21-jre-alpine

WORKDIR /app

# Récupération du JAR
COPY --from=build /build/target/*.jar app.jar

# Configuration
EXPOSE 8080
ENV DB_PASSWORD=""

ENTRYPOINT ["java", "-jar", "/app/app.jar"]