# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Architecture Overview

This repository contains a Docker Compose configuration for running n8n (workflow automation platform) with supporting services:

- **n8n**: The main workflow automation service (`n8nio/n8n:latest`)
- **PostgreSQL**: Database backend for n8n data persistence
- **Traefik**: Reverse proxy with automatic HTTPS via Cloudflare DNS challenge

The setup is configured for production deployment with:
- SSL termination through Traefik with Let's Encrypt certificates
- PostgreSQL as the database backend (instead of SQLite)
- Basic authentication enabled for n8n
- Custom domain routing (`n8n.2ho.me`)

## Common Development Commands

### Starting the Stack
```bash
docker-compose up -d
```

### Stopping the Stack
```bash
docker-compose down
```

### Viewing Logs
```bash
# All services
docker-compose logs -f

# Specific service
docker-compose logs -f n8n
docker-compose logs -f postgres
docker-compose logs -f traefik
```

### Rebuilding Services
```bash
docker-compose pull
docker-compose up -d --force-recreate
```

## Environment Requirements

- `CF_DNS_API_TOKEN`: Cloudflare API token for DNS challenge (required for SSL certificates)
- Domain `n8n.2ho.me` must be configured to point to the server

## Data Persistence

- **n8n data**: `./data` directory (workflows, credentials, settings)
- **PostgreSQL data**: `./postgres_data` directory
- **SSL certificates**: `./letsencrypt` directory

## Access Information

- **n8n Interface**: https://n8n.2ho.me
- **Default Credentials**: admin / admin123 (configured via environment variables)
- **Database**: PostgreSQL with credentials n8n / n8npass

## Container Architecture

All services run in Docker containers with specific networking:
- Traefik exposes port 443 for HTTPS traffic
- n8n connects to PostgreSQL via internal Docker network
- Traefik automatically discovers n8n service via Docker labels