# S500 Web-Based Workspace Setup Guide

## Overview

This guide sets up a complete web-based development environment for the S500 project with Docker containers. The setup includes:

- **Web Dashboard** - Visual interface for project management
- **File Server** - RAR extraction and workspace management
- **API Server** - Backend API for workspace operations
- **Build Environment** - .NET/C# build tools

## Prerequisites

- **Docker** (19.03+) - [Install Docker](https://docs.docker.com/get-docker/)
- **Docker Compose** (1.29+) - [Install Compose](https://docs.docker.com/compose/install/)
- **Git** - For cloning the repository
- 2GB+ available disk space
- 1GB+ RAM recommended

## Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/MarketMarven/s500.git
cd s500
```

### 2. Start the Development Environment

```bash
# Start all services with Docker Compose
docker-compose up -d

# View logs
docker-compose logs -f
```

### 3. Access the Dashboard

Open your browser and navigate to:

```
http://localhost:3000
```

## Project Structure

```
s500/
├── docker-compose.yml          # Docker services configuration
├── web-dashboard/              # Express.js dashboard
│   ├── server.js               # Dashboard server
│   ├── package.json            # Node dependencies
│   └── views/                  # EJS templates
├── scripts/
│   └── file_server.py          # Python file management server
├── extracted/                  # RAR extraction output
├── build/                      # Build output
└── SETUP_GUIDE.md              # This file
```

## Services Overview

### 1. Web Dashboard (Port 3000)
- Main user interface for workspace management
- Project navigation and overview
- Real-time statistics and status

**Access:** http://localhost:3000

### 2. File Server (Port 8000)
- RAR file extraction capabilities
- Workspace file management
- Project discovery and listing

**API Endpoint:** http://localhost:8000/api

**Available Endpoints:**
- `GET /api/health` - Health check
- `GET /api/workspace` - Get workspace structure
- `POST /api/extract` - Extract RAR file
- `GET /api/extracted` - List extracted files
- `GET /api/projects` - List C# projects
- `GET /api/build-info` - Get build information

### 3. API Server (Port 5001)
- RESTful API for workspace operations
- Build and test management
- Project information and metadata

**API Endpoint:** http://localhost:5001/api

### 4. .NET Builder
- Visual Studio build tools integration
- C# compilation and project building
- MSBuild support

## Common Operations

### Extract RAR Files

**Via API:**
```bash
curl -X POST http://localhost:8000/api/extract \
  -H "Content-Type: application/json" \
  -d '{"file": "S500HVNC.rar"}'
```

**Via Dashboard:**
1. Navigate to Workspace tab
2. Find RAR file in list
3. Click "Extract" button

### View Workspace Structure

```bash
curl http://localhost:8000/api/workspace
```

### List All Projects

```bash
curl http://localhost:8000/api/projects
```

### Check System Health

```bash
curl http://localhost:8000/api/health
```

## Docker Commands

### Start Environment

```bash
docker-compose up -d
```

### Stop Environment

```bash
docker-compose stop
```

### View Logs

```bash
# All services
docker-compose logs -f

# Specific service
docker-compose logs -f workspace-dashboard
```

### Connect to Container

```bash
docker-compose exec workspace-dashboard sh
```

### Rebuild Images

```bash
docker-compose build --no-cache
```

### Remove Everything

```bash
docker-compose down -v
```

## Troubleshooting

### Docker Compose won't start

```bash
# Check Docker daemon status
docker ps

# View detailed logs
docker-compose logs

# Restart services
docker-compose restart
```

### Can't access dashboard at localhost:3000

**Check if port is in use:**
```bash
# macOS/Linux
lsof -i :3000

# Windows
netstat -ano | findstr :3000
```

**Check container status:**
```bash
docker-compose ps
docker-compose logs workspace-dashboard
```

### RAR extraction fails

**Install extraction tools:**
```bash
# macOS
brew install unar

# Linux (Ubuntu/Debian)
sudo apt-get install unar

# Linux (Fedora/RedHat)
sudo dnf install unar
```

### Out of disk space

```bash
# Clean extracted files
rm -rf extracted/*

# Clean Docker system
docker system prune -a

# Restart services
docker-compose restart
```

## Environment Variables

Create a `.env` file to customize settings:

```env
# Web Dashboard
PORT=3000
NODE_ENV=development

# File Server
FILE_SERVER_PORT=8000
EXTRACTION_PATH=/extracted

# API Server
API_PORT=5001

# Build Configuration
DOTNET_VERSION=4.8
VS_VERSION=2022
```

Run with custom environment:
```bash
docker-compose --env-file .env up
```

## Development Workflow

### Step 1: Extract Source Code
- Open Dashboard at http://localhost:3000
- Go to "Workspace" tab
- Select RAR file and click "Extract"

### Step 2: Browse Projects
- Go to "Projects" tab
- View all discovered C# projects
- Click on project for details

### Step 3: Build Projects
- Go to "Build & Test" tab
- Select project from list
- Click "Build Solution"

### Step 4: Review Build Results
- Check build logs and status
- View any errors or warnings
- Download build artifacts if available

## Advanced Usage

### Multiple Instances

Scale API server:
```bash
docker-compose up -d --scale api-server=3
```

### Memory Management

Limit container memory:
```bash
docker update --memory 512m s500-workspace
```

### Monitor Performance

View real-time stats:
```bash
docker stats
```

## File System Layout After Extraction

```
extracted/
├── S500HVNC/
│   ├── S500/
│   │   ├── S500.sln
│   │   ├── S500/
│   │   │   ├── S500.csproj
│   │   │   ├── *.cs files
│   │   │   └── ...
│   │   └── ...
│   └── ...
└── app/
    ├── [Extracted app files]
    └── ...
```

## Security Considerations

⚠️ **For Development Only**
- Dashboard runs on localhost by default
- No authentication enabled by default
- File access unrestricted

**For Production:**
- Add authentication layer
- Enable SSL/TLS
- Restrict file access with permissions
- Use environment-specific secrets

## Next Steps

1. **Start Services** - Run `docker-compose up -d`
2. **Extract RAR Files** - Use Dashboard Workspace tab
3. **Review Projects** - Browse extracted C# projects
4. **Local Development** - Open projects in Visual Studio
5. **Build & Test** - Use Build tab or Visual Studio

## Resources

- **Docker Documentation**: https://docs.docker.com
- **Docker Compose**: https://docs.docker.com/compose
- **Visual Studio**: https://visualstudio.microsoft.com
- **S500 Repository**: https://github.com/MarketMarven/s500

## Support

For issues:
1. Check logs: `docker-compose logs`
2. Verify Docker installation: `docker --version`
3. Review this guide for troubleshooting
4. Check Docker Hub for updated images

---

**Happy developing!** 🚀