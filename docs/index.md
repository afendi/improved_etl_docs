# Improved ETL Documentation

Welcome to the comprehensive documentation for the Improved ETL system - a modern, scalable document processing and analysis pipeline.

## Quick Links

- [🏗️ Architecture Overview](architecture/overview.md)
- [🚀 Developer Guide](developer/guide.md)
- [📊 API Reference](api/backend-api.md)
- [🔧 Configuration](config/configuration.md)
- [📦 Deployment](deployment/operations.md)

## System Overview

The Improved ETL system is a comprehensive document processing pipeline built with modern technologies:

- **FastAPI** backend services
- **Prefect** workflow orchestration
- **PostgreSQL** with pgvector for data storage
- **Docker** containerized deployment
- **Apache Tika** for document parsing

## Key Features

- ✅ Automated document ingestion and processing
- ✅ Advanced text extraction and chunking
- ✅ Quality evaluation and triage
- ✅ Human review workflows
- ✅ Flexible export capabilities
- ✅ Comprehensive monitoring and health checks

## Getting Started

1. [Set up your development environment](developer/guide.md#local-development-setup)
2. [Understand the architecture](architecture/overview.md)
3. [Explore the API endpoints](api/backend-api.md)
4. [Configure your deployment](config/configuration.md)

## Architecture at a Glance

```mermaid
graph TB
    A[Document Upload] --> B[Ingestor Service]
    B --> C[Text Extraction]
    C --> D[Chunking Process]
    D --> E[Quality Evaluation]
    E --> F[Human Review]
    F --> G[Export & Streaming]