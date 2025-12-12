# -------- Étape 1 : Build (On construit notre propre environnement) --------
# On part d'une image qui a VRAIMENT Java 25 (version Debian/Slim plus stable que Alpine pour ça)
FROM openjdk:25-slim AS build

# 1. On installe Maven manuellement (car il n'est pas dans l'image Java 25)
RUN apt-get update && apt-get install -y maven

WORKDIR /build

# 2. On copie les fichiers
COPY pom.xml .
COPY src src

# 3. On lance la compilation (Maven utilise maintenant le JDK 25 de l'image)
RUN mvn clean package -DskipTests

# -------- Étape 2 : Image Finale (Exécution) --------
# On reprend la même image Java 25 pour lancer l'appli
FROM openjdk:25-slim

WORKDIR /app

# On récupère le JAR qu'on vient de fabriquer
COPY --from=build /build/target/*.jar app.jar

EXPOSE 8080
ENV DB_PASSWORD=""

ENTRYPOINT ["java", "-jar", "/app/app.jar"]