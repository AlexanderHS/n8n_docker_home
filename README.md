# n8n Docker Deployment

Production-ready n8n workflow automation platform with PostgreSQL backend and automatic HTTPS.

## Quick Start

This is a **remote deployment setup**. Configuration changes are made locally, then deployed remotely.

### Prerequisites

- Docker and Docker Compose on remote server
- Cloudflare DNS API token
- Domain `n8n.2ho.me` pointing to your server

### Remote Deployment

1. **Clone this repository** on your remote server
2. **Set environment variable**:
   ```bash
   export CF_DNS_API_TOKEN=your_cloudflare_api_token
   ```
3. **Start the stack**:
   ```bash
   docker-compose up -d
   ```
4. **Access n8n** at: https://n8n.2ho.me
   - Username: `admin`
   - Password: `admin123`

## Architecture

- **n8n**: Workflow automation platform (`n8nio/n8n:latest`)
- **PostgreSQL**: Database backend (PostgreSQL 15)  
- **Traefik**: Reverse proxy with automatic HTTPS via Let's Encrypt + Cloudflare DNS challenge

## Management Commands

```bash
# View logs
docker-compose logs -f

# Stop services
docker-compose down

# Update and restart
docker-compose pull && docker-compose up -d --force-recreate

# Service-specific logs
docker-compose logs -f n8n
docker-compose logs -f postgres
docker-compose logs -f traefik
```

## Data Persistence

All data is stored in local directories:
- `./data/` - n8n workflows, credentials, settings
- `./postgres_data/` - PostgreSQL database
- `./letsencrypt/` - SSL certificates

## Security

- HTTPS enforced via Traefik with Let's Encrypt certificates
- Basic authentication enabled for n8n interface
- PostgreSQL isolated on internal Docker network
- Traefik dashboard disabled for security

## Development Workflow

1. Make changes to configuration files locally
2. Commit and push to Git repository
3. Pull changes on remote server
4. Restart services: `docker-compose up -d --force-recreate`

---

For detailed technical information and Claude Code guidance, see [CLAUDE.md](CLAUDE.md).