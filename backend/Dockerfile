# Build Stage
# We use the Amazon Corretto 17 image of Maven as the build stage base.
FROM maven:3.8.6-amazoncorretto-17 AS backend-build
# Set the working directory for the build stage.
ENV BACKEND_HOME /opt/backend
WORKDIR $BACKEND_HOME
# Copy the application's POM file in the image.
COPY pom.xml .
# Copy the application source code in the image.
COPY src ./src
# Compile the code into a JAR file, skipping unit tests.
RUN mvn package -DskipTests

# Run Stage
# We use the Amazon Corretto 17 image for running the backend.
FROM amazoncorretto:17
# Set the working directory for the runtime stage.
ENV BACKEND_HOME /opt/backend
WORKDIR $BACKEND_HOME
# Copy the compiled JAR from the build stage to the runtime stage.
COPY --from=backend-build $BACKEND_HOME/target/*.jar $BACKEND_HOME/backend.jar

# Define the entry point to run the Java application from the JAR file.
ENTRYPOINT java -jar backend.jar