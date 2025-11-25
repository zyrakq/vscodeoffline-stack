# 🐳 Development Tools Docker Stack

This project contains Docker configurations and compose files for running development tools and services with support for various SSL automation options.

## 🧩 Components

### [🎨 VSCode Offline Gallery](src/vscodeoffline)

VSCode Offline Gallery — an offline marketplace for VS Code extensions. Provides local hosting of extensions and installers, enabling VS Code to work without internet access or with restricted network environments.

[Learn more about VSCode Offline configuration](src/vscodeoffline/README.md).

### [🔐 SSL Automation](src/ssl-automation)

SSL certificate management solutions for Docker environments:

#### [🚀 Let's Encrypt Manager](src/ssl-automation/letsencrypt-manager)

Automatic SSL certificate management from Let's Encrypt using nginx-proxy and acme-companion. Provides free, trusted SSL certificates with automatic renewal for public domains.

[Learn more about Let's Encrypt Manager](src/ssl-automation/letsencrypt-manager/README.md).

#### [🔒 Step CA Manager](src/ssl-automation/step-ca-manager)

Private Certificate Authority management with Step CA and local DNS resolution. Enables trusted SSL certificates for local development domains (e.g., `*.local`) with automatic certificate issuance via ACME protocol.

[Learn more about Step CA Manager](src/ssl-automation/step-ca-manager/README.md).

## 🚀 Getting Started

To run the services, use the appropriate `docker-compose.yml` files in the subprojects. Make sure all environment variables are configured correctly.

Each service directory contains:

- 📋 Docker Compose configurations
- 🔧 Environment variable examples
- 📖 Detailed setup instructions
- 🛠️ Helper scripts for development and production

## 🏗️ Project Structure

```sh
├── src/
│   ├── vscodeoffline/           # VSCode Offline Gallery
│   └── ssl-automation/          # SSL certificate management
│       ├── letsencrypt-manager/ # Let's Encrypt automation
│       └── step-ca-manager/     # Step CA private certificates
```

## 📄 License

This project is dual-licensed under:

- [Apache License 2.0](LICENSE-APACHE)
- [MIT License](LICENSE-MIT)

You may choose either license for your use.
