# White Labeling Changes Summary for Osher Digital

This document summarizes all the changes made to rebrand the application for Osher Digital.

## Company Information Applied

- **Company Name:** Osher Digital
- **Website:** https://osher.com.au
- **Contact Email:** info@osher.com.au
- **Brand Colors:** Orange (#F89406), White, and Black

## Changes Made

### 1. Documentation & README Files
- ✅ Updated main `README.md` with Osher Digital branding
- ✅ Removed all localized README files (Chinese, Japanese, Korean, Portuguese, Indonesian)
- ✅ Created new simplified `docs/README.md`
- ✅ Updated `docker/README.md` with deployment documentation
- ✅ Removed all references to ragflow.io URLs and InfiniFlow

### 2. Web Application (Frontend)
- ✅ Updated `web/src/conf.json` - App name set to "Osher Digital"
- ✅ Updated `web/package.json` - Author changed to "Osher Digital"
- ✅ Replaced logo files with Osher Digital branding:
  - `web/src/assets/logo-with-text.png`
- ✅ Updated banner components with Osher Digital branding and orange/black gradient
- ✅ Removed localized language files (Chinese, Russian, Portuguese, Japanese, Indonesian, Korean)
- ✅ Updated English localization to remove RAGFlow references
- ✅ Removed documentation links pointing to ragflow.io
- ✅ Updated modal components to remove external links

### 3. Docker Configuration
- ✅ Updated `docker/.env`:
  - Changed `RAGFLOW_IMAGE` to `OSHER_DIGITAL_IMAGE`
  - Updated image references to osher-digital
- ✅ Updated `docker-compose.yml`:
  - Service name: `ragflow` → `osher-digital`
  - Container name: `ragflow-server` → `osher-digital-server`
  - Network name: `ragflow` → `osher-digital`
  - Log directory: `ragflow-logs` → `osher-digital-logs`
- ✅ Updated `docker-compose-gpu.yml` with same changes
- ✅ Updated `docker-compose-base.yml`:
  - All container names prefixed with `osher-digital-`
  - Network updated to `osher-digital`

### 4. Python SDK & API
- ✅ Updated `sdk/python/pyproject.toml`:
  - Package name: `ragflow-sdk` → `osher-digital-sdk`
  - Version: 1.0.0
  - Author: Osher Digital
  - Contact: info@osher.com.au
- ✅ Created new `sdk/python/README.md`
- ✅ Updated `api/ragflow_server.py` copyright to Osher Digital
- ✅ Updated main `pyproject.toml` with Osher Digital branding

### 5. Helm Charts
- ✅ Updated `helm/Chart.yaml`:
  - Chart name: `ragflow` → `osher-digital`
  - Version: 1.0.0
  - Description updated
- ✅ Updated `helm/values.yaml`:
  - Database names and passwords updated
  - User references updated to osher_digital

### 6. Integrations
- ✅ Updated Chrome extension manifest
- ✅ Created integration README files
- ✅ Removed external RAGFlow service references

## Container Names Reference

Old → New:
- `ragflow-server` → `osher-digital-server`
- `ragflow-es-01` → `osher-digital-es-01`
- `ragflow-opensearch-01` → `osher-digital-opensearch-01`
- `ragflow-infinity` → `osher-digital-infinity`
- `ragflow-mysql` → `osher-digital-mysql`
- `ragflow-minio` → `osher-digital-minio`
- `ragflow-redis` → `osher-digital-redis`
- `ragflow-sandbox-executor-manager` → `osher-digital-sandbox-executor-manager`

## Network Names

- Docker network: `ragflow` → `osher-digital`

## Image Names

- `infiniflow/ragflow:*` → `osher-digital/osher-digital:*`

## Database/Storage Names

- MySQL database: `rag_flow` → `osher_digital`
- MinIO user: `rag_flow` → `osher_digital`
- Passwords updated with `osher_digital` prefix

## Next Steps

1. **Build new Docker images:**
   ```bash
   cd osher-digital/
   docker build --platform linux/amd64 --build-arg LIGHTEN=1 -f Dockerfile -t osher-digital/osher-digital:latest-slim .
   docker build --platform linux/amd64 -f Dockerfile -t osher-digital/osher-digital:latest .
   ```

2. **Update `.env` file if needed:**
   - Set your timezone
   - Configure passwords
   - Set up LLM API keys in `service_conf.yaml.template`

3. **Deploy:**
   ```bash
   cd docker
   docker compose -f docker-compose.yml up -d
   ```

4. **Verify the deployment:**
   ```bash
   docker logs -f osher-digital-server
   ```

## Visual Branding Applied

- Primary gradient: Orange (#F89406) to Black (#000000)
- Logo already updated with Osher Digital branding
- Banner text updated throughout the application

## Support & Contact

All references now point to:
- **Email:** info@osher.com.au
- **Website:** https://osher.com.au

---

**White Labeling Completed:** $(date +%Y-%m-%d)  
**Target Company:** Osher Digital  
**Version:** 1.0.0

© 2025 Osher Digital. All rights reserved.

