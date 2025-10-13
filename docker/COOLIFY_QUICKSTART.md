# Coolify Quick Start Guide

## TL;DR - Deploy in 3 Steps

### Step 1: Configure Coolify Project
- **Docker Compose File**: `docker/docker-compose.coolify.yml`
- **Working Directory**: `docker`
- **Git Branch**: Your branch name

### Step 2: Set Essential Environment Variables in Coolify

Copy these from your `docker/.env` file to Coolify's environment variables section:

```env
# Core Settings
TIMEZONE=Asia/Shanghai
DOC_ENGINE=elasticsearch
RAGFLOW_IMAGE=infiniflow/ragflow:v0.20.4-slim

# Database
MYSQL_PASSWORD=infini_rag_flow
MYSQL_PORT=5455

# Redis
REDIS_PASSWORD=infini_rag_flow
REDIS_PORT=6379

# MinIO
MINIO_USER=rag_flow
MINIO_PASSWORD=infini_rag_flow
MINIO_PORT=9000
MINIO_CONSOLE_PORT=9001

# Elasticsearch (if using DOC_ENGINE=elasticsearch)
ELASTIC_PASSWORD=infini_rag_flow
ES_PORT=1200
STACK_VERSION=8.11.3

# OpenSearch (if using DOC_ENGINE=opensearch)
OPENSEARCH_PASSWORD=infini_rag_flow_OS_01
OS_PORT=1201

# Server
SVR_HTTP_PORT=9380
HF_ENDPOINT=https://hf-mirror.com

# Limits
MEM_LIMIT=8073741824

# Profile Selection - Choose your backend
COMPOSE_PROFILES=elasticsearch
# OR: COMPOSE_PROFILES=opensearch
# OR: COMPOSE_PROFILES=infinity
```

### Step 3: Deploy
Click "Deploy" in Coolify!

## Common Errors & Quick Fixes

### Error: "Unknown variable DOC_ENGINE"
**Status**: ✅ **FIXED**

**Solution**: The Dockerfile has been updated to accept all build arguments that Coolify passes. This error should no longer occur.

### Error: "Dockerfile not found at ../Dockerfile"
**Cause**: Working directory not set correctly in Coolify.

**Fix**: Set Working Directory to `docker` in Coolify project settings.

### Error: Volume mount paths don't work
**Cause**: Working directory not set correctly.

**Fix**: Set Working Directory to `docker` in Coolify project settings.

## What Changed from Original?

1. **Removed `include` directive**: All services consolidated into one file
2. **Updated Dockerfile**: Now accepts all build args that Coolify injects (prevents "Unknown variable" errors)
3. **Builds from source**: Configured to build your custom image automatically
4. **Explicit profiles**: Services grouped by backend type (elasticsearch, opensearch, infinity, sandbox)

## Build Customization Options

You can customize the build by setting these environment variables in Coolify:

- **`NEED_MIRROR=1`**: Use mirror repositories (recommended for China or faster builds)
- **`LIGHTEN=1`**: Build without large embedding models (reduces image size)

Both are optional and default to `0`.

## Architecture Overview

```
ragflow (main app)
├── mysql (required)
├── redis (required)  
├── minio (required)
└── One of:
    ├── elasticsearch (profile: elasticsearch) - Default
    ├── opensearch (profile: opensearch)
    └── infinity (profile: infinity)

Optional:
└── sandbox-executor-manager (profile: sandbox)
```

## Port Mappings

| Service | Internal Port | Environment Variable | Default Host Port |
|---------|---------------|---------------------|-------------------|
| RAGFlow | 9380 | SVR_HTTP_PORT | 9380 |
| MySQL | 3306 | MYSQL_PORT | 5455 |
| Redis | 6379 | REDIS_PORT | 6379 |
| MinIO | 9000 | MINIO_PORT | 9000 |
| MinIO Console | 9001 | MINIO_CONSOLE_PORT | 9001 |
| Elasticsearch | 9200 | ES_PORT | 1200 |
| OpenSearch | 9201 | OS_PORT | 1201 |

## Next Steps After Deployment

1. Wait for all services to be healthy (check Coolify logs)
2. Access RAGFlow at `http://your-server:9380`
3. Check MinIO console at `http://your-server:9001`
4. Monitor logs for any startup issues

## Still Having Issues?

See the full `COOLIFY_DEPLOYMENT.md` guide for detailed troubleshooting.
