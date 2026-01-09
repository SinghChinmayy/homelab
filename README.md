# 🏠 Homelab

> [!WARNING]
> 🚧 **This repository is currently under construction** - Active development in progress. Configurations and documentation may change frequently.

A comprehensive, Docker-based homelab setup for self-hosted media management and streaming. This repository contains production-ready Docker Compose configurations for deploying a complete media automation stack with optional Tailscale integration for secure remote access. All containers are managed through **Portainer** for easy monitoring, updates, and administration.

[![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)](https://www.linux.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

## 📋 Table of Contents

- [Overview](#-overview)
- [Services](#-services)
- [Features](#-features)
- [Prerequisites](#-prerequisites)
- [Quick Start](#-quick-start)
- [Deployment Options](#-deployment-options)
- [Configuration](#-configuration)
- [Port Reference](#-port-reference)
- [Network Architecture](#-network-architecture)
- [Environment Variables](#-environment-variables)
- [Maintenance](#-maintenance)
- [Troubleshooting](#-troubleshooting)
- [Contributing](#-contributing)
- [License](#-license)

## 🎯 Overview

This homelab setup provides a complete, automated media management solution with the following capabilities:
- **Automated content discovery** and download management
- **Multi-format media support** (TV shows, movies, music, books)
- **Centralized media streaming** through Jellyfin
- **Unified dashboard** for monitoring and management
- **Secure remote access** via Tailscale VPN
- **Professional container management** using Portainer for easy deployment, monitoring, and maintenance

## 🚀 Services

### Media Management (*arr Stack)

| Service | Description | Default Port |
|---------|-------------|--------------|
| **[Prowlarr](https://github.com/Prowlarr/Prowlarr)** | Indexer manager and proxy | 9696 |
| **[Sonarr](https://github.com/Sonarr/Sonarr)** | TV show collection manager | 8989 |
| **[Radarr](https://github.com/Radarr/Radarr)** | Movie collection manager | 7878 |
| **[Lidarr](https://github.com/Lidarr/Lidarr)** | Music collection manager | 8686 |
| **[Readarr](https://github.com/Readarr/Readarr)** | eBook and audiobook manager | 8787 |
| **[Bazarr](https://github.com/morpheus65535/bazarr)** | Subtitle management | - |

### Download & Streaming

| Service | Description | Default Port |
|---------|-------------|--------------|
| **[qBittorrent](https://github.com/qbittorrent/qBittorrent)** | BitTorrent client with WebUI | 8080 |
| **[Jellyfin](https://github.com/jellyfin/jellyfin)** | Open-source media server | 8096 |

### Management & Monitoring

| Service | Description | Default Port |
|---------|-------------|--------------|
| **[Homarr](https://github.com/ajnart/homarr)** | Customizable dashboard | 7575 |
| **[Portainer](https://github.com/portainer/portainer)** | Docker container management & orchestration UI | 9000/9443 |

> **Note**: This homelab uses Portainer for comprehensive container lifecycle management, making it easy to deploy, monitor, update, and troubleshoot all services through a single web interface.

### Network & Security

| Service | Description |
|---------|-------------|
| **[Tailscale](https://tailscale.com/)** | Zero-config VPN (mesh network) |
| **[Caddy](https://caddyserver.com/)** | Reverse proxy with automatic HTTPS |

## ✨ Features

- 🐳 **Fully Containerized**: All services run in Docker containers for easy deployment and updates
- 🎛️ **Portainer Management**: Full container lifecycle management through intuitive web UI
- 🔒 **Secure Remote Access**: Tailscale integration for secure access from anywhere
- 📊 **Unified Dashboard**: Homarr provides a beautiful, centralized interface
- 🔄 **Automatic Updates**: Services configured with `unless-stopped` restart policy
- 💾 **Persistent Storage**: All data stored in mapped volumes
- 🌐 **Reverse Proxy Ready**: Pre-configured Caddy setup for custom domains
- 📦 **Modular Design**: Deploy individual services or the entire stack
- ⚡ **Performance Optimized**: LinuxServer.io images for best performance

## 📦 Prerequisites

### Required

- **Docker**: Version 20.10 or higher
- **Docker Compose**: Version 2.0 or higher
- **Storage**: Minimum 100GB free space (recommended: 1TB+)
- **RAM**: Minimum 4GB (recommended: 8GB+)

### Optional
- **Tailscale Account**: For remote access deployment
- **Domain Name**: For reverse proxy with custom domains

### Installation

```bash
# Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Install Docker Compose
sudo apt-get update
sudo apt-get install docker-compose-plugin

# Verify installations
docker --version
docker compose version
```

## 🏁 Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/SinghChinmayy/homelab.git
cd homelab
```

### 2. Create Environment File

```bash
# For standard deployment
cp .env.example .env

# Edit with your settings
nano .env
```

### 3. Deploy the Stack

```bash
# Option A: Standard deployment (local network)
cd compose-files/arr-stack
docker compose up -d

# Option B: Tailscale deployment (remote access)
cd tailscalexcompose-files/arrstack
docker compose up -d
```

### 4. Access Services

Once deployed, access your services:
- **Portainer** (Container Management): http://localhost:9000 or https://localhost:9443
- **Homarr Dashboard**: http://localhost:7575
- **Jellyfin**: http://localhost:8096
- **qBittorrent**: http://localhost:8080
- **Prowlarr**: http://localhost:9696
- **Sonarr**: http://localhost:8989
- **Radarr**: http://localhost:7878
- **Lidarr**: http://localhost:8686
- **Readarr**: http://localhost:8787

> **💡 Pro Tip**: Use Portainer to monitor all your containers, view logs, restart services, and manage the entire stack from one interface!

## 🔧 Deployment Options

### Option 1: Standard Deployment

Best for **local network access** only. Services expose ports directly on the host.

```bash
cd compose-files/arr-stack
docker compose up -d
```

**Pros:**
- Simple setup
- Direct port access
- Lower resource usage

**Cons:**
- No remote access without port forwarding
- No built-in encryption
- Requires VPN or reverse proxy for external access

### Option 2: Tailscale Deployment

Best for **secure remote access** from anywhere. All services run within Tailscale network namespace.

```bash
cd tailscalexcompose-files/arrstack
docker compose up -d
```

**Pros:**
- Secure remote access
- Zero-config VPN
- End-to-end encryption
- No port forwarding needed
- Access from mobile devices

**Cons:**
- Requires Tailscale account
- Slightly more complex setup
- Additional container overhead

### Option 3: Portainer Management (Recommended)

For **GUI-based container management**. This is the **preferred method** for this homelab as it provides:
- Visual container monitoring and management
- One-click container start/stop/restart
- Real-time resource usage statistics
- Container logs viewer
- Stack deployment and updates
- Image management

```bash
cd portainer/arr
docker compose up -d
```

Once Portainer is running, you can deploy and manage all other stacks directly from the Portainer web interface at http://localhost:9000

**Benefits:**
- No need to remember docker commands
- Visual interface for all operations
- Container health monitoring
- Easy troubleshooting with integrated logs
- Template-based deployment

## ⚙️ Configuration

### Environment Variables

Create a `.env` file in the compose directory:

```bash
# Standard Deployment
ARRPATH=/path/to/your/media/  # Must end with /

# Tailscale Deployment
Root=/path/to/your/media
TS_HOSTNAME=homelab-arr
TS_AUTHKEY=your_tailscale_auth_key
TS_EXTRA_ARGS=--advertise-tags=tag:container
TAILSCALE_STATE_DIR=/var/lib/tailscale
PUID=1000
PGID=1000
TZ=America/New_York
WEBUI_PORT=8080
TORRENTING_PORT=6881
```

### Directory Structure

Ensure your media directory follows this structure:

```
/path/to/your/media/
├── Downloads/           # Shared download directory
├── Prowlarr/
│   ├── config/
│   └── backup/
├── Sonarr/
│   ├── config/
│   ├── backup/
│   └── tvshows/
├── Radarr/
│   ├── config/
│   ├── backup/
│   └── movies/
├── Lidarr/
│   ├── config/
│   └── music/
├── Readarr/
│   ├── config/
│   └── books/
├── Jellyfin/
│   └── config/
├── Homarr/
│   ├── configs/
│   ├── icons/
│   └── data/
└── qbittorrent/
    └── config/
```

### Getting Tailscale Auth Key

1. Create a [Tailscale account](https://login.tailscale.com/start)
2. Go to [Settings > Keys](https://login.tailscale.com/admin/settings/keys)
3. Generate an auth key:
   - Select "Reusable" if deploying multiple stacks
   - Set expiration as needed
   - Add tag `container` if using ACLs
4. Copy the key to your `.env` file

## 🔌 Port Reference

| Service | Internal Port | Mapped Port | Protocol |
|---------|---------------|-------------|----------|
| Portainer (HTTP) | 9000 | 9000 | HTTP |
| Portainer (HTTPS) | 9443 | 9443 | HTTPS |
| Prowlarr | 9696 | 9696 | HTTP |
| Sonarr | 8989 | 8989 | HTTP |
| Radarr | 7878 | 7878 | HTTP |
| Lidarr | 8686 | 8686 | HTTP |
| Readarr | 8787 | 8787 | HTTP |
| Homarr | 7575 | 7575 | HTTP |
| Jellyfin | 8096 | 8096 | HTTP |
| Jellyfin Discovery | 7359 | 7359 | UDP |
| Jellyfin DLNA | 1900 | 1900 | UDP |
| qBittorrent WebUI | 8080 | 8080 | HTTP |
| qBittorrent | 6881 | 6881 | TCP/UDP |
| Caddy HTTP | 80 | 80 | HTTP |
| Caddy HTTPS | 443 | 443 | HTTPS/HTTP3 |

## 🌐 Network Architecture

### Standard Deployment
```
Internet
    ↓
Router → Host Machine → Docker Bridge Network
                            ↓
                        [Services]
```

### Tailscale Deployment
```
Internet
    ↓
Tailscale Network (Encrypted Mesh)
    ↓
tailscale-arr container (Gateway)
    ↓
[Services] (Shared Network Namespace)
    ├── Radarr
    ├── Sonarr
    ├── Prowlarr
    ├── Lidarr
    ├── Readarr
    ├── Bazarr
    ├── qBittorrent
    ├── Jellyfin
    └── Caddy (Reverse Proxy)
```

## 🔐 Environment Variables

### Required for All Deployments

```bash
# File paths
ARRPATH=/path/to/media/        # Standard deployment
Root=/path/to/media            # Tailscale deployment
```

### Required for Tailscale Deployment

```bash
# Tailscale Configuration
TS_HOSTNAME=homelab-arr        # Hostname in Tailscale network
TS_AUTHKEY=tskey-xxxxx        # Your Tailscale auth key
TS_EXTRA_ARGS=--advertise-tags=tag:container
TAILSCALE_STATE_DIR=/var/lib/tailscale

# User/Group IDs (for file permissions)
PUID=1000                      # User ID (run 'id -u' to find yours)
PGID=1000                      # Group ID (run 'id -g' to find yours)

# Timezone
TZ=America/New_York           # Your timezone (see: timedatectl list-timezones)

# qBittorrent
WEBUI_PORT=8080               # Web interface port
TORRENTING_PORT=6881          # Torrent traffic port
```

## 🛠️ Maintenance

### Update All Services

```bash
# Pull latest images
docker compose pull

# Recreate containers with new images
docker compose up -d

# Remove old images
docker image prune -f
```

### View Logs

```bash
# All services
docker compose logs -f

# Specific service
docker compose logs -f radarr

# Last 100 lines
docker compose logs --tail=100 sonarr
```

### Restart Services

```bash
# Restart all
docker compose restart

# Restart specific service
docker compose restart jellyfin
```

### Stop Services

```bash
# Stop all
docker compose down

# Stop without removing containers
docker compose stop
```

### Backup Configuration

```bash
# Backup entire media directory
tar -czf homelab-backup-$(date +%Y%m%d).tar.gz /path/to/media/

# Backup specific service
tar -czf sonarr-backup-$(date +%Y%m%d).tar.gz /path/to/media/Sonarr/
```

## 🐛 Troubleshooting

### Permission Issues

```bash
# Fix ownership (replace PUID/PGID with your values)
sudo chown -R 1000:1000 /path/to/media/

# Fix permissions
sudo chmod -R 755 /path/to/media/
```

### Container Won't Start

```bash
# Check logs
docker compose logs [service-name]

# Check if port is already in use
sudo lsof -i :[port-number]

# Recreate container
docker compose up -d --force-recreate [service-name]
```

### Tailscale Connection Issues

```bash
# Check Tailscale status
docker compose exec tailscale-arr tailscale status

# Re-authenticate
docker compose exec tailscale-arr tailscale up --authkey=$TS_AUTHKEY
```

### Network Issues in Tailscale Mode

```bash
# Verify network mode
docker compose config | grep network_mode

# Check if services can communicate
docker compose exec tailscale-arr ping [service-name]
```

### Reset Everything

```bash
# Stop and remove all containers, networks, volumes
docker compose down -v

# Remove all images
docker compose down --rmi all

# Start fresh
docker compose up -d
```

## 🎯 Best Practices

1. **Regular Backups**: Schedule automated backups of your configuration directories
2. **Update Regularly**: Keep services updated for security patches and new features
3. **Monitor Disk Space**: Media files can grow quickly; monitor available storage
4. **Use Strong Passwords**: Change default passwords immediately after setup
5. **Enable 2FA**: Where supported, enable two-factor authentication
6. **Review Logs**: Periodically check logs for errors or warnings
7. **Test Backups**: Regularly test restore procedures
8. **Document Changes**: Keep notes on customizations and configurations

## 🔒 Security Considerations

- **Change Default Credentials**: All services with web interfaces
- **Use Tailscale ACLs**: Restrict access to specific devices/users
- **Enable Authentication**: Configure auth in qBittorrent and Jellyfin
- **Keep Updated**: Regularly update all containers
- **Use Reverse Proxy**: Consider Caddy/Nginx for additional security layers
- **Firewall Rules**: Configure host firewall appropriately
- **VPN for Torrents**: Consider VPN integration for qBittorrent

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request. For major changes, please open an issue first to discuss what you would like to change.

### How to Contribute

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [LinuxServer.io](https://www.linuxserver.io/) for maintaining excellent Docker images
- [Tailscale](https://tailscale.com/) for making secure networking simple
- The entire *arr stack development teams
- [Jellyfin](https://jellyfin.org/) community
- All contributors and users of this homelab setup

## 📞 Support

- **Issues**: [GitHub Issues](https://github.com/SinghChinmayy/homelab/issues)
- **Discussions**: [GitHub Discussions](https://github.com/SinghChinmayy/homelab/discussions)

## 🔗 Useful Links

- [Docker Documentation](https://docs.docker.com/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [Tailscale Documentation](https://tailscale.com/kb/)
- [Servarr Wiki](https://wiki.servarr.com/)
- [Jellyfin Documentation](https://jellyfin.org/docs/)
- [Homarr Documentation](https://homarr.dev/)

---

<div align="center">

**[⬆ Back to Top](#-homelab)**

Made with ❤️ for the self-hosting community

</div>
