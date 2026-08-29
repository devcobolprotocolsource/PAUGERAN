---
title: DEPLOY-GUIDE — Panduan Deployment
document_id: DEPLOY-GUIDE
version: 1.0
cb_reference: [CB §23], [CB §31]
status: DRAFT
owner: DevOps Team
last_updated: 2026-08-29
---

# DEPLOY-GUIDE — Panduan Deployment

Panduan deployment untuk setiap skenario.

## Referensi CB
- [CB §23] — Deployment architecture
- [CB §31] — Operations requirements

---

## Deployment Scenarios

### 1. Local Laptop (Binary Standalone)

**Target:** Individual users, offline work

```bash
# Download
curl -L https://releases.paugeran.dev/paugeran-macos-arm64.zip -o paugeran.zip
unzip paugeran.zip

# Run
./paugeran

# Access at http://localhost:3000
```

**Requirements:**
- 500MB disk space
- 200MB RAM
- macOS 11+, Windows 10+, Ubuntu 20.04+

**License:** Offline license required

---

### 2. Docker Local

**Target:** Development, testing

```bash
docker run -p 3000:3000 \
  -e PAUGERAN_DB=sqlite:///data/paugeran.db \
  -v ~/.paugeran:/data \
  paugeran/app:latest
```

**docker-compose.yml:**

```yaml
version: '3.8'
services:
  paugeran:
    image: paugeran/app:latest
    ports:
      - "3000:3000"
    volumes:
      - paugeran-data:/data
    environment:
      DATABASE_URL: sqlite:///data/paugeran.db
      
volumes:
  paugeran-data:
```

---

### 3. Railway (One-Click Deploy)

**Target:** Non-technical users

1. Click: https://railway.app/template/paugeran
2. Connect GitHub account
3. Deploy automatically
4. Get public URL

**Includes:**
- PostgreSQL database
- HTTPS/SSL
- Auto-scaling
- Backups

---

### 4. VPS with Nginx

**Target:** Self-hosted, production-ready

```bash
# 1. SSH to VPS
ssh user@example.com

# 2. Install dependencies
sudo apt update
sudo apt install -y postgresql nginx docker.io

# 3. Create PostgreSQL database
sudo -u postgres createdb paugeran

# 4. Deploy Docker container
docker pull paugeran/app:latest
docker run -d \
  --name paugeran \
  -e DATABASE_URL=postgresql://user:pass@localhost/paugeran \
  -e PORT=3001 \
  paugeran/app:latest

# 5. Configure Nginx
sudo tee /etc/nginx/sites-available/paugeran > /dev/null <<EOF
server {
    listen 80;
    server_name example.com;
    
    location / {
        proxy_pass http://localhost:3001;
        proxy_set_header Host \$host;
    }
}
EOF

sudo systemctl restart nginx

# 6. Install SSL (Let's Encrypt)
sudo certbot certonly -d example.com
# Update Nginx to use SSL
```

---

### 5. Homelab with Tailscale/Cloudflare

**Target:** Private network access

```bash
# Install Tailscale
curl -fsSL https://tailscale.com/install.sh | sh

# Connect to Tailscale network
sudo tailscale up

# Get Tailscale IP (e.g., 100.x.x.x)
tailscale ip -4

# Access at: http://100.x.x.x:3000
# From any device on your Tailscale network
```

**Cloudflare Tunnel (Alternative):**

```bash
# Install cloudflared
curl -L https://github.com/cloudflare/cloudflared/releases/download/latest/cloudflared-linux-amd64.deb

# Create tunnel
cloudflared tunnel create paugeran

# Configure
echo "url: http://localhost:3000" > config.yml

# Run
cloudflared tunnel run paugeran

# Access at: paugeran.example.com (via Cloudflare)
```

---

### 6. Tauri Desktop App

**Target:** Desktop users (Windows, macOS, Linux)

```bash
# Development
npm run tauri dev

# Build
npm run tauri build

# Output: 
# - src-tauri/target/release/paugeran (executable)
# - Installer (.msi for Windows, .dmg for macOS, .AppImage for Linux)

# Distribute via GitHub Releases
gh release create v1.0.0 ./target/release/bundle/*
```

---

### 7. Air-Gapped Deployment

**Target:** Secure, offline environments

**Setup:**
1. Export data on connected machine
2. Create encrypted USB drive
3. Transfer to air-gapped machine
4. Import data and run offline

```bash
# On connected machine
paugeran export --format backup --output backup.enc

# On air-gapped machine
paugeran import --file backup.enc

# Use offline license
paugeran activate --license-file paugeran.license --offline
```

---

## Verification Checklists

### Post-Deployment Verification

- [ ] App starts without errors
- [ ] Database initialized
- [ ] License validated
- [ ] UI accessible at expected URL
- [ ] Can create new chat
- [ ] Can search knowledge base
- [ ] Export functionality works
- [ ] All configured LLM providers reachable
- [ ] HTTPS/SSL working (if configured)
- [ ] Backups configured
- [ ] Logging enabled and working
- [ ] Monitoring/alerts configured

### Security Verification

- [ ] API keys not in logs
- [ ] Database credentials in environment vars only
- [ ] HTTPS enforced
- [ ] CORS properly configured
- [ ] Rate limiting working
- [ ] License validation working
- [ ] No debug mode in production
- [ ] Backup encryption enabled

### Performance Verification

- [ ] Response time < 30s for chat
- [ ] Export time < 5s
- [ ] Search time < 500ms
- [ ] Memory usage stable
- [ ] No memory leaks after 24h operation

---

## Troubleshooting

### Common Issues

| Issue | Diagnosis | Fix |
|-------|-----------|-----|
| "Port already in use" | `lsof -i :3000` | Kill process or use different port |
| "Database connection failed" | Check DATABASE_URL | Verify PostgreSQL running |
| "License invalid" | Check license file | Generate new offline license |
| "LLM provider timeout" | Test API key | Check internet/firewall |
| "High memory usage" | Check logs | Restart or scale up |

---

## Checklist Implementasi

- [ ] Binary builds automated (GitHub Actions)
- [ ] Docker image builds and pushes
- [ ] Railway template setup
- [ ] VPS deployment docs with examples
- [ ] Desktop app (Tauri) building
- [ ] SSL/HTTPS certificates managed
- [ ] Monitoring dashboards configured
- [ ] Backup procedures tested
- [ ] Rollback procedures documented
- [ ] Deployment runbook created

