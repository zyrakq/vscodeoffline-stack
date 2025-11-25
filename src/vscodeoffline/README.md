# 🎨 VSCode Offline Gallery

A modular Docker Compose configuration system for offline VS Code extensions gallery with support for multiple deployment environments.

## 🚀 Quick Start

### 1. Build Configurations

Generate all configurations using [stackbuilder](https://github.com/zyrakq/stackbuilder):

```bash
sb build
```

This creates ready-to-use Docker Compose configurations for VSCode Offline in the `build/` directory.

### 2. Deploy

Navigate to your chosen configuration and deploy:

```bash
# Example: deploy with port forwarding for local access
cd build/forwarding/
cp .env.example .env
# Edit .env with your values if needed
docker compose up --build -d
```

For more information about vscsync and vscgallery, visit the [lolinternet/vscsync](https://github.com/lolinternet/vscsync) repository.

## 📁 Project Structure

- **`components/`** - Source Docker Compose components
  - `base/` - Core services configuration (vscsync, vscgallery)
  - `environments/` - Environment configurations (devcontainer, forwarding, letsencrypt, step-ca)
  - `extensions/` - Optional extensions (mTLS guard)
- **`build/`** - Generated configurations (created by `sb build`)
- **`stackbuilder.toml`** - Build configuration for stackbuilder

## 🔧 Available Configurations

### Environments

- **devcontainer** - Development environment with external workspace network for VS Code Dev Containers
- **forwarding** - Local access configuration with port 80 exposed to host
- **letsencrypt** - Integration with Let's Encrypt for automatic SSL certificates (base and mTLS variants)
- **step-ca** - Integration with Step CA for private SSL certificates (base and mTLS variants)

Generated configurations are available in the `build/` directory after running `sb build`.

## 🔧 Environment Variables

### Base Configuration (from `components/base/.env.example`)

**Project Settings:**

- `COMPOSE_PROJECT_NAME`: Project name for Docker Compose (default: `vscodeoffline`)

**Sync Configuration:**

- `SYNCARGS`: Arguments for vscsync synchronization (default: `--sync`)
  - Example: `--sync --skip-binaries --check-specified-extensions --total-recommended 0`

**Gallery Configuration:**

- `BIND`: Bind address and port for vscgallery (default: `0.0.0.0:80`)
- `SSLARGS`: SSL arguments for gunicorn (default: empty)

### Environment-Specific Configuration

**Devcontainer Environment:**

- Uses base configuration
- Connects to external workspace network `vscodeoffline-workspace-network`
- Suitable for VS Code Dev Containers integration

**Forwarding Environment:**

- Uses base configuration
- Exposes port `80:80` for local access
- Accessible at `http://localhost`

**Let's Encrypt Environment:**

- `VIRTUAL_PORT`: Container port for nginx-proxy (default: `80`)
- `VIRTUAL_HOST`: Domain name for the gallery (e.g., `vscgallery.example.com`)
- `LETSENCRYPT_HOST`: Domain for SSL certificate (usually same as VIRTUAL_HOST)
- `LETSENCRYPT_EMAIL`: Email for Let's Encrypt certificate registration
- Connects to external `letsencrypt-network` and `letsencrypt-certs` volume
- Requires [letsencrypt-manager](../ssl-automation/letsencrypt-manager) to be running

**Step CA Environment:**

- `VIRTUAL_PORT`: Container port for nginx-proxy (default: `80`)
- `VIRTUAL_HOST`: Domain name for the gallery (e.g., `vscgallery.local`)
- `LETSENCRYPT_HOST`: Domain for SSL certificate (usually same as VIRTUAL_HOST)
- `LETSENCRYPT_EMAIL`: Email for Step CA certificate registration
- Connects to external `step-ca-network` and `step-ca-certs` volume
- Requires [step-ca-manager](../ssl-automation/step-ca-manager) to be running

### mTLS Extension Configuration

**Guard mTLS (optional):**

- `SSL_CLIENT_CERT_CONTENT`: Base64-encoded CA certificate for client verification
  - Generate: `cat ca.crt | base64 -w 0`
- `SSL_CLIENT_CERT_PATH`: Path to CA certificate file (default: `/certs/${VIRTUAL_HOST}.ca.crt`)
- Enables mutual TLS authentication for enhanced security
- Available for `letsencrypt` and `step-ca` environments

**Setup Guides:**

- [📝 Certificate Creation Guide (Step CA)](mTLS-step-ca.md) - Create intermediate CA and client certificates
- [🌐 Browser Configuration Guide](mTLS-browser-setup.md) - Import certificates into Firefox and Chromium
- [🔐 Certificate Examples](certificate/) - Example certificates with detailed documentation
- For Step CA deployment and initial CLI setup, see [step-ca-stack](https://github.com/zyrakq/step-ca-stack)

## 🔌 Service Details

### vscsync

Synchronizes VS Code extensions and installers from official marketplace.

- **Container Name**: `vscsync`
- **Image**: `lolinternet/vscsync:latest`
- **Purpose**: Downloads and maintains offline copy of VS Code extensions
- **Volume**: `vscodeoffline` (shared with vscgallery)
- **Network**: `vscodeoffline-network`

### vscgallery

Serves VS Code extensions through HTTP/HTTPS API compatible with VS Code.

- **Container Name**: `vscgallery`
- **Image**: `lolinternet/vscgallery:latest`
- **Port**: 80 (HTTP/HTTPS depending on configuration)
- **Volume**: `vscodeoffline` (shared with vscsync)
- **Network**: `vscodeoffline-network` (+ environment-specific networks)

### Accessing the Gallery

After deployment, configure VS Code to use your offline gallery:

**For local/forwarding deployment:**

Add to VS Code `settings.json`:

```json
{
  "extensions.gallery": {
    "serviceUrl": "http://localhost/vscode/gallery",
    "itemUrl": "http://localhost/vscode/item"
  }
}
```

**For Let's Encrypt/Step CA deployment:**

```json
{
  "extensions.gallery": {
    "serviceUrl": "https://vscgallery.example.com/vscode/gallery",
    "itemUrl": "https://vscgallery.example.com/vscode/item"
  }
}
```

## 🛠️ Development

### Adding New Components

1. **New Environment**: Create directory in `components/environments/` with `docker-compose.yml` and optional `.env.example`
2. **Update Configuration**: Modify [`stackbuilder.toml`](stackbuilder.toml) to include new environment
3. **Rebuild**: Run `sb build` to regenerate configurations

### Modifying Components

1. Edit files in `components/`
2. Run `sb build` to regenerate all configurations
3. The `build/` directory will be completely recreated

## 📝 Notes

- The `build/` directory is automatically generated - do not edit manually
- User `.env` files are preserved during rebuilds
- All configurations are built from [`stackbuilder.toml`](stackbuilder.toml) specification
- Extensions are synchronized automatically by vscsync on container start
- Both services share the same volume for artifacts storage
- For SSL deployments, ensure the corresponding SSL automation stack is running first

## 🔗 Source Code

- **This project:** [LOLINTERNETZ/vscodeoffline](https://github.com/LOLINTERNETZ/vscodeoffline) (MIT License)

## 📚 Additional Resources

- [VS Code Extension Marketplace API](https://code.visualstudio.com/api/working-with-extensions/publishing-extension)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [nginx-proxy Documentation](https://github.com/nginx-proxy/nginx-proxy)
