# Payment API & App

A full-stack payment application with ASP.NET Core Web API backend and Angular frontend, containerized with Docker.

## Architecture

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   Angular App   │────▶│   .NET Core API │────▶│   SQL Server    │
│   (Port 3000)   │     │   (Port 5000)   │     │   (External)    │
└─────────────────┘     └─────────────────┘     └─────────────────┘
```

## Tech Stack

- **Frontend**: Angular 16+ with Nginx
- **Backend**: ASP.NET Core 7.0 Web API
- **Database**: SQL Server
- **Containerization**: Docker

## Quick Start

### Prerequisites

- Docker
- Docker Compose (optional)

### Build Images

```bash
# Build API image
docker build --network=host -f Docker/Dockerfile-api -t sousvkeet/payment-api .

# Build App image
docker build --network=host -f Docker/Dockerfile-APP -t sousvkeet/payment-app .
```

### Run Containers

```bash
# Run API (with DNS for external database connectivity)
docker run -d -p 5000:5000 --dns 8.8.8.8 --name payment-api sousvkeet/payment-api

# Run App
docker run -d -p 3000:80 --name payment-app sousvkeet/payment-app
```

### Access the Application

- **Frontend**: http://localhost:3000
- **API**: http://localhost:5000/api/PaymentDetail

## Project Structure

```
api-node/
├── Docker/
│   ├── Dockerfile-api      # API container configuration
│   ├── Dockerfile-APP      # Frontend container configuration
│   └── nginx.conf          # Nginx configuration for Angular
├── PaymentAPI/
│   └── PaymentAPI/         # .NET Core Web API source
└── PaymentApp/             # Angular frontend source
```

## Configuration

### API Configuration

Edit `PaymentAPI/PaymentAPI/appsettings.json`:

```json
{
  "ConnectionStrings": {
    "DevConnection": "your-connection-string"
  },
  "Cors": {
    "AllowedOrigins": ["http://localhost:3000"]
  }
}
```

### Docker Image Sizes

| Image | Size |
|-------|------|
| payment-app | ~62 MB |
| payment-api | ~220 MB |

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/PaymentDetail` | Get all payment records |
| GET | `/api/PaymentDetail/{id}` | Get payment by ID |
| POST | `/api/PaymentDetail` | Create new payment |
| PUT | `/api/PaymentDetail/{id}` | Update payment |
| DELETE | `/api/PaymentDetail/{id}` | Delete payment |

## Troubleshooting

### CORS Errors
Ensure the frontend origin is added to `appsettings.json` under `Cors.AllowedOrigins`.

### Database Connection Issues
When running without `--network=host`, add `--dns 8.8.8.8` to enable external DNS resolution.

### WSL2 Port Access
If ports aren't accessible from Windows, try accessing via `127.0.0.1` instead of `localhost`.
