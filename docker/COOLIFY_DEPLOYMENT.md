# Coolify Deployment Guide

## Overview

This guide explains how to deploy RAGFlow on Coolify, addressing the platform's limitations with Docker Compose `include` directives.

## The Problem

Coolify has known limitations with Docker Compose `include` directives. While Docker Compose supports including sub-compose files, Coolify may only parse the top-level `docker-compose.yml` file and not fully process the included files.

## The Solution

We've created a consolidated `docker-compose.coolify.yml` file that merges all necessary services into a single file, eliminating the need for `include` directives.

## Deployment Steps

### 1. Use the Consolidated Compose File

Instead of using the standard `docker-compose.yml`, use the consolidated file:

```bash
# In Coolify, specify this file as your compose file:
docker/docker-compose.coolify.yml
```

### 2. Environment Variables

Ensure you have all required environment variables set in your `.env` file. Key variables include:

```env
# Database
MYSQL_PASSWORD=your_mysql_password
MYSQL_PORT=3306

# Redis
REDIS_PASSWORD=your_redis_password
REDIS_PORT=6379

# MinIO
MINIO_USER=your_minio_user
MINIO_PASSWORD=your_minio_password
MINIO_PORT=9000
MINIO_CONSOLE_PORT=9001

# Elasticsearch (if using elasticsearch profile)
STACK_VERSION=8.11.0
ELASTIC_PASSWORD=your_elastic_password
ES_PORT=9200

# OpenSearch (if using opensearch profile)
OPENSEARCH_PASSWORD=your_opensearch_password
OS_PORT=9201

# Infinity (if using infinity profile)
INFINITY_THRIFT_PORT=23817
INFINITY_HTTP_PORT=23820
INFINITY_PSQL_PORT=5432

# Sandbox (if using sandbox profile)
SANDBOX_EXECUTOR_MANAGER_PORT=9385

# RAGFlow Server
SVR_HTTP_PORT=9380
TIMEZONE=UTC
HF_ENDPOINT=https://huggingface.co
MACOS=

# Memory limits
MEM_LIMIT=4g
```

### 3. Profile Selection

The consolidated file includes several profiles for different components:

- `elasticsearch` - For Elasticsearch backend
- `opensearch` - For OpenSearch backend  
- `infinity` - For Infinity backend
- `sandbox` - For sandbox executor

In Coolify, you can specify which profiles to activate by setting the `COMPOSE_PROFILES` environment variable:

```env
# For a basic setup with Elasticsearch:
COMPOSE_PROFILES=elasticsearch

# For a setup with OpenSearch:
COMPOSE_PROFILES=opensearch

# For a setup with Infinity:
COMPOSE_PROFILES=infinity

# For a setup with sandbox support:
COMPOSE_PROFILES=elasticsearch,sandbox
```

### 4. Coolify Configuration

In your Coolify project settings:

1. **Docker Compose File**: Set to `docker/docker-compose.coolify.yml`
2. **Environment Variables**: Upload your `.env` file or set variables in Coolify's UI
3. **Working Directory**: Set to `docker` (the compose file needs to be run from the docker directory)
4. **Profiles**: Set `COMPOSE_PROFILES` environment variable as needed

**Important Notes:**
- The Dockerfile has been updated to accept all build arguments that Coolify automatically injects
- Building from source will work out of the box - no special configuration needed
- Set `NEED_MIRROR=1` if you need to use mirror repositories (for China/faster builds)
- Set `LIGHTEN=1` for a lighter build without some embedding models

### 5. Alternative: Override Files

If you prefer to keep using the original files, you can use Docker Compose overrides in Coolify:

```bash
# In Coolify's command override, use:
docker-compose -f docker-compose.yml -f docker-compose-base.yml up
```

However, the consolidated file approach is more reliable for Coolify.

## File Structure

```
docker/
├── docker-compose.coolify.yml    # Consolidated file for Coolify
├── docker-compose.yml            # Original (uses include)
├── docker-compose-base.yml       # Base services (included by others)
├── docker-compose-macos.yml      # macOS variant (uses include)
├── docker-compose-gpu.yml        # GPU variant (uses include)
├── docker-compose-CN-oc9.yml     # Chinese variant (uses include)
└── COOLIFY_DEPLOYMENT.md         # This guide
```

## Building from Source

The `docker-compose.coolify.yml` file is configured to build from source by default. The Dockerfile has been updated to accept all build arguments that Coolify automatically injects, so building will work seamlessly.

### Build Configuration Options

You can customize the build with these optional environment variables:

- **`NEED_MIRROR`**: Set to `1` if you need to use mirror repositories (recommended for deployments in China or for faster builds)
- **`LIGHTEN`**: Set to `1` for a lighter build that excludes some large embedding models (reduces image size)

These can be set in Coolify's environment variables section.

## Troubleshooting

### Common Issues

1. **Build argument errors** (like "Unknown variable DOC_ENGINE"):
   - Status: ✅ **FIXED** - The Dockerfile now accepts all build args that Coolify passes
   - The Dockerfile declares all environment variables as build args to prevent errors
   - Only `NEED_MIRROR` and `LIGHTEN` are actually used; others are accepted but ignored

2. **Services not starting**: Ensure all required environment variables are set

3. **Port conflicts**: Check that ports in your environment don't conflict with other services

4. **Memory issues**: Adjust `MEM_LIMIT` based on your server's available memory

5. **Profile not activated**: Make sure `COMPOSE_PROFILES` is set correctly

6. **Dockerfile not found at ../Dockerfile**: This shouldn't occur with the build configuration, but if you see it, check that the Working Directory is set to `docker` in Coolify.

7. **Volume mount issues**: Ensure the working directory is set to `docker` in Coolify so relative paths work correctly

### Logs

Check Coolify's logs for any startup issues. Common problems include:
- Missing environment variables
- Port conflicts
- Insufficient memory
- Network connectivity issues
- Build argument conflicts (fixed in the updated Dockerfile)

## Support

If you encounter issues with this deployment, please:
1. Check the Coolify logs
2. Verify all environment variables are set
3. Ensure your server meets the minimum requirements
4. Review the original RAGFlow documentation for additional configuration options
