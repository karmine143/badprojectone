# AU Campus Court API

A role-based sports-facility booking REST API for the AU Campus Court project.

## Features

- JWT authentication and Student, Staff, and Administrator RBAC
- Court management, Google Maps navigation links, booking conflict prevention
- Student booking history/cancellation and staff approval workflow
- Admin summary reporting and secured peer availability API using `x-api-key`
- Helmet, CORS, validation, password hashing, Prisma data access

## Run locally

```bash
cp .env.example .env
npm install
npm run db:push
npm run db:seed
npm run dev
```

Open `http://localhost:3000/health`. Demo users are `student@au.edu`, `staff@au.edu`, and `admin@au.edu`; each password is `Password123!`.

## Main endpoints

| Method | Path | Access |
| --- | --- | --- |
| POST | `/api/auth/register`, `/api/auth/login` | Public |
| GET | `/api/courts` | Public |
| POST/PATCH | `/api/courts` | Staff/Admin |
| GET/POST | `/api/bookings` | Signed in |
| PATCH | `/api/bookings/:id/status` | Staff/Admin |
| POST | `/api/bookings/:id/cancel` | Owner/Staff/Admin |
| GET | `/api/reports/summary` | Admin |
| GET | `/api/peer/availability` | `x-api-key` |

## Deploy

Use any Node/Docker host (Render, Railway, Fly.io, or a Linux VPS). Set `DATABASE_URL`, `JWT_SECRET`, `PEER_API_KEY`, and `CORS_ORIGIN` as production environment variables. The included `Dockerfile` runs the API. For a production database, change the Prisma datasource provider to `postgresql`, use a PostgreSQL connection string, then create and apply a Prisma migration.

`render.yaml` provides a Render deployment blueprint. Before publishing, follow [the production checklist](docs/production-checklist.md). The interactive API contract is in [OpenAPI format](docs/openapi.yaml) and can be imported into Swagger Editor or Postman.
