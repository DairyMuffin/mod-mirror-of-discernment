# Deployment Guide — Mirror of Discernment (MoD)

## 1. Prerequisites
- Docker & Docker Compose installed  
- A host/server (Linux VM, AWS EC2, DigitalOcean droplet, etc.)  
- Domain name or public IP  
- SSL certificate (Let’s Encrypt or similar)

---

## 2. Repository Structure


---

## 3. Environment Configuration
1. **Create a `.env`** at project root:
   ```ini
   CAA_HOST=caa
   VIP_API_KEY=YOUR_VIP_API_KEY
   DASHBOARD_PORT=8501

version: '3.8'
services:
  caa:
    image: dairymuffin/caa:1.0
    environment:
      - VIP_API_KEY=${VIP_API_KEY}
    ports:
      - "8080:8080"

  dashboard:
    image: yourdocker/qsbm-dashboard:latest
    environment:
      - CAA_URL=http://caa:8080/scan
    ports:
      - "${DASHBOARD_PORT}:8501"

  mod-gateway:
    build: .
    depends_on:
      - caa
      - dashboard
    ports:
      - "3000:3000"
    environment:
      - CAA_URL=http://caa:8080/scan
      - DASHBOARD_URL=http://dashboard:${DASHBOARD_PORT}

  ar-client:
    build:
      context: ar-client
    ports:
      - "8081:80"
    environment:
      - GATEWAY_URL=http://mod-gateway:3000

eof
