FROM golang:1.23-alpine AS build
WORKDIR /app

COPY dispatcher.go .
COPY static ./static

RUN go build -o dispatcher dispatcher.go

FROM alpine:3.23
WORKDIR /app
COPY --from=build /app/dispatcher .
COPY --from=build /app/static ./static

EXPOSE 80
CMD ["./dispatcher"]