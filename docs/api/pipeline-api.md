# Pipeline API Reference

## Overview

The Pipeline API provides endpoints for document processing operations, chunking, and database management. This service handles the core ETL processing workflows and integrates with text extraction services.

**Base URL:** `http://localhost:8010`  
**API Documentation:** `http://localhost:8010/docs`  
**OpenAPI Schema:** `http://localhost:8010/openapi.json`

## Authentication

The Pipeline API uses the same JWT-based authentication as the Backend API. In development mode, authentication can be bypassed using environment configuration.

### Using Authentication
Include the JWT token in the Authorization header: Authorization: Bearer <yourjwttoken>

## Health and Status

### Check Pipeline Health
Get pipeline service health status and processing capabilities.

**Endpoint:** `GET /health`

**Response:**
```json
{
  "status": "healthy",
  "timestamp": "2025-01-01T12:00:00Z",
  "dependencies": {
    "database": "healthy",
    "tika_service": "healthy",
    "nlm_gateway": "healthy",
    "storage": "healthy"
  },
  "processing": {
    "active_jobs": 3,
    "queue_size": 12,
    "throughput_per_minute": 8.5
  }
}
```
```bash
curl http://localhost:8010/health
```
