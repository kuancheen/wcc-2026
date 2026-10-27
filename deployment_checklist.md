# WCC 2026 — Multi-Machine Setup & Deployment Checklist

Operational checklist and guide for engineers and Antigravity agents developing, deploying, or testing the World Changers Conference 2026 landing page across different local environments and remote Apache servers.

---

## 1. Machine & Environment Profiles

### Local Machine A (Original Workstation - `kuancheen`)
- **Workspace Path:** `/Users/kuancheen/Documents/bash/wcc`
- **SSH Key Location:** `~/.ssh/` or `~/Documents/bash/.ssh/`

### Local Machine B (Current Workstation - `admin` / iMac 2017 KC)
- **Workspace Path:** `/Users/admin/Documents/antigravity/bash/wcc`
- **SSH Key Location:** `/Users/admin/Documents/antigravity/bash/.ssh/`

### Production Server Target
- **Server:** Apache 2.4 (Ubuntu/Debian) hosting `actschurchconference.com`
- **Document Root:** `/var/www/html`
- **Target Application Path:** `/var/www/html/wcc/`
- **Public URL:** `https://actschurchconference.com/wcc/`

---

## 2. Pre-Deployment Readiness Checklist (New Machine Setup)

Before triggering deployments or modifying assets on a new machine:

### [x] 1. Repository & Submodule Checkout
Ensure the submodule repository is checked out and up to date:
```bash
git submodule update --init --recursive wcc
cd wcc
git status
```

### [ ] 2. Static Asset Verification
Verify that all core images and markup exist locally:
```bash
test -f index.html && echo "index.html: OK"
test -d assets/images/gallery04 && echo "gallery04: OK"
```

### [ ] 3. External Endpoint Validation
Verify that external service links are intact in `index.html`:
- **Luma Ticket Registration:** `https://luma.com/o6mqew3f`
- **Email Contact:** `mailto:ywa@actschurch.org`

---

## 3. Deployment Methods

### Method A: Direct Git Deployment to Apache Server (Recommended)
If the web server is connected via Git:
```bash
# On remote web server:
cd /var/www/html/wcc
git pull origin main
sudo chown -R www-data:www-data /var/www/html/wcc
sudo chmod -R 755 /var/www/html/wcc
```

### Method B: Rsync / SCP from Local Machine B
If pushing static files directly from this local workstation:
```bash
# Example deployment via rsync (adjust remote host and user):
rsync -avz --exclude '.git' --exclude '*.md' \
  ./ user@remote-server:/var/www/html/wcc/
```

### Method C: Apache `.htaccess` Routing Check
Ensure `/var/www/html/.htaccess` allows clean redirection without interfering with root WordPress:
```apache
# Forward root to /wcc/
RewriteEngine On
RewriteRule ^$ /wcc/ [R=301,L]
```

---

## 4. Post-Deployment Verification Checklist

After deploying to production:
- [ ] **HTTP 200 OK**: Access `https://actschurchconference.com/wcc` and confirm HTTP status 200.
- [ ] **Asset Loading**: Open Developer Tools Network tab; verify 0 broken images (404s).
- [ ] **Luma Registration**: Click the registration CTA and confirm redirection to Luma.
- [ ] **Root Redirection**: Visit `https://actschurchconference.com/` and confirm 301 redirect to `/wcc/`.
- [ ] **WordPress Non-Interference**: Access `/wp-admin` or `/wp-login.php` on `actschurchconference.com` to confirm WordPress functionality is intact.
