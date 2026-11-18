# DasaMode

DasaMode is a portfolio project for a general-purpose Canadian e-commerce store designed to support CAD pricing and Ontario HST. It uses Angular 22 with server-side rendering, ASP.NET Core on .NET 10, and SQL Server 2022. The project is still in its early stages.

## Tech stack

| Status | Technologies |
| --- | --- |
| In the repository | Angular 22, TypeScript, and SSR. ASP.NET Core on .NET 10 with OpenAPI. SQL Server 2022 in Docker Compose. xUnit and Vitest test scaffolding. |
| Planned | EF Core for data access, ASP.NET Core Identity and JWT for authentication, Stripe for payments, and Testcontainers for integration tests. |

## Status / Roadmap

1. **Stage 0 — Foundation (in progress).** The solution, Angular client, and local SQL Server setup are in place. CI setup is pending.
2. **Stage 1 — Catalog (planned).** Add EF Core persistence, product APIs, and storefront browsing.
3. **Stage 2 — Accounts and cart (planned).** Add Identity, JWT authentication, and cart management.
4. **Stage 3 — Checkout and orders (planned).** Add CAD checkout, Ontario HST calculation, and Stripe payments.
5. **Stage 4 — Integration testing (planned).** Add Testcontainers coverage for database-backed workflows.

## Project structure

| Path | Purpose |
| --- | --- |
| `client/` | Angular 22 and TypeScript client with SSR |
| `src/DasaMode.Api/` | ASP.NET Core API host with development OpenAPI support |
| `src/DasaMode.Application/` | Application layer for use cases |
| `src/DasaMode.Domain/` | Domain layer for business rules and models |
| `src/DasaMode.Infrastructure/` | Infrastructure layer for data access and external services |
| `tests/` | xUnit unit and integration test projects with starter tests |
| `docker-compose.yml` | Local SQL Server 2022 container with a persistent named volume |

## Prerequisites

- .NET 10 SDK. The repository requests version `10.0.403` in `global.json` and allows roll-forward to a later .NET 10 feature band.
- Node.js 24.x (24.15.0 or later, per the Angular 22 dependency requirements) and npm 11.
- Docker with the Compose plugin if you want to start the local database.

## Run locally

Clone the repository.

~~~bash
git clone https://github.com/ramindoru-netizen/dasamode.git
cd dasamode
~~~

### Start the API

From the repository root, trust the local development certificate and start the HTTPS profile.

~~~bash
dotnet dev-certs https --trust
dotnet run --project src/DasaMode.Api --launch-profile https
~~~

The development OpenAPI document is available at [https://localhost:7080/openapi/v1.json](https://localhost:7080/openapi/v1.json). It currently has no application endpoints.

### Start the Angular client

Open a second terminal at the repository root and run the following commands.

~~~bash
cd client
npm ci
npm start
~~~

Open [http://localhost:4200](http://localhost:4200).

### Start SQL Server (optional)

Copy `.env.example` to `.env` beside `docker-compose.yml` and replace the example `MSSQL_SA_PASSWORD` with a strong local password. Git ignores `.env`.

~~~bash
docker compose up -d sqlserver
~~~

Connect to `localhost,14333` with the `sa` login and the password you set. The API can currently run independently of the database. To stop the database and keep its data, run `docker compose stop sqlserver`.

## Build and test

From the repository root, build and test the .NET solution.

~~~bash
dotnet build DasaMode.slnx
dotnet test DasaMode.slnx
~~~

From `client/`, build the Angular app, run its tests once, and serve the SSR build locally.

~~~bash
npm run build
npm test -- --watch=false
npm run serve:ssr:client
~~~

The SSR server defaults to [http://localhost:4000](http://localhost:4000).

## Author

[Ramin Dorudian](https://github.com/ramindoru-netizen)
