# Service Components

## Backend Service (FastAPI Gateway)

### Purpose
The Backend Service serves as the central API gateway providing REST endpoints for system interaction, data retrieval, and administrative functions.

### Key Features
- **FastAPI Framework**: High-performance async web framework
- **JWT Authentication**: Secure token-based authentication system
- **Database Integration**: SQLAlchemy ORM with PostgreSQL
- **Health Monitoring**: Built-in health check endpoints
- **API Documentation**: Auto-generated OpenAPI/Swagger documentation

### Core Endpoints
- **Health API** (`/health`): System status and dependency checks
- **Review API** (`/review`): Human review workflow management
- **Exports API** (`/exports`): Data export functionality
- **Stats API** (`/stats`): System statistics and metrics
- **Chunks API** (`/chunks`): Document chunk management

### Technology Stack
- Python 3.11 with FastAPI
- Uvicorn ASGI server
- SQLAlchemy ORM v2.0+
- Pydantic for data validation
- Python-jose for JWT handling

### Configuration
- **Port**: 8000 (configurable via environment)
- **Database**: PostgreSQL connection via DATABASE_URL
- **Authentication**: JWT secret and algorithm configuration
- **CORS**: Configurable cross-origin resource sharing


## Pipeline Service (Processing API)

### Purpose
Core document processing service responsible for text chunking, quality analysis, and database persistence operations.

### Key Features
- **Document Processing**: Text chunking and metadata extraction
- **Database Operations**: CRUD operations for documents and chunks
- **Quality Analysis**: Content evaluation and scoring
- **Async Processing**: Non-blocking I/O operations
- **Health Monitoring**: Service readiness and dependency checks

### Core Functions
- **Chunking Engine**: Intelligent text segmentation
- **Metadata Extraction**: Document properties and statistics
- **Database Persistence**: Document and chunk storage
- **Quality Scoring**: Content evaluation metrics
- **Batch Processing**: Efficient bulk operations

### Technology Stack
- Python 3.11 with FastAPI
- Custom chunking algorithms
- PostgreSQL with SQLAlchemy
- Async/await patterns
- libmagic for file type detection

### Configuration
- **Port**: 8010 (configurable)
- **Database**: Shared PostgreSQL instance
- **Storage**: File system access for document storage
- **Processing**: Configurable chunk size and overlap parameters


## Ingestor Service (Filesystem Watcher)

### Purpose
Monitors designated directories for new documents and triggers the processing pipeline when files are detected.

### Key Features
- **Filesystem Monitoring**: Real-time file system event detection
- **File Validation**: Format and size validation before processing
- **Pipeline Triggering**: Initiates processing workflows via Prefect
- **Error Handling**: Robust error handling and retry mechanisms
- **Logging**: Comprehensive activity logging and monitoring

### Core Functions
- **Directory Watching**: Monitors inbox directories for new files
- **File Processing**: Initial file validation and metadata collection
- **Workflow Triggering**: Starts Prefect flows for document processing
- **Status Tracking**: Maintains processing status and history
- **Cleanup Operations**: Manages processed file cleanup

### Technology Stack
- Python 3.11
- Watchdog library for filesystem monitoring
- Prefect client for workflow triggering
- File system utilities
- Logging and monitoring tools

### Configuration
- **INBOX_PATH**: Directory to monitor for new documents
- **STORAGE_ROOT**: Base directory for file storage
- **File Patterns**: Configurable file type filters
- **Processing Rules**: Validation and routing logic


## NLM Gateway Service (Text Extraction Router)

### Purpose
Advanced text extraction service that routes documents to appropriate parsing engines and provides enhanced text extraction capabilities.

### Key Features
- **Smart Routing**: Document type detection and routing
- **Multiple Parsers**: Integration with various text extraction engines
- **Quality Enhancement**: Improved text extraction accuracy
- **Format Support**: Wide range of document format support
- **API Integration**: RESTful interface for extraction requests

### Core Functions
- **Document Analysis**: Format detection and classification
- **Text Extraction**: High-quality text extraction from documents
- **Metadata Parsing**: Document properties and structure analysis
- **Content Cleanup**: Text normalization and cleanup
- **API Endpoints**: RESTful extraction services

