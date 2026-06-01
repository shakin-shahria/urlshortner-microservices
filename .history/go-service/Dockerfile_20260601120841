FROM golang:1.24-alpine

# Install complete build tools required by CGO/SQLite
RUN apk add --no-cache gcc musl-dev sqlite-dev build-base

WORKDIR /app

# Copy ONLY the dependency manifest file
COPY go.mod ./

# Force Go to download dependencies and generate its own fresh, valid checksum definitions
RUN go mod download && go mod tidy

# Copy the application source code file
COPY main.go ./

# Compile with CGO enabled
ENV CGO_ENABLED=1
RUN go build -tags "libsqlite3" -o urlshortener main.go

# Expose the application port
EXPOSE 8000

# Run the binary
CMD ["./urlshortener"]