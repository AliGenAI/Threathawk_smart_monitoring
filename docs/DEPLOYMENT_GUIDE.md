# 🚀 Deployment Options Guide

This guide helps you choose the right deployment method for your needs.

## 🎯 Which Deployment Should You Choose?

### 🖥️ **Local Mode** - For Individual Users

**Choose this if you:**
- Are an individual security analyst
- Want to use Claude Desktop locally
- Need quick setup with minimal complexity
- Don't require team collaboration
- Want to run on your laptop/workstation

**Setup Time:** ~5 minutes  
**Complexity:** Low  
**Infrastructure:** None required

```bash
# Quick setup
pip install -e .
python wazuh_mcp_server.py --stdio
```

### 🌐 **Production Mode** - For Teams & Organizations

**Choose this if you:**
- Have multiple team members using AI tools
- Need centralized access control and monitoring
- Want to integrate with other applications
- Require production-grade reliability
- Need to scale across multiple clients

**Setup Time:** ~15 minutes  
**Complexity:** Medium  
**Infrastructure:** Docker, domain name (FQDN) OR IP address (IP mode)

```bash
# FQDN-based deployment (with SSL)
./deploy.sh deploy

# IP-based deployment (private networks)
./deploy-local-ip.sh deploy
```

## 📊 Feature Comparison

| Feature | Local Mode | Production (FQDN) | Production (IP/Port) |
|---------|------------|-------------------|---------------------|
| **Setup Complexity** | Simple | Medium | Medium-Low |
| **Claude Desktop Integration** | ✅ Native | ✅ Via HTTP/WS | ✅ Via HTTP/WS |
| **Multiple Users** | ❌ Single user | ✅ Multi-user | ✅ Multi-user |
| **Authentication** | Environment variables | JWT + API keys | JWT + API keys |
| **Monitoring** | Basic logs | Prometheus + Grafana | Prometheus + Grafana |
| **SSL/TLS** | Optional | ✅ Automatic (Let's Encrypt) | Optional (self-signed) |
| **Rate Limiting** | None | Configurable | Configurable |
| **High Availability** | ❌ | ✅ Load balanced | ✅ Load balanced |
| **External API Access** | ❌ | ✅ HTTP/WebSocket | ✅ HTTP/WebSocket |
| **Domain Requirements** | ❌ None | ✅ FQDN required | ❌ IP address only |
| **Private Network** | ✅ Local only | ❌ Public domain needed | ✅ LAN/VPN friendly |
| **Resource Usage** | Low | Medium-High | Medium |
| **Production Ready** | Development | ✅ Enterprise | ✅ Enterprise |

## 🛠️ Technical Requirements

### Local Mode Requirements
- Python 3.9+
- Claude Desktop application
- Network access to Wazuh server
- ~100MB RAM, minimal CPU

### Production Mode Requirements

#### FQDN-Based (Traditional)
- Docker & Docker Compose
- Domain name (for SSL certificates)
- 2GB+ RAM, 1+ CPU cores
- Ports 80, 443 open (for SSL)
- Network access to Wazuh server

#### IP/Port-Based (Private Networks)
- Docker & Docker Compose
- Static IP address (public or private)
- 1GB+ RAM, 1+ CPU cores
- Custom ports open (default: 8000, 8001)
- Network access to Wazuh server

## 🚀 Quick Start by Use Case

### 🔍 **Security Analyst (Individual)**
```bash
# Local setup for Claude Desktop
git clone https://github.com/gensecaihq/Wazuh-MCP-Server.git
cd Wazuh-MCP-Server
pip install -e .
cp .env.example .env
# Edit .env with your Wazuh credentials
# Add to Claude Desktop config (see LOCAL_SETUP.md)
```

### 🏢 **Security Team (Organization)**

#### Option A: FQDN-Based (Public Access)
```bash
# Production deployment for team access with domain
git clone https://github.com/gensecaihq/Wazuh-MCP-Server.git
cd Wazuh-MCP-Server
cp .env.production.example .env.production
# Edit .env.production with your domain and configuration
./deploy.sh deploy
```

#### Option B: IP-Based (Private Network)
```bash
# Production deployment for team access with IP
git clone https://github.com/gensecaihq/Wazuh-MCP-Server.git
cd Wazuh-MCP-Server
cp .env.local-ip.example .env.local-ip
# Edit .env.local-ip with your IP and configuration
./deploy-local-ip.sh deploy
```

### 🧑‍💻 **Developer (Testing)**
```bash
# Local development with both options
pip install -e .

# Test local mode
python wazuh_mcp_server.py --stdio

# Test production mode
./deploy.sh deploy
```

## 🔄 Migration Path

### From Local to Production

If you start with local mode and want to upgrade:

1. **Backup your local configuration**:
   ```bash
   cp .env .env.local.backup
   ```

2. **Create production configuration**:
   ```bash
   cp .env.production.example .env.production
   # Copy relevant settings from .env to .env.production
   ```

3. **Deploy production mode**:
   ```bash
   ./deploy.sh deploy
   ```

4. **Update client configurations** to use HTTP/WebSocket APIs

## 🔧 Configuration Examples

### Local Mode Claude Desktop Config
```json
{
  "mcpServers": {
    "wazuh-security": {
      "command": "python",
      "args": ["/path/to/wazuh_mcp_server.py", "--stdio"],
      "env": {
        "WAZUH_HOST": "wazuh.company.com",
        "WAZUH_USER": "analyst",
        "WAZUH_PASS": "password"
      }
    }
  }
}
```

### Production Mode API Access
```bash
# Get authentication token
curl -X POST https://mcp.company.com/auth/login \
     -d '{"api_key": "your-api-key"}'

# Use authenticated API
curl -H "Authorization: Bearer TOKEN" \
     https://mcp.company.com/tools/get_alerts/call
```

## 🚨 Common Mistakes to Avoid

### ❌ **Don't Do This**
- Use local mode for production team access
- Run production mode without SSL in production
- Share credentials in local mode setup
- Skip monitoring in production deployments

### ✅ **Best Practices**
- Start with local mode for evaluation
- Use production mode for team deployments
- Always use environment variables for credentials
- Monitor production deployments with Grafana
- Use strong JWT secrets in production
- Implement proper firewall rules

## 🆘 Getting Help

### Local Mode Issues
- See [LOCAL_SETUP.md](LOCAL_SETUP.md)
- Check Python and dependency versions
- Verify Wazuh server connectivity

### Production Mode Issues  
- See [REMOTE_SETUP.md](REMOTE_SETUP.md)
- Check Docker and networking
- Verify domain DNS configuration
- Monitor logs: `./deploy.sh logs`

## 📈 Scaling Considerations

### When to Choose Production Mode

**Team Size:**
- 1 user → Local mode
- 2-10 users → Production mode (single instance)
- 10+ users → Production mode (scaled)

**Usage Patterns:**
- Occasional analysis → Local mode
- Daily team operations → Production mode
- 24/7 monitoring → Production mode (HA)

**Integration Needs:**
- Claude Desktop only → Local mode
- Multiple AI tools → Production mode
- Custom applications → Production mode

## 🔮 Future Considerations

Both deployment modes support:
- All Wazuh MCP Server features
- Future updates and enhancements
- Migration between modes
- Integration with new AI platforms

Choose based on your **current needs**, but both options provide **full functionality** and **upgrade paths** for future growth.