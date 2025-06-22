# ---------- Stage 1: Build ----------
FROM maven:3.9.6-eclipse-temurin-17 AS build

# Set working directory
WORKDIR /build

# Copy the Maven wrapper + pom first to leverage Docker cache
COPY pom.xml .
COPY mvnw .
COPY .mvn .mvn

# Download dependencies
RUN ./mvnw dependency:go-offline -B

# Copy the rest of the source code
COPY src ./src

# Package the application (skip tests)
RUN ./mvnw clean package -DskipTests

# ---------- Stage 2: Run ----------
FROM eclipse-temurin:17-jdk-alpine

WORKDIR /app

# Copy the built jar
COPY --from=build /build/target/*.jar app.jar

# Run the app
ENTRYPOINT ["java", "-jar", "app.jar"]