### Technology Stack
- Python-based extraction engine
- Document parsing libraries
- REST API interface
- Multi-format support
- Error handling and logging

### Configuration
- **Port**: 5001
- **Supported Formats**: PDF, DOCX, TXT, HTML, and more
- **API Endpoints**: `/extract`, `/health`, `/formats`
- **Processing Limits**: Configurable file size and timeout limits

## Tika Service (Document Parsing Engine)

### Purpose
Apache Tika-based document parsing service providing comprehensive document analysis and text extraction capabilities.

### Key Features
- **Apache Tika Integration**: Industry-standard document parsing
- **Format Detection**: Automatic document type identification
- **Metadata Extraction**: Comprehensive document metadata
- **Text Extraction**: Clean text extraction from various formats
- **OCR Support**: Optical character recognition for scanned documents

### Core Functions
- **Document Parsing**: Extract text and metadata from documents
- **Format Support**: Over 1000+ supported file formats
- **Content Analysis**: Document structure and content analysis
- **Metadata Harvesting**: Author, creation date, properties extraction
- **Binary Analysis**: File type detection and validation

### Technology Stack
- Apache Tika server
- Java-based parsing engine
- REST API interface
- OCR libraries (Tesseract integration)
- Custom configuration support

### Configuration
- **Port**: 9998
- **Memory**: Configurable JVM heap settings
- **Timeout**: Request timeout configuration
- **OCR**: Optional OCR engine integration
- **Custom Config**: Tika configuration file support


## Worker Service (Prefect Orchestration)

### Purpose
Prefect-orchestrated background worker service responsible for executing data processing workflows and managing task queues.

### Key Features
- **Prefect Integration**: Workflow orchestration and task management
- **Queue Management**: Work queue creation and task distribution
- **Flow Execution**: Background execution of processing workflows
- **Task Monitoring**: Real-time task status and progress tracking
- **Error Handling**: Robust error handling and retry mechanisms

### Core Functions
- **Workflow Execution**: Run ingest, evaluate, and embeddings flows
- **Task Scheduling**: Schedule and manage background tasks
- **Queue Processing**: Process work queues and task distribution
- **Status Reporting**: Report task status and completion
- **Resource Management**: Manage worker resources and scaling

### Supported Workflows
- **Ingest Flow**: Document ingestion and initial processing
- **Evaluate Flow**: Quality evaluation and content analysis
- **Embeddings Flow**: Vector embedding generation and storage
- **Custom Flows**: Extensible workflow definitions

### Technology Stack
- Python 3.11 with Prefect 2.x
- Work pool and queue management
- Async task execution
- PostgreSQL for state storage
- Docker container orchestration

### Configuration
- **PREFECT_API_URL**: Prefect server endpoint
- **PREFECT_POOL**: Worker pool configuration
- **Work Queues**: Queue definitions and routing
- **Resource Limits**: Memory and CPU constraints
- **Retry Settings**: Error handling and retry policies

## Service Dependencies

### Inter-Service Communication
- **Backend API** ↔ **PostgreSQL**: Database operations and queries
- **Pipeline Service** ↔ **PostgreSQL**: Document and chunk storage
- **Ingestor Service** → **Worker Service**: Workflow triggering
- **Worker Service** → **Pipeline Service**: Processing execution
- **Pipeline Service** → **Tika/NLM Gateway**: Text extraction

### Health Check Dependencies
All services implement health checks that verify:
- **Database connectivity**: PostgreSQL availability
- **Service readiness**: Internal service health
- **Dependency status**: Upstream service availability
- **Resource availability**: Memory, disk, and network status

### Startup Order
1. **PostgreSQL**: Database must be available first
2. **Tika & NLM Gateway**: Text extraction services
3. **Prefect Server**: Workflow orchestration
4. **Pipeline Service**: Core processing engine
5. **Backend API**: REST interface
6. **Worker Service**: Background task execution
7. **Ingestor Service**: File monitoring (last)