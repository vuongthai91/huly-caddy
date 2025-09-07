# Huly – Deployment with Caddy & Coolify

Huly is a self-hosted platform composed of multiple services (front, account, workspace, transactor, fulltext, stats, collaborator, rekoni) and infrastructure dependencies (CockroachDB, Redpanda, MinIO, Elasticsearch).
Caddy is used as a lightweight reverse proxy inside the stack. When deployed with Coolify, Traefik serves as the external ingress, while Caddy handles internal routing.

---
## Requirements

* Docker 24+ and Docker Compose v2+
* Coolify v4+ for deployment with domain/DNS configured
* Server without restrictive VPN/MTU issues (recommended Docker daemon MTU 1450)

## Coolify Deployment
## Add application in Coolify
1. Log into your Coolify dashboard.
2. Go to **Applications → New Application**.
3. Choose **Public Git Repository**.
4. Paste your repo URL (e.g. `https://github.com/vuongthai91/huly-caddy`).
5. Select branch (`main` or `master`).
6. Set the **Deployment Type** to **Docker Compose**.
7. Coolify will pull the repo, detect `docker-compose.yml`, and prepare the stack.
---

## Configure environment variables

* In the Coolify app settings, go to **Environment Variables**.
```
- SERVICE_FQDN_FRONT=huly.examlple.com
- SERVICE_URL_FRONT=https://huly.examlple.com
- CR_DB_URL='postgresql://${CR_USERNAME}:${CR_USER_PASSWORD}@cockroach:26257/${CR_DATABASE}?sslmode=disable'
- DOCKER_NAME=huly
- HULY_VERSION=s0.7.204
- SECRET=changeme-secret-key
- HOST_ADDRESS=${SERVICE_FQDN_CADDY}
- TITLE=Huly Self Host
- DEFAULT_LANGUAGE=en
- LAST_NAME_FIRST=true
- SECURE=false
- CR_DATABASE=defaultdb
- CR_USERNAME=selfhost
- CR_USER_PASSWORD=REPLACE_CR_USER_PASSWORD
- VOLUME_CR_DATA_PATH=cr_data
- VOLUME_CR_CERTS_PATH=cr_certs
- VOLUME_ELASTIC_PATH=elastic
- VOLUME_FILES_PATH=files
- VOLUME_REDPANDA_PATH=redpanda
- REDPANDA_ADMIN_USER=admin
- REDPANDA_ADMIN_PWD=REPLACE_REDPANDA_ADMIN_PASS
- MINIO_ROOT_USER=minioadmin
- MINIO_ROOT_PASSWORD=REPLACE_MINIO_ROOT_PASSWORD
- ELASTIC_PASSWORD=REPLACE_ELASTIC_PASSWORD
- HULY_DOMAIN=${HOST_ADDRESS}
- STREAM_URL=
```

---

## 4. Domain & Networking

* Assign your domain (e.g. `huly.examlple.com`) to the **Caddy service** or directly to `front` in Coolify.
* Coolify’s Traefik will handle HTTPS (Let’s Encrypt).
* You don’t need to expose ports manually (Coolify manages ingress).