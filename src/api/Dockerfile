# -------- Étape 1 : Build Maven --------
FROM eclipse-temurin:25-jre-alpine-nightly AS build

# Dossier de travail dans le container
WORKDIR /build

# On copie le pom.xml du projet API
COPY pom.xml .

# On copie le code source du projet API
COPY src src

# On build le projet (sans tests)
RUN mvn clean package -DskipTests

# -------- Étape 2 : Image finale --------
FROM eclipse-temurin:17-jre-alpine

# Dossier de travail de l'image finale
WORKDIR /app

# Copier le JAR généré (dans /build/target)
COPY --from=build /build/target/*.jar app.jar

# Port exposé par l'API
EXPOSE 8080

# Variable d'environnement pour le mot de passe DB (valeur par défaut vide)
ENV DB_PASSWORD=""

# Commande de démarrage
ENTRYPOINT ["java", "-jar", "/app/app.jar"]
