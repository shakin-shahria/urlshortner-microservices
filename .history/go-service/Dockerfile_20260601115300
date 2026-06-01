# --- STAGE 1: Build Environment (Requires CGO + Compilers) ---
FROM golang:1.24-alpine AS builder

# Install build tools required to compile CGO dependencies (SQLite)
RUN apk add --no-cache gcc musl-dev sqlite-dev

WORKDIR /app

# Cache dependencies
COPY go.mod ./
RUN go mod download

# Copy source code and compile
COPY main.go ./
ENV CGO_ENABLED=1
RUN go build -tags "libsqlite3" -o urlshortener main.go

# --- STAGE 2: Minimal Production Runtime ---
FROM alpine:3.19

# Install ONLY the runtime SQLite library (no compilers, perfectly secure)
RUN apk add --no-cache sqlite-libs

WORKDIR /app

# Copy the compiled binary from the builder stage
COPY --from=builder /app/urlshortener .

# The app configuration exposes Port 8000
EXPOSE 8000

CMD ["./urlshortener"]