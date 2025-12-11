# ---------- Build stage ----------
FROM maven:3.9.9-eclipse-temurin-17 AS build

WORKDIR /app

# Copy pom.xml and download dependencies
COPY pom.xml .
RUN mvn -q dependency:go-offline

# Copy source code
COPY src ./src

# Build the app
RUN mvn -q clean package -DskipTests

# ---------- Run stage ----------
FROM eclipse-temurin:17-jre

WORKDIR /app

# Copy the built jar from the build stage
COPY --from=build /app/target/*.jar app.jar

# Render will inject PORT, Spring uses it via server.port=${PORT:8080}
ENV PORT=8080
EXPOSE 8080

CMD ["java", "-jar", "app.jar"]
