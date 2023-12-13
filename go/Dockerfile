ARG GO_VERSION=1.21
FROM --platform=$BUILDPLATFORM golang:${GO_VERSION}-alpine AS base

# Build stage
FROM base AS builder
WORKDIR /src

COPY go.mod .
COPY go.sum* .
RUN go mod download

COPY . .
RUN CGO_ENABLED=0 go build -v -o bin/app
RUN CGO_ENABLED=0 go test -v

# Run stage
FROM gcr.io/distroless/static AS runner

USER nonroot:nonroot
COPY --from=builder --chown=nonroot:nonroot /src/bin/app /app

EXPOSE 8080
ENTRYPOINT ["/app"]
