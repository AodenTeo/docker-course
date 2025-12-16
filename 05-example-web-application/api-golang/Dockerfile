FROM golang:tip-alpine3.22 AS build

WORKDIR /app

COPY go.mod go.sum ./

RUN --mount=type=cache,target=/go/pkg/mod \
    go mod download

COPY . .

# compile the application and all its dependencies 
RUN apk add --no-cache gcc musl-dev && \
    CGO_ENABLED=1 go build \
      -ldflags="-linkmode external -extldflags -static" \
      -tags netgo \
      -o api-golang

# build a separate container to run the compiled binary
FROM scratch

COPY --from=build /app/api-golang /api-golang

CMD ["/api-golang"]