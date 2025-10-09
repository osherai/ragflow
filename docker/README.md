# Docker Configuration Guide

## Environment Variables

The `.env` file contains all the configuration options for the Osher Digital deployment.

### Key Configuration Options

#### Database Settings
- `MYSQL_PASSWORD`: MySQL root password
- `MYSQL_HOST`: MySQL hostname (default: mysql)
- `MYSQL_PORT`: MySQL port (default: 5455)

#### Storage Settings
- `MINIO_USER`: MinIO username
- `MINIO_PASSWORD`: MinIO password
- `MINIO_PORT`: MinIO API port (default: 9000)
- `MINIO_CONSOLE_PORT`: MinIO console port (default: 9001)

#### Redis Settings
- `REDIS_PASSWORD`: Redis password
- `REDIS_PORT`: Redis port (default: 6379)

#### Search Engine Settings
- `DOC_ENGINE`: Document engine type (elasticsearch, infinity, or opensearch)
- `ES_PORT`: Elasticsearch port
- `ELASTIC_PASSWORD`: Elasticsearch password

#### Application Settings
- `SVR_HTTP_PORT`: HTTP server port (default: 9380)
- `TIMEZONE`: System timezone
- `OSHER_DIGITAL_IMAGE`: Docker image to use

## Docker Compose Files

### docker-compose.yml
Standard deployment configuration using CPU.

```bash
docker compose -f docker-compose.yml up -d
```

### docker-compose-gpu.yml
GPU-accelerated deployment for faster processing.

```bash
docker compose -f docker-compose-gpu.yml up -d
```

### docker-compose-base.yml
Base configuration with shared services (MySQL, Redis, MinIO, Elasticsearch/Infinity).

## Service Configuration

The `service_conf.yaml.template` file configures backend services. Environment variables in this file are automatically populated when the Docker container starts.

## Logs

Application logs are stored in `./osher-digital-logs/`

## Support

For assistance, contact info@osher.com.au

---

© 2025 Osher Digital. All rights reserved.
