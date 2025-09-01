# Architecture Overview

## System Architecture

The Improved ETL system follows a microservices architecture pattern with containerized components that work together to process documents through a comprehensive pipeline.

## High-Level Architecture

```mermaid
graph TB
    subgraph "External Input"
        A[File System] --> B[Ingestor Service]
    end
    
    subgraph "Processing Layer"
        B --> C[NLM Gateway]
        B --> D[Tika Service]
        C --> E[Pipeline Service]
        D --> E
        E --> F[Worker Service]
    end
    
    subgraph "Orchestration"
        G[Prefect Server] --> F
        F --> H[Processing Flows]
    end
    
    subgraph "Data Layer"
        E --> I[PostgreSQL]
        I --> J[Backend API]
    end
    
    subgraph "User Interface"
        J --> K[Review Interface]
        J --> L[Export API]
    end


## Core Components

### Data Processing Services
- **Ingestor Service**: Monitors filesystem for new documents and triggers processing
- **Pipeline Service**: Core processing logic for chunking and database operations
- **NLM Gateway**: Advanced text extraction and document parsing
- **Tika Service**: Apache Tika-based document parsing engine
- **Worker Service**: Prefect-orchestrated background task execution

### Orchestration & Storage
- **Prefect Server**: Workflow orchestration and scheduling
- **PostgreSQL**: Primary database with pgvector extension for document storage
- **Backend API**: FastAPI-based REST interface for system interaction


## Design Principles

### Microservices Architecture
Each service has a single responsibility and communicates via well-defined APIs:
- **Loose Coupling**: Services can be developed, deployed, and scaled independently
- **High Cohesion**: Related functionality is grouped within service boundaries
- **Service Discovery**: Docker Compose networking enables service-to-service communication

### Event-Driven Processing
- Document ingestion triggers processing workflows
- Asynchronous task execution through Prefect queues
- Status updates propagated through the system

### Containerization
- All services run in Docker containers
- Consistent deployment across environments
- Easy scaling and resource management


## Data Flow

1. **Ingestion**: Files placed in monitored directory trigger ingestor
2. **Extraction**: Documents processed by Tika and NLM Gateway for text extraction
3. **Processing**: Pipeline service chunks text and stores in database
4. **Orchestration**: Prefect manages workflow execution and task scheduling
5. **Review**: Backend API provides interface for human review and validation
6. **Export**: Processed data available through various export formats


## Scalability Considerations

### Horizontal Scaling
- Multiple worker instances can process jobs in parallel
- Pipeline and backend services can be replicated
- Database read replicas for query scaling

### Resource Optimization
- Container resource limits prevent resource contention
- Async processing for I/O-bound operations
- Connection pooling for database efficiency

### Performance Monitoring
- Health checks on all services
- Metrics collection for performance analysis
- Distributed tracing for request flow visibility