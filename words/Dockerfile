FROM maven:3.9.9-eclipse-temurin-21 AS build
WORKDIR /app
COPY pom.xml .
COPY src ./src
RUN mvn -q package -DskipTests

FROM alpine:3.20
WORKDIR /app
RUN apk add --no-cache ca-certificates \
    && addgroup -S app && adduser -S app -G app

COPY --from=eclipse-temurin:21-jre-alpine /opt/java/openjdk /opt/java/openjdk
ENV JAVA_HOME=/opt/java/openjdk
ENV PATH="${JAVA_HOME}/bin:${PATH}"

COPY --from=build /app/target/words.jar ./words.jar
COPY --from=build /app/target/dependency ./dependency

USER app
CMD ["java", "-Djava.net.preferIPv4Stack=true", "-Xmx16m", "-Xms8m", "-jar", "words.jar"]