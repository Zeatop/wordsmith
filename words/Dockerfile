FROM maven:3.9.9-eclipse-temurin-21 AS build
WORKDIR /app
COPY pom.xml .
COPY src ./src
RUN mvn -q clean package

FROM eclipse-temurin:21-jdk-alpine AS jre-build
WORKDIR /app
COPY --from=build /app/target/words.jar ./words.jar
COPY --from=build /app/target/dependency ./dependency
RUN set -eux; \
		deps="$(jdeps \
			--ignore-missing-deps \
			--print-module-deps \
			--recursive \
			--class-path 'dependency/*' \
			words.jar)"; \
		jlink \
			--add-modules "$deps,jdk.crypto.ec" \
			--strip-debug \
			--no-man-pages \
			--no-header-files \
			--compress=2 \
			--output /opt/jre

FROM alpine:3.20
WORKDIR /app
RUN apk add --no-cache ca-certificates \
		&& addgroup -S app \
		&& adduser -S app -G app
COPY --from=jre-build /opt/jre /opt/jre
COPY --from=build /app/target/words.jar ./words.jar
COPY --from=build /app/target/dependency ./dependency
ENV PATH="/opt/jre/bin:${PATH}"
USER app
CMD ["java", "-Xmx8m", "-Xms8m", "-cp", "words.jar:dependency/*", "Main"]