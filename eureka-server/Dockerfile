# ----------------------------------------------------------
# 🧱 STAGE 1: Build the application (using Maven)
# ----------------------------------------------------------
FROM maven:3.9.6-eclipse-temurin-17 AS build
WORKDIR /app

# Copy Maven project files
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Copy source code
COPY src ./src

# Package the Spring Boot application (skip tests for speed)
RUN mvn clean package -DskipTests

# ----------------------------------------------------------
# 🚀 STAGE 2: Run the application (lighter image)
# ----------------------------------------------------------
FROM eclipse-temurin:17-jdk
WORKDIR /app

# Copy the JAR file from the previous build stage
COPY --from=build /app/target/*.jar app.jar

# Expose the port your User Service runs on
EXPOSE 8761

# Set environment variable for JVM optimization (optional)
ENV JAVA_OPTS="-Xms256m -Xmx512m"

# Start the application
ENTRYPOINT ["sh", "-c", "java $JAVA_OPTS -jar app.jar"]
