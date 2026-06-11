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

- Docker
- Docker Compose
- Java 21
- Python 3.12+
- Node.js 22+
- React Native development environment

## Local Services

| Service | Port |
|----------|------|
| API Gateway | 8080 |
| User Service | 8081 |
| Core Service | 8082 |
| AI Service | 8000 |
| User DB | 5433 |
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

## Build Service Images

Build all services before starting the infrastructure.

### User Service

```bash
cd onelife-user-service
docker build -t onelife-user-service:latest .
```

### Core Service

```bash
cd onelife-core-service
docker build -t onelife-core-service:latest .
```

### API Gateway

```bash
cd onelife-api-gateway
docker build -t onelife-api-gateway:latest .
```

### AI Service

```bash
cd onelife-ai-service
docker build -t onelife-ai-service:latest .
```

## Start Infrastructure

```bash
docker compose up -d
```

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