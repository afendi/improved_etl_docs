# Developer Guide

## Prerequisites

Before setting up the Improved ETL system for development, ensure you have the following tools installed:

### Required Tools
- **Docker**: Version 20.0+ for containerization
- **Docker Compose**: Version 2.0+ for multi-container orchestration  
- **Make**: Build automation tool (usually pre-installed on Linux/macOS)
- **Git**: Version control system
- **Python**: 3.10+ for local development (optional)

### Optional Tools
- **jq**: JSON processor for API testing and data manipulation
- **curl**: Command-line HTTP client for API testing
- **PostgreSQL client**: For direct database access and debugging

### System Requirements
- **Memory**: Minimum 8GB RAM (16GB recommended)
- **Storage**: At least 10GB free disk space
- **Network**: Internet access for downloading Docker images
- **Ports**: Ensure ports 5432, 8000, 8010, 9998, 4200, 11434, 5001 are available


## Quick Start

### 1. Clone the Repository
- git clone <repository-url>
- cd improved_etl

### 2. Environment Setup
- Create the required environment file:
- cp env.example .env
- Create the storage directory:
- mkdir -p storage/raw_inbox

### 3. Start Development Environment
- Start all services: make dev-up
- Wait for services to be ready: make dev-wait
- Verify services are running: make ps

### 4. Verify Installation
Check that all services are healthy:

Check backend API: curl http://localhost:8000/health

Check pipeline API: curl http://localhost:8010/health

Check Prefect UI (open in browser): http://localhost:4200

## Development Commands

### Common Make Commands
Start all services:
make dev-up

Stop all services:
make dev-down

Restart services after code changes:
make dev-restart

View service status:
make ps

View service logs:
make 

### Database Operations

Run database migrations:
make migrate


Seed the database with test data:
make seed


### Database Operations

Run database migrations: make migrate

Seed the database with test data: make seed

Access PostgreSQL directly: make db-shell

## Testing

### Running Tests
Run all tests: make test


Run backend tests only: make test-backend


Run pipeline tests only: make test-pipeline


### Test Coverage
Generate test coverage report: make coverage

### Common Development Tasks

Test document processing: Copy a sample document to the inbox
cp sample.pdf storage/raw_inbox/

Monitor logs to see processing
make logs


Access API documentation:
Backend API docs
http://localhost:8000/docs

Pipeline API docs
http://localhost:8010/docs


## Troubleshooting

### Service Won't Start
Check Docker daemon is running and ports are available:
docker ps
netstat -tlnp | grep :8000


### Database Connection Issues
Verify PostgreSQL is healthy:
make db-shell


### View Service Logs
Check specific service logs:
docker compose -f docker/docker-compose.yml logs backend
docker compose -f docker/docker-compose.yml logs pipeline
