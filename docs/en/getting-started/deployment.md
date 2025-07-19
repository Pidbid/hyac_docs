# Deployment

This guide will walk you through the process of deploying Hyac on your own server.

## Prerequisites

- A server with Docker and Docker Compose installed.
- A domain name (optional, but recommended for production use).

## 1. Clone the Repository

```bash
git clone https://github.com/Pidbid/Hyac.git
cd Hyac
```

## 2. Configure Environment Variables

Copy the `.env.example` file to `.env` and fill in the required values.

```bash
cp .env.example .env
```

## 3. Start the Services

```bash
docker-compose up -d
```

This will start all the required services in the background.

## 4. Start the Frontend

```bash
cd web
pnpm install
pnpm dev
```

## 5. Access the System

- Frontend: `http://localhost:5173`
- Server API Docs: `http://localhost:8000/docs`
