# Stage 1 - Build
FROM golang:1.26-alpine AS builder

WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download

COPY . .

RUN CGO_ENABLED=0 GOOS=linux \
go build -ldflags="-w -s" \
-o skillpulse .

# Stage 2 - Runtime
FROM alpine:3.23

WORKDIR /app

RUN apk add --no-cache ca-certificates=20250911-r0 || \
    apk add --no-cache ca-certificates

COPY --from=builder /app/skillpulse .

USER nobody

EXPOSE 8080

CMD ["./skillpulse"]
