# -------- Étape 1 : Build avec Java 25 --------
# On utilise une image Early Access (EA) de Java 25
FROM openjdk:25-ea-jdk-slim AS build

WORKDIR /build

# 1. On installe juste de quoi télécharger (pas de "apt install maven" !)
RUN apt-get update && apt-get install -y curl tar

# 2. On installe Maven manuellement pour qu'il utilise NOTRE Java 25
ENV MAVEN_VERSION=3.9.9
RUN curl -fsSL https://archive.apache.org/dist/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz | tar xzf - -C /opt \
    && ln -s /opt/apache-maven-$MAVEN_VERSION /opt/maven \
    && ln -s /opt/maven/bin/mvn /usr/bin/mvn

# 3. Vérification (optionnel, pour voir dans les logs)
RUN java -version && mvn -version

# 4. Copie des fichiers et compilation
COPY pom.xml .
COPY src src
RUN mvn clean package -DskipTests

# -------- Étape 2 : Image Finale Java 25 --------
FROM openjdk:25-ea-jdk-slim

WORKDIR /app
COPY --from=build /build/target/*.jar app.jar

EXPOSE 8080
ENV DB_PASSWORD=""

ENTRYPOINT ["java", "-jar", "/app/app.jar"]