# Deploy Patchbowl on DigitalOcean

This guide walks through three ways to self-host Patchbowl on DigitalOcean, from easiest to most flexible.

> 💸 **Cost:** A `$6/mo` basic droplet runs Patchbowl comfortably for a solo dev. Team deployments can use `$12–24/mo` droplets.

---

## Option 1 — One-click App Platform (easiest)

The fastest path. DigitalOcean App Platform handles the build, deploy, TLS, and updates for you.

[![Deploy to DO](https://www.deploytodo.com/do-btn-blue.svg)](https://cloud.digitalocean.com/apps/new)

1. Click the button above (you'll need a DO account).
2. App Platform reads `.do/app.yaml` from this repo (coming soon) and provisions:
   - A web service running the Patchbowl container
   - A managed volume for persistent SQLite + secrets storage
3. Pick a region near you. App Platform assigns an `ondigitalocean.app` URL.
4. Open the URL. Done.

**Pros:** Zero ops. Auto TLS. Auto restarts.
**Cons:** Slightly more expensive than a raw droplet (~$12/mo entry). Less control.

---

## Option 2 — Droplet + Docker (recommended)

Best balance of cost, control, and simplicity.

### 1. Create the droplet

- **Image:** Ubuntu 24.04 LTS
- **Size:** Basic — Regular Intel, 1 GB RAM / 1 vCPU (`$6/mo`)
- **Region:** closest to you
- **SSH key:** add yours during creation
- **Hostname:** `patchbowl`

### 2. Install Docker

```bash
ssh root@<droplet-ip>

# Install Docker
curl -fsSL https://get.docker.com | sh

# Enable on boot
systemctl enable --now docker
```

### 3. Run Patchbowl

```bash
# Create a persistent volume for the database + secrets
docker volume create patchbowl-data

# Run Patchbowl
docker run -d \
  --name patchbowl \
  --restart unless-stopped \
  -p 80:3000 \
  -v patchbowl-data:/data \
  -e PATCHBOWL_ADMIN_PASSWORD=<choose-a-strong-password> \
  ghcr.io/patchbowl/patchbowl:latest
```

Open `http://<droplet-ip>` and log in.

### 4. Add HTTPS (recommended)

Point a domain at the droplet, then put Caddy in front of Patchbowl for automatic Let's Encrypt:

```bash
# Install Caddy
apt install -y caddy

# Edit /etc/caddy/Caddyfile
cat > /etc/caddy/Caddyfile <<EOF
patchbowl.yourdomain.com {
  reverse_proxy localhost:3000
}
EOF

# Restart
docker stop patchbowl
docker run -d \
  --name patchbowl \
  --restart unless-stopped \
  -p 127.0.0.1:3000:3000 \
  -v patchbowl-data:/data \
  -e PATCHBOWL_ADMIN_PASSWORD=<strong-password> \
  ghcr.io/patchbowl/patchbowl:latest

systemctl reload caddy
```

Open `https://patchbowl.yourdomain.com`.

---

## Option 3 — DigitalOcean Kubernetes (DOKS)

For teams running Patchbowl alongside other services. A Helm chart will live at `charts/patchbowl/` in this repo (coming with v0.2).

```bash
helm repo add patchbowl https://patchbowl.github.io/charts
helm install patchbowl patchbowl/patchbowl
```

---

## Backups

Whatever option you pick, **back up `/data`**. It contains:

- `patchbowl.db` — your SQLite database (server configs, install history)
- `secrets.enc` — your encrypted API keys

A nightly `doctl` snapshot of the droplet (or volume) is enough for most solo users. For team deployments, dump the DB and rsync off-box.

---

## Troubleshooting

- **Can't reach the droplet on port 80:** check the DO firewall — allow HTTP/80 and HTTPS/443 inbound.
- **Container restarts in a loop:** `docker logs patchbowl` will show why. Most often: missing `PATCHBOWL_ADMIN_PASSWORD` env var.
- **Lost the admin password:** SSH in, stop the container, run with `-e PATCHBOWL_RESET_ADMIN=1` once.

---

## Why DigitalOcean?

Patchbowl is sponsored in part by DigitalOcean's Open Source program. We design our deploy path to be excellent on DO first, but Patchbowl runs anywhere Docker runs — AWS, GCP, Hetzner, your homelab, your laptop. No lock-in.

Need help? Open an [issue](../../../../issues) or ask in [Discord](#).
