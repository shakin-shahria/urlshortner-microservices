# --- STAGE 1: Build Environment ---
FROM golang:1.24-alpine AS builder

# Install build tools required to compile CGO dependencies (SQLite)
RUN apk add --no-cache gcc musl-dev sqlite-dev

WORKDIR /app

# Copy ONLY the explicit dependency file
COPY go.mod ./
RUN go mod download

# Copy ONLY the explicit application file instead of the whole folder
COPY main.go ./

# Compile the Go application with SQLite tags
ENV CGO_ENABLED=1
RUN go build -tags "libsqlite3" -o urlshortener main.go

# --- STAGE 2: Minimal Production Runtime ---
FROM alpine:3.19

# Install ONLY the runtime SQLite library
RUN apk add --no-cache sqlite-libs

WORKDIR /app

# Copy the compiled binary from the builder stage
COPY --from=builder /app/urlshortener .

# Expose the correct application port
EXPOSE 8000

CMD ["./urlshortener"]