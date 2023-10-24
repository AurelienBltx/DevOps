# Build
#On utilise l'image suivante de maven
FROM maven:3.8.6-amazoncorretto-17 AS backend-build
#Répertoire de travail
ENV BACKEND_HOME /opt/backend
WORKDIR $BACKEND_HOME
#On copie le pom et le dossier src
COPY pom.xml .
COPY src ./src
#On compile le code en fichier JAR en sautant les tests unitaires
RUN mvn package -DskipTests

# Run
#On utilise l'image suivante pour executer le backend
FROM amazoncorretto:17
ENV BACKEND_HOME /opt/backend
WORKDIR $BACKEND_HOME
#On compile dans un fichier backend.jar
COPY --from=backend-build $BACKEND_HOME/target/*.jar $BACKEND_HOME/backend.jar

ENTRYPOINT java -jar backend.jar