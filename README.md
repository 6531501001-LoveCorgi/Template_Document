# Mae Fah Luang Template Document Generator

Mae Fah Luang Template Document Generator is a full-stack web application for creating reusable document and report layouts from structured JSON data. It is designed for Mae Fah Luang University teams that need to turn operational datasets into consistent, previewable reports without rebuilding tables, charts, and document structure for every use case.

The application supports a practical template workflow: upload a JSON data file, select an approved template, validate the template against the uploaded fields, configure table and chart sections, and preview the final report before using or saving the template. This makes the system suitable for internal university reporting, administrative dashboards, departmental summaries, and repeatable document generation workflows.

The core product module lives in `frontend/src/views/templates`, including the upload area, template selector, template builder modal, table configuration, chart configuration, reusable data table renderer, chart renderer, and document preview screen.

## Product Purpose

- Standardize recurring Mae Fah Luang University report formats
- Reduce manual report layout work across departments and administrative units
- Let teams reuse approved templates with different datasets
- Provide a visual preview before a generated document or report is used
- Support tables and charts in the same configurable document layout
- Keep template definitions structured so they can be stored, reviewed, and reused

## Features

- Drag-and-drop JSON file upload for report data
- Template selector with search, sorting, pagination, active/inactive status, and field mismatch warning
- Template builder modal for editing template metadata, document metadata, data source, tables, and charts
- Table manager for selecting JSON fields, renaming column labels, adding multiple table sections, and previewing table output
- Chart manager for configuring bar, line, radar, pie, doughnut, and polar-area charts from numeric fields
- Live preview while building a template
- Document preview page that renders configured tables and charts against uploaded data
- Reusable generic data table with filtering, sorting, and pagination
- Express/MongoDB backend model and REST API for template persistence
- Settings APIs for reusable messages, status values, and verification records
- Swagger UI for API inspection at `/api-docs`
- Docker support for frontend, backend, MongoDB, and mongo-express

## Tech Stack

| Layer | Technology |
| --- | --- |
| Frontend | Vue 2, Vue Router 3, Vuex 3, CoreUI Pro Vue, CoreUI chart components |
| Backend | Node.js, Express, Mongoose, Socket.IO |
| Database | MongoDB 6 |
| API Docs | Swagger UI |
| Build/Runtime | npm, pnpm, Docker, Docker Compose, Nginx |
| Testing | Jest, Vue Test Utils, Nightwatch |

## Repository Structure

```text
.
|-- backend/
|   |-- config/                 # Express, CORS, rate limit, logger, runtime config
|   |-- helpers/                # Mongo initialization, Redis helper, base service utilities
|   |-- middleware/             # Express middleware stack
|   |-- server/
|   |   |-- Project/
|   |   |   |-- Settings/       # Message, status, and verification APIs
|   |   |   `-- Templates/      # Template model, routes, controller, and service
|   |   |-- routes/             # API route registration and socket wiring
|   |   `-- swagger/            # Swagger JSON definitions
|   |-- Dockerfile
|   |-- docker-compose.yml
|   `-- server.js
|-- frontend/
|   |-- public/                 # Static assets, icons, ML model files, manifest
|   |-- src/
|   |   |-- assets/             # Images, icons, SCSS, fonts
|   |   |-- containers/         # CoreUI layout containers
|   |   |-- projects/           # Project-specific screens and reusable components
|   |   |-- service/            # Axios API client and Socket.IO client
|   |   |-- store/              # Vuex modules
|   |   `-- views/
|   |       `-- templates/      # Upload, selector, builder, tables, charts, preview
|   |-- Dockerfile
|   |-- docker-compose.yml
|   `-- package.json
`-- README.md
```

## Prerequisites

- Node.js 18 or later for the backend
- Node.js 20 or later for the frontend Docker build
- npm 9 or later
- pnpm for backend Docker/runtime parity
- MongoDB 6, or Docker with Docker Compose

The frontend is based on Vue CLI 4 and Vue 2. Some dependencies are older and may require `--legacy-peer-deps` during installation.

## Environment Variables

Create `backend/.env` for local backend development:

```env
NODE_ENV=development
PORT=8081

KEY=
MONGODB=mongodb://127.0.0.1:27017/Centers?authSource=admin

TIMEOUT=500000
TOKENLANGTH=32
TOKENEXPIRED=30
TRANSACTIONEXPIRED=10

SMTP_HOST=smtp.gmail.com
SMTP_PORT=465
SMTP_USER=
SMTP_PASS=
```

Create `frontend/.env` for local frontend development:

```env
VUE_APP_TITLE=Template Document
VUE_APP_API_URL=http://127.0.0.1:8081
VUE_APP_VERSION=1.0.0

