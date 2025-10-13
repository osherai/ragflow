# Coolify Compatibility Changes

## Summary of Changes

This document explains the changes made to enable seamless deployment on Coolify while maintaining the ability to build from source for white-labeling and customization.

## The Problem

Coolify has two key limitations:
1. **Include directives not supported**: Coolify doesn't process Docker Compose `include` directives
2. **Automatic build arg injection**: Coolify automatically passes ALL environment variables as build arguments, which causes errors if the Dockerfile doesn't declare them

## The Solution

### 1. Consolidated Docker Compose File
**File**: `docker/docker-compose.coolify.yml`

- Merged `docker-compose-base.yml` and `docker-compose.yml` into a single file
- Eliminates the `include` directive that Coolify can't process
- Maintains all functionality from the original files

### 2. Updated Dockerfile
**File**: `Dockerfile` (root directory)

Added ARG declarations at the top to accept all environment variables that Coolify injects:

```dockerfile
# Build args that are actually used
ARG NEED_MIRROR=0
ARG LIGHTEN=0

# Coolify compatibility: Accept (but ignore) build args that Coolify automatically injects
ARG SOURCE_COMMIT=""
ARG COOLIFY_BRANCH=""
ARG DOC_ENGINE=""
ARG MYSQL_PASSWORD=""
# ... and many more
```

**Why this works:**
- Docker requires all build args to be declared in the Dockerfile
- By declaring them (with empty defaults), we prevent "Unknown variable" errors
- Only `NEED_MIRROR` and `LIGHTEN` are actually used in the build process
- All other args are accepted but safely ignored

### 3. Documentation
**Files**: `COOLIFY_DEPLOYMENT.md`, `COOLIFY_QUICKSTART.md`

Created comprehensive guides for deploying on Coolify.

## Files Changed

### Modified
1. **`Dockerfile`**
   - Added ~60 ARG declarations for Coolify compatibility
   - No functional changes to the build process
   - Backward compatible with normal Docker builds

2. **`docker/docker-compose.coolify.yml`**
   - Consolidated compose file for Coolify
   - Configured to build from source by default
   - Includes all services from base compose file

3. **`docker/COOLIFY_DEPLOYMENT.md`**
   - Updated to reflect the build-from-source approach
   - Updated troubleshooting section

### Created
1. **`docker/COOLIFY_QUICKSTART.md`**
   - Quick reference guide
   - Common errors and solutions
   - Essential configuration checklist

2. **`docker/COOLIFY_CHANGES.md`**
   - This file
   - Documents all changes for future reference

## Impact on Existing Deployments

### Non-Coolify Deployments (No Impact)
- Original `docker-compose.yml` files are unchanged
- Dockerfile changes are backward compatible
- Standard Docker builds work exactly as before
- The added ARG declarations have no effect when args aren't passed

### Coolify Deployments (Fully Supported)
- Use `docker/docker-compose.coolify.yml` as the compose file
- Set working directory to `docker`
- Builds from source will work without errors
- Perfect for white-labeling and customization

## Build Arguments Explained

### Used by Dockerfile
- **`NEED_MIRROR`** (default: 0): Use mirror repositories for faster downloads in China
- **`LIGHTEN`** (default: 0): Skip large embedding models to reduce image size

### Accepted but Ignored
All other environment variables (like `DOC_ENGINE`, `MYSQL_PASSWORD`, etc.) are:
- Required by the running containers (via environment variables)
- Passed as build args by Coolify automatically
- Declared in Dockerfile to prevent errors
- Not used during the build process

## Testing

### Before Deployment
1. Verify `docker/docker-compose.coolify.yml` exists
2. Check that Dockerfile has the ARG declarations (lines 10-66)
3. Ensure `.env` file or Coolify environment variables are configured

### After Deployment
1. Check build logs for any ARG-related errors (should be none)
2. Verify all services start successfully
3. Test RAGFlow application functionality

## Maintenance

### Adding New Environment Variables
If you add new environment variables to your `.env` file:
1. Add runtime variables to `docker-compose.coolify.yml` in the `environment:` section
2. If Coolify passes them as build args and you get errors, add them to Dockerfile's ARG list

### Customizing for White-Labeling
The Dockerfile can be freely modified for white-labeling:
- Change branding
- Add custom packages
- Modify configurations
- All changes will be built by Coolify automatically

## Rollback Plan

If issues occur with the Coolify-specific changes:

1. **Restore original Dockerfile**:
   ```bash
   git checkout main -- Dockerfile
   ```

2. **Use pre-built image approach**:
   Edit `docker-compose.coolify.yml` and replace the `build:` section with:
   ```yaml
   image: infiniflow/ragflow:v0.20.4-slim
   ```

## Support

For issues specific to these changes:
1. Check `COOLIFY_QUICKSTART.md` for common errors
2. Review `COOLIFY_DEPLOYMENT.md` for detailed troubleshooting
3. Verify Coolify logs for build or deployment errors

## References

- [Coolify GitHub Issue #4295](https://github.com/coollabsio/coolify/discussions/4295) - Include directive limitations
- Docker Compose documentation - Build arguments
- Coolify documentation - Docker Compose deployments
