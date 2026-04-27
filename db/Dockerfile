FROM postgres:17-alpine
WORKDIR /app
ENV POSTGRES_HOST_AUTH_METHOD=trust
COPY words.sql /docker-entrypoint-initdb.d/words.sql
