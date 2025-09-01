# Backend API Reference

## Overview

The Backend API provides REST endpoints for managing the ETL system, accessing processed data, and monitoring system health. Built with FastAPI, it offers automatic OpenAPI documentation and high-performance async operations.

**Base URL:** `http://localhost:8000`  
**API Documentation:** `http://localhost:8000/docs`  
**OpenAPI Schema:** `http://localhost:8000/openapi.json`

## Authentication

The API supports JWT-based authentication. In development mode, authentication can be bypassed using the [DEV_BYPASS_JWT](improved_etl\pipeline\api\main.py#L131-L131) environment variable.

### Getting a Token

POST /auth/token
Content-Type: application/json

{
"username": "your_username",
"password": "your_password"
}


### Using the Token
Include the JWT token in the Authorization header:
Authorization: Bearer <your_jwt_token>

## Health API

### Check System Health
Get overall system health status and dependency checks.

**Endpoint:** `GET /health`

**Response:**
```json
{
  "status": "healthy",
  "timestamp": "2025-01-01T12:00:00Z",
  "dependencies": {
    "database": "healthy",
    "tika_service": "healthy",
    "nlm_gateway": "healthy"
  },
  "version": "1.0.0"
}
```bash
curl http://localhost:8000/health
```

### Database Health Check
Check PostgreSQL database connectivity and status.

Endpoint: GET /health/db

**Response:**
```json
{
  "status": "healthy",
  "connection_pool": {
    "active": 2,
    "idle": 8,
    "total": 10
  },
  "response_time_ms": 15
}

```bash
curl http://localhost:8000/health/db
```

## Review API

### Get Review Queue
Retrieve documents pending human review.

**Endpoint:** `GET /review/queue`

**Query Parameters:**
- `limit` (optional): Number of items to return (default: 50)
- `offset` (optional): Number of items to skip (default: 0)
- `status` (optional): Filter by review status

**Response:**
```json
{
  "items": [
    {
      "document_id": "doc_123",
      "filename": "report.pdf",
      "status": "pending_review",
      "created_at": "2025-01-01T10:00:00Z",
      "priority": "normal",
      "chunks_count": 15
    }
  ],
  "total": 25,
  "limit": 50,
  "offset": 0
}

```bash
curl "http://localhost:8000/review/queue?limit=10&status=pending_review"
```
### Submit Review Decision
Submit a review decision for a document or chunk.

**Endpoint**: `POST/review/decision`
**Request Body**:
```json
{
  "document_id": "doc_123",
  "chunk_id": "chunk_456",
  "decision": "approve",
  "reviewer": "user@example.com",
  "notes": "Content looks good, approved for processing"
}
```
**Response**:
```json
{
  "success": true,
  "decision_id": "decision_789",
  "timestamp": "2025-01-01T12:00:00Z"
}
```
**Example**
```bash
curl -X POST http://localhost:8000/review/decision \
  -H "Content-Type: application/json" \
  -d '{"document_id": "doc_123", "decision": "approve", "reviewer": "user@example.com"}'
```


## Exports API

### Get Available Exports
Retrieve list of available data exports.

**Endpoint:** `GET/exports`

**Query Parameters:**
- [format](file://\\wsl.localhost\Ubuntu\home\afendi\projects\improved_etl\backend\app\main.py#L21-L35) (optional): Filter by export format (jsonl, csv, xml)
- `status` (optional): Filter by export status (ready, processing, failed)

**Response:**
```json
{
  "exports": [
    {
      "export_id": "export_123",
      "format": "jsonl",
      "status": "ready",
      "created_at": "2025-01-01T10:00:00Z",
      "file_size": 1024000,
      "record_count": 1500,
      "download_url": "/exports/export_123/download"
    }
  ],
  "total": 10
}
```
```bash
curl "http://localhost:8000/exports?format=jsonl&status=ready"
```
### Create New Export
Request a new data export.

**Endpoint**: `POST/exports`

**Request Body**:
```json
{
  "format": "jsonl",
  "filters": {
    "date_from": "2025-01-01",
    "date_to": "2025-01-31",
    "status": "approved"
  },
  "include_chunks": true,
  "include_metadata": true
}
```
**Response**
```json
{
  "export_id": "export_456",
  "status": "processing",
  "estimated_completion": "2025-01-01T12:30:00Z"
}
```
**Example**
```bash
curl -X POST http://localhost:8000/exports \
  -H "Content-Type: application/json" \
  -d '{"format": "jsonl", "include_chunks": true}'
```
### Download Export
Download a completed export file.

**Endpoint:** `GET /exports/{export_id}/download`

**Response**: File download (Content-Type varies by format)

**Example**:

```bash
curl -O http://localhost:8000/exports/export_123/download
```


## Stats API

### Get System Statistics
Retrieve system-wide statistics and metrics.

**Endpoint:** `GET /stats`

**Response:**
```json
{
  "documents": {
    "total": 1250,
    "processed": 1100,
    "pending": 150,
    "failed": 0
  },
  "chunks": {
    "total": 18750,
    "approved": 16200,
    "pending_review": 2400,
    "rejected": 150
  },
  "processing": {
    "avg_processing_time_seconds": 45.2,
    "success_rate": 0.98,
    "throughput_per_hour": 120
  }
}
```
```bash
curl http://localhost:8000/stats
```
## Chunks API
### Get Chunks
Retrieve processed document chunks.

**Endpoint**: `GET /chunks`

**Query Parameters**:
```
document_id (optional): Filter by document ID
status (optional): Filter by chunk status
limit (optional): Number of chunks to return (default: 100)
offset (optional): Number of chunks to skip (default: 0)
```
**Response**:
```json
{
  "chunks": [
    {
      "chunk_id": "chunk_789",
      "document_id": "doc_123",
      "content": "This is the extracted text content...",
      "position": 1,
      "quality_score": 0.95,
      "status": "approved",
      "created_at": "2025-01-01T10:00:00Z"
    }
  ],
  "total": 500,
  "limit": 100,
  "offset": 0
}
```
```bash
curl "http://localhost:8000/chunks?document_id=doc_123&limit=50"
```
### Get Chunk Details
Retrieve detailed information about a specific chunk.

**Endpoint**: `GET /chunks/{chunk_id}`

**Response**:
```json
{
  "chunk_id": "chunk_789",
  "document_id": "doc_123",
  "content": "Full text content of the chunk...",
  "metadata": {
    "word_count": 125,
    "character_count": 890,
    "language": "en",
    "confidence": 0.98
  },
  "quality_metrics": {
    "readability_score": 0.85,
    "coherence_score": 0.92,
    "relevance_score": 0.88
  }
}
```
**Example**:
```bash
curl http://localhost:8000/chunks/chunk_789
```
### Error Handling
All API endpoints return standard HTTP status codes and JSON error responses:

**Success Codes**:

- 200 OK: Request successful
- 201 Created: Resource created successfully
- 204 No Content: Successful request with no content

**Error Codes**:

- 400 Bad Request: Invalid request parameters
- 401 Unauthorized: Authentication required
- 403 Forbidden: Insufficient permissions
- Not Found: Resource not found
- 422 Unprocessable Entity: Validation errors
- Internal Server Error: Server error

**Error Response Format:***

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid request parameters",
    "details": {
      "field": "document_id",
      "issue": "required field missing"
    }
  }
}
```
