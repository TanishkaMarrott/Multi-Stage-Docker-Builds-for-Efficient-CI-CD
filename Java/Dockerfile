# First stage: Build the application
FROM maven:3.8.1-openjdk-11 AS build
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline
COPY src ./src
RUN mvn package -DskipTests

# Second stage: Create the final image
FROM openjdk:11-jre-slim
WORKDIR /app
COPY --from=build /app/target/myapp.jar /app/myapp.jar
CMD ["java", "-jar", "myapp.jar"]
