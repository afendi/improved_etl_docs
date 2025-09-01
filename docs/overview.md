# Project Overview

## Technology Stack & Dependencies

The Improved ETL system leverages a modern technology stack designed for scalability and maintainability:

### Core Technologies
- **Python 3.11**: Primary development language
- **FastAPI**: High-performance web framework for APIs
- **PostgreSQL**: Primary database with pgvector extension
- **Prefect**: Workflow orchestration and scheduling
- **Docker**: Containerization and deployment

### Processing Components
- **Apache Tika**: Document parsing and text extraction
- **Ollama**: Local LLM inference for text evaluation
- **NLM-Ingestor**: Advanced text extraction service

### Infrastructure
- **Docker Compose**: Multi-service orchestration
- **Alembic**: Database migrations
- **pytest**: Testing framework

## System Purpose

The Improved ETL system processes documents through a comprehensive pipeline:

1. **Ingestion**: Monitors filesystem for new documents
2. **Extraction**: Converts documents to structured text
3. **Processing**: Chunks text and evaluates quality
4. **Review**: Enables human review and validation
5. **Export**: Provides multiple output formats and streaming

## Key Benefits

- **Scalable**: Microservices architecture supports horizontal scaling
- **Reliable**: Health checks and monitoring throughout the pipeline
- **Flexible**: Configurable processing parameters and workflows
- **Maintainable**: Clean separation of concerns and comprehensive testing

## Target Users

- **Data Engineers**: Building and maintaining ETL pipelines
- **Backend Developers**: Integrating with document processing systems
- **System Administrators**: Deploying and monitoring the infrastructure

## Core Problems Solved

- **Complex ETL Workflows**: Simplified through modular architecture
- **Document Processing**: Automated text extraction and analysis
- **Quality Control**: Built-in evaluation and human review processes
- **Scalability**: Container-based deployment for easy scaling
- **Monitoring**: Comprehensive health checks and status reporting