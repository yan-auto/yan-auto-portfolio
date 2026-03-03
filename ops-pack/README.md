# Ops Pack v0.1 — Self-hosted n8n (Production Basics)

This document records the current production setup and the minimum operational knowledge required to maintain it safely.

## 1) Entry & Security Baseline
- Public domain: `n8n.yan-auto.me`
- Reverse proxy: Caddy (HTTPS enabled)
- Public ports: 22 / 80 / 443 only (UFW enabled)
- n8n is NOT exposed directly to the public internet (no host port mapping for 5678)

## 2) Deployment (Docker Compose)
- Host: Ubuntu 24.04 (VPS)
- Compose path: `/root/n8n-docker/docker-compose.yml`
- Services:
  - `caddy:latest` (reverse proxy)
  - `docker.n8n.io/n8nio/n8n` (current version: `2.8.3`)

## 3) Data & Critical Files (Most Important)
### Docker volumes
- `n8n-docker_n8n_data`
  - Mountpoint: `/var/lib/docker/volumes/n8n-docker_n8n_data/_data`
- `n8n-docker_caddy_data`
  - Mountpoint: `/var/lib/docker/volumes/n8n-docker_caddy_data/_data`
- `n8n-docker_caddy_config`
  - Mountpoint: `/var/lib/docker/volumes/n8n-docker_caddy_config/_data`

### n8n internal storage
Inside the n8n container: `/home/node/.n8n/`
Key files:
- `config` (contains encryptionKey; **required to decrypt credentials after restore**)
- `database.sqlite` (+ `database.sqlite-wal` / `database.sqlite-shm`)
- `n8nEventLog.log`

## 4) Operational Notes
- TLS certificates are handled automatically by Caddy.
- Current DB is SQLite (sufficient for small scale; backup must include sqlite + wal/shm).
- Next milestone: run a full backup + restore drill (Day 2).

## 5) Backup & Restore Drill (Day 2)
### Backup (VPS)
- Backup target: Docker volume `n8n-docker_n8n_data`
- Backup command (tar):
  - Archive includes: `config`, `database.sqlite`, `database.sqlite-wal`, `database.sqlite-shm`
- Example backup file:
  - `n8n_data_2026-03-03_130846.tar.gz`
  - SHA256: `c4a54d24a571f10e1ad225dec81f23c00ee22acf253039cd491cb26fc7336eb3`

### Restore (Local shadow environment)
Goal: restore into a NEW docker volume and run n8n on `localhost:5679` (no impact to production).

- Create restore volume:
  - `docker volume create n8n_restore_data`
- Extract archive into volume (alpine):
  - mount volume at `/data`
  - mount backup folder at `/backup`
  - `tar -xzf ...` into `/data`
- Start local n8n (same version as production):
  - Image: `docker.n8n.io/n8nio/n8n:2.8.3`
  - Port mapping: `5679:5678`
- Verification:
  - `http://localhost:5679` returns 200 and editor is accessible
