#Multistage Docker Build

#-----Build Stage-------#

FROM golang:1.24.6-alpine AS builder

# Set Workdir

WORKDIR /usr/src/app

RUN --mount=type=cache,target=/go/pkg/mod \
    --mount=type=cache,target=/root/.cache/go-build \
    mkdir -p /root/.cache/go-build

#Copy
COPY go.mod go.sum ./

# Download

RUN go mod download

# Copy the rest of the files/source code

COPY . .

RUN go build -o product-catalog .

#------Release Stage---------#

# trivy vuln check done on alpine image. 0 vuln. found.
FROM alpine AS release

RUN addgroup -S products-group && adduser -S products-user -G products-group

WORKDIR /usr/src/app


COPY ./products/ ./products
COPY --from=builder /usr/src/app/product-catalog/ ./

# Ensure correct owdership and permission
RUN chown -R products-user:products-group /usr/src/app/ && chmod +x product-catalog

# switch user context
USER products-user

# set env. viarable
ENV PRODUCT_CATALOG_PORT=3550

ENTRYPOINT ["./product-catalog"]
