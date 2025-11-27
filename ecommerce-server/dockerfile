# Stage 1: build jar
FROM maven:3.9-eclipse-temurin-17 AS build
WORKDIR /workspace
COPY pom.xml .
# copy only dependencies for caching
RUN mvn -B -q -DskipTests dependency:go-offline

COPY src ./src
RUN mvn -B -DskipTests package

# Stage 2: runtime
FROM eclipse-temurin:17-jre-alpine
ARG JAR_FILE=target/*.jar
WORKDIR /app
COPY --from=build /workspace/target/*.jar app.jar
EXPOSE 2025
ENTRYPOINT ["java","-jar","/app/app.jar"]
