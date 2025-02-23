FROM eclipse-temurin:21@sha256:e538e34d1df871c9b7da571582cdc49538f1eaee1dacbfb317a3e7f54abeebae AS builder

WORKDIR /app

COPY ["build.gradle", "gradlew", "./"]
COPY gradle gradle
RUN chmod +x gradlew
RUN ./gradlew downloadRepos

COPY . .
RUN chmod +x gradlew
RUN ./gradlew installDist

FROM eclipse-temurin:21.0.4_7-jre-alpine@sha256:8cc1202a100e72f6e91bf05ab274b373a5def789ab6d9e3e293a61236662ac27

WORKDIR /app
COPY --from=builder /app .

RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

EXPOSE 9555

HEALTHCHECK --interval=30s --timeout=10s --retries=3 CMD curl --fail http://localhost:9555/ || exit 1

ENTRYPOINT ["/app/build/install/hipstershop/bin/AdService"]

