# ----------------------
# Stage 1: Build Stage
# ----------------------
FROM maven:3.9.0-eclipse-temurin-17 AS build

WORKDIR /app

# Copy pom.xml first (for caching dependencies)
COPY pom.xml .

# Download dependencies
RUN mvn dependency:go-offline

# Copy source code
COPY src ./src

# Build the jar (skip tests in CI build if you want)
RUN mvn clean package -DskipTests

# ----------------------
# Stage 2: Runtime Stage
# ----------------------
FROM eclipse-temurin:17-jdk-jammy

WORKDIR /app

# Copy jar from build stage
COPY --from=build /app/target/shopping-cart-0.0.1-SNAPSHOT.jar app.jar

# Expose Spring Boot port
EXPOSE 8070

# Run the app
ENTRYPOINT ["java", "-jar", "app.jar"]
