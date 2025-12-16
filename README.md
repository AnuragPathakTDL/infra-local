# PocketLOL Microservices

This workspace contains the PocketLOL API Gateway together with the Auth and User services. The stack now ships with a shared Docker Compose workflow so you can run the full slice locally with one command.

## Prerequisites

- Docker 24+ (or Docker Desktop)

## Quick Start

1. From the repository root, build and launch every service plus PostgreSQL and Redis:

   ```bash
   docker compose up --build
   ```

2. The first run seeds the `pocketlol_auth` and `pocketlol_users` databases and applies Prisma migrations automatically.
3. Once the containers report healthy, the endpoints are available at:
   - API Gateway: <http://localhost:3000> (health at `/health/live`)
   - Auth Service: <http://localhost:4000> (JWKS at `/.well-known/jwks.json`)
   - User Service: <http://localhost:4500> (gRPC on `localhost:50052`)

The shared service token defaults to `dev-service-token` and is distributed automatically between containers. Update it in `docker-compose.yml` if you need a different value.

## Data & Migrations

- PostgreSQL runs inside Docker with a persistent `postgres-data` volume.
- Databases `pocketlol_auth` and `pocketlol_users` are created via `docker/postgres/01-init-databases.sql`.
- Each service entrypoint runs `prisma migrate deploy` (or `prisma db push` when no migrations exist) before starting the HTTP server.

## Local Development Outside Docker

You can still run services directly with Node.js. Refer to the service-specific guides:

- [APIGW/README.md](APIGW/README.md)
- [AuthService/README.md](AuthService/README.md)
- [UserService/README.md](UserService/README.md)

Remember to keep the environment variables in `.env` files aligned with your chosen runtime (Docker or bare metal).
