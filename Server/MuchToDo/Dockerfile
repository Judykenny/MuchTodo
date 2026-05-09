# Build stage
FROM golang:1.23-alpine AS builder

WORKDIR /app

# Install git (needed for some go modules)
RUN apk add --no-cache git

COPY go.mod go.sum ./

# Tidy and download dependencies
RUN go mod tidy && go mod download

COPY . .

RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main ./cmd/api

# Final stage
FROM alpine:3.18

WORKDIR /app

RUN apk --no-cache add ca-certificates tzdata

COPY --from=builder /app/main .

EXPOSE 8080

CMD ["./main"]
