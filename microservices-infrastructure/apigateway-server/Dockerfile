FROM maven:3.9.9-eclipse-temurin-21 AS build
WORKDIR /build

COPY pom.xml ./
RUN mvn -B -ntp dependency:go-offline || true

COPY src ./src
RUN mvn -B -ntp clean package -Dmaven.test.skip=true

FROM eclipse-temurin:21-jre
WORKDIR /app

RUN apt-get update \
    && apt-get install -y --no-install-recommends curl \
    && rm -rf /var/lib/apt/lists/* \
    && useradd -r -u 1001 appuser

COPY --from=build /build/target/*.jar app.jar

USER appuser
EXPOSE 8085
ENTRYPOINT ["java", "-XX:MaxRAMPercentage=75.0", "-jar", "/app/app.jar"]
