# OneLife Infrastructure

Local development environment for the OneLife platform.

## Architecture

```text
React Native App
        |
        v
API Gateway (Spring Cloud Gateway)
        |
        |--------------------------|
        v                          v
User Service                 Core Service
(Spring Boot)                (Spring Boot)
                                    |
                                    v
                             AI Service
                             (Django)
```

## Repositories

- onelife-mobile-app
- onelife-api-gateway
- onelife-user-service
- onelife-core-service
- onelife-ai-service
- onelife-infra

## Prerequisites

- Docker with Docker Compose v2.23 or newer, with the Docker daemon running.
- Clone the service repositories alongside this infrastructure repository:

```text
onelife-platform/
├── onelife-infra/
├── onelife-api-gateway/
├── onelife-user-service/
└── onelife-core-service/
```

Docker builds the Java applications using their Gradle wrappers; no local Java or
Gradle installation is required. The first build needs internet access to download
base images, Gradle, and dependencies. User service and gateway use Java 21; core
uses Java 26, matching each project's Gradle toolchain.

## Local Services

| Service | Port |
|----------|------|
| API Gateway | 8080 |
| User Service | 8081 |
| Core Service | 8082 |
| AI Service | 8000 |
| User DB | 5432 |
| Core DB | 5434 |
| AI DB | 5435 |
| pgAdmin | 5050 |

## Environment Variables

Create a `.env` file:

```env
USER_SERVICE_PORT=8081
CORE_SERVICE_PORT=8082
AI_SERVICE_PORT=8000
API_GATEWAY_PORT=8080

POSTGRES_USER=onelife
POSTGRES_PASSWORD=onelife_password

POSTGRES_USER_DB=onelife_user_db
POSTGRES_CORE_DB=onelife_core_db
POSTGRES_AI_DB=onelife_ai_db
```

## Build and Start

From `onelife-infra`, after creating `.env` as described above:

```bash
docker compose up -d --build
```

Compose builds the gateway, user service, and core service from the sibling
repositories. Their `pull_policy: build` also makes a plain `docker compose up -d`
build locally instead of trying to pull private OneLife images from Docker Hub.
The user and core services wait for their databases to become healthy.

### Optional AI service

AI is disabled by default because its source is not included in this workspace.
To enable it, clone `onelife-ai-service` alongside the other repositories and ensure
it includes a Dockerfile that starts the application on `0.0.0.0:8000`. Then run:

```bash
docker compose --profile ai up -d --build
```

This also starts the AI database. AI gateway routes require this profile and a
working AI implementation. To stop the full stack including AI, run
`docker compose --profile ai down`.

Check running containers:

```bash
docker ps
```

## Stop Infrastructure

```bash
docker compose down
```

To remove databases:

```bash
docker compose down -v
```

## Access URLs

### API Gateway

```text
http://localhost:8080
```

### pgAdmin

```text
http://localhost:5050
```

Credentials:

```text
Email: admin@onelife.local
Password: admin
```

## Service Communication

### Gateway → User Service

```text
/api/v1/auth/**
/api/v1/users/**
```

### Gateway → Core Service

```text
/api/v1/dashboard/**
/api/v1/accounts/**
/api/v1/wealth/**
```

### Gateway → AI Service

```text
/api/v1/ai/**
```

## MVP Scope

### User Service

Responsibilities:

- Authentication
- Authorization
- User profile
- Preferences
- Consents
- Subscription management

### Core Service

Responsibilities:

- Financial account aggregation
- Wealth calculation
- Dashboard generation
- Asset allocation
- Liabilities
- Cashflow analysis

### AI Service

Responsibilities:

- Financial recommendations
- Portfolio insights
- Spending analysis
- Tax optimization suggestions
- AI CFO chat

## Future Improvements

- Kubernetes
- Helm Charts
- Kafka
- Redis
- OpenTelemetry
- Prometheus
- Grafana
- Keycloak
- CI/CD Pipelines
- Multi-region deployment