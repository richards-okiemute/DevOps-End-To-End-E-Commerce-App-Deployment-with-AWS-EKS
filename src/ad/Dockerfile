# Mulitstage Java Docker build

#------build stage-------#

FROM eclipse-temurin:21-jdk AS builder

# Set Workdir
WORKDIR /usr/src/app

COPY gradlew* settings.gradle* build.gradle ./
COPY ./gradle ./gradle

RUN chmod +x ./gradlew
RUN ./gradlew
RUN ./gradlew downloadRepos

COPY . .

COPY ./pb ./proto
RUN chmod +x ./gradlew
RUN ./gradlew installDist -PprotoSourceDir=./proto

#---Release Stage-----#

FROM eclipse-temurin:21-jre AS release

WORKDIR /usr/src/app

COPY --from=builder /usr/src/app ./

ENV AD_PORT=9555

ENTRYPOINT ["./build/install/opentelemetry-demo-ad/bin/Ad"]