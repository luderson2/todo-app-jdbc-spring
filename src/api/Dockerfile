# -------- Étape 1 : Build Maven --------
# On utilise une image qui contient DEJA Maven (sinon la commande mvn ne marche pas)
# Note: Si "25" n'est pas dispo, utilise "21" ou "23" qui sont les dernières stables.
# Pour le TP, on tente une image récente.
FROM maven:3.9-eclipse-temurin-21-alpine AS build

# Dossier de travail
WORKDIR /build

# Copie du pom.xml et du code source
COPY pom.xml .
COPY src src

# On build le projet
# (Assure-toi que ton pom.xml a bien <java.version>25</java.version>)
RUN mvn clean package -DskipTests

# -------- Étape 2 : Image finale --------
# ATTENTION : Ici, on DOIT utiliser Java 25 (ou une version Nightly/EA)
# Sinon ça ne pourra pas lire le code compilé au-dessus.
FROM eclipse-temurin:25-jre-alpine-nightly

# Dossier de travail
WORKDIR /app

# Copier le JAR généré depuis l'étape "build"
COPY --from=build /build/target/*.jar app.jar

# Port exposé
EXPOSE 8080

# Variable d'environnement (bonne pratique)
ENV DB_PASSWORD=""

# Démarrage
ENTRYPOINT ["java", "-jar", "/app/app.jar"]