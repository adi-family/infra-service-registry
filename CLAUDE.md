infra-service-registry, docker-compose, deployment, plugins

## Overview
- Docker Compose deployment for ADI Plugin Registry
- Hosts and serves ADI plugins (.tar.gz packages)
- Provides plugin metadata, versioning, and downloads
- License: BSL-1.0

## Structure
- `docker-compose.yml` - Deployment configuration
- Builds from `../../crates/adi-plugin-registry-http` (submodule)

## Environment Variables
- `PORT` - Listen port (default: 8080)
- `DATA_PATH` - Plugin storage directory (default: /data)
- `RUST_LOG` - Log level (info, debug, trace)

## API Endpoints
- `GET /plugins` - List all available plugins
- `GET /plugins/{name}` - Get plugin metadata
- `GET /plugins/{name}/{version}` - Get specific version metadata
- `GET /plugins/{name}/{version}/download` - Download plugin package
- `POST /plugins` - Upload new plugin (requires auth)
- `GET /health` - Health check

## Deployment
```bash
docker compose up -d
```

## URL
Production: https://adi.the-ihor.com/api/registry

## Storage
Plugin packages are stored in persistent volume `registry-data` mounted at `/data`.

## Updating
The registry code is in a git submodule:
```bash
cd ../../crates/adi-plugin-registry-http
git pull origin main
cd ../../apps/infra-service-registry
git add ../../crates/adi-plugin-registry-http
git commit -m "🔗 Update registry: <description>"
git push
```

## Notes
- Used by `adi plugin install <name>` command
- Stores plugin manifests and .tar.gz archives
- Supports semantic versioning
- Can be self-hosted for private plugins