VUE_APP_CLIENTID=
VUE_APP_SCOPE=profile email
VUE_APP_PROMPT=select_account
```

Do not commit real secrets, SMTP credentials, OAuth client IDs, database credentials, or production connection strings.

## Getting Started

### Backend

```bash
cd backend
pnpm install
pnpm start
```

The API runs on the port defined by `PORT` in `backend/.env`. With the example configuration, the backend is available at:

- API: `http://127.0.0.1:8081`
- Health check: `http://127.0.0.1:8081/healthz`
- Swagger UI: `http://127.0.0.1:8081/api-docs`

### Frontend

```bash
cd frontend
npm install --legacy-peer-deps
npm run serve
```

Vue CLI will print the local development URL, commonly `http://localhost:8080`.

### Docker

Backend, MongoDB, and mongo-express:

```bash
cd backend
docker compose up --build
```

Default service URLs:

- Backend API: `http://localhost:8082`
- MongoDB: `localhost:27017`
- mongo-express: `http://localhost:8083`

Frontend production build served by Nginx:

```bash
cd frontend
docker compose up --build
```

Default frontend URL:

- Frontend: `http://localhost:8080`

## API Overview

All application routes are mounted under `/api/v1`.

### Templates

| Method | Endpoint | Description |
| --- | --- | --- |
| `GET` | `/api/v1/templates` | List templates |
| `GET` | `/api/v1/templates/:id` | Read one template |
| `POST` | `/api/v1/templates` | Create a template |
| `PUT` | `/api/v1/templates` | Update a template |
| `DELETE` | `/api/v1/templates` | Delete templates matching the request query/body handled by the service |

### Settings

| Method | Endpoint | Description |
| --- | --- | --- |
| `GET` | `/api/v1/setting/message` | List message records |
| `POST` | `/api/v1/setting/message` | Create a message record |
| `PUT` | `/api/v1/setting/message` | Update a message record |
| `DELETE` | `/api/v1/setting/message` | Delete message records |
| `GET` | `/api/v1/setting/status` | List status records |
| `POST` | `/api/v1/setting/status` | Create a status record |
| `PUT` | `/api/v1/setting/status` | Update a status record |
| `DELETE` | `/api/v1/setting/status` | Delete status records |
| `GET` | `/api/v1/setting/verification` | List verification records |
| `POST` | `/api/v1/setting/verification` | Create a verification record |
| `PUT` | `/api/v1/setting/verification` | Update a verification record |
| `DELETE` | `/api/v1/setting/verification` | Delete verification records |

Swagger definitions are loaded from `backend/server/swagger/app.json` and `backend/server/swagger/app1.json`.

## Template Data Model

Templates are represented in the frontend and backend with the following high-level structure. The frontend currently demonstrates template selection with local mock data, while the backend provides a MongoDB model and REST routes for persistence.

```js
{
  templateMeta: {
    name: String,
    description: String,
    ownerDepartment: [String],
    status: Boolean
  },
  documentMeta: {
    name: String,
    description: String,
    dataSource: String
  },
  layout: {
    tables: [
      {
        name: String,
        fields: [{ key: String, label: String }]
      }
    ],
    charts: [
      {
        name: String,
        type: "bar" | "line" | "radar" | "pie" | "doughnut" | "polarArea",
        labelKey: String,
        valueKeys: [String],
        valueKey: String,
        colors: [String],
        style: String
      }
    ]
  }
}
```

## Development Commands

Backend:

```bash
cd backend
pnpm start
pnpm run serve:test
pnpm run serve:prod
```

Frontend:

```bash
cd frontend
npm run serve
npm run build
npm run lint
npm run test:unit
npm run test:e2e
npm run release
```

## Testing

The frontend includes Jest unit tests and Nightwatch end-to-end tests inherited from the Vue CLI/CoreUI setup.

```bash
cd frontend
npm run test:unit
npm run test:e2e
```

The backend currently has a placeholder `npm test` script. Add endpoint, service, and model tests before relying on automated backend validation in CI.

## Production Notes

- Set `NODE_ENV=production` and provide a production MongoDB connection string.
- Restrict CORS origins before exposing the API publicly.
- Replace development tokens and placeholder keys with managed secrets.
- Serve the frontend through HTTPS in production.
- Review the static `X-Access-Token` behavior in the frontend API client before deployment.
- Keep MongoDB behind the application network; avoid exposing it directly to the public internet.
- Add authentication and authorization middleware around management APIs if this dashboard is used beyond a trusted internal network.

## License

The backend package is marked as ISC. The frontend package is based on CoreUI Pro Vue and references the CoreUI Pro license. Review third-party license obligations before publishing or distributing this project.
