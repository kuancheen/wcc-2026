# Project Implementation Plan: WCC 2026 Site Migration

## Project Goal
Migrate the World Changers Conference 2026 website (originally hosted on `https://worldchangersconference2026.carrd.co/`) to the self-hosted Apache web server at `https://actschurchconference.com/wcc` alongside the existing WordPress installation, configuring root domain redirects and preserving all functionality.

---

## Remaining Tasks & Architectural Steps

### Phase 1: Server Deployment & Apache Setup
- [ ] **1.1 SFTP / Git Clone to Server**: Clone `https://github.com/kuancheen/wcc-2026.git` into `/var/www/html/wcc` on the live Apache server (or extract `wcc_site.zip`).
- [ ] **1.2 Directory & File Permissions**: Set ownership to `www-data:www-data` and permissions to `755` for `/var/www/html/wcc`.
- [ ] **1.3 Web Server Compatibility Check**: Ensure Apache's `mod_rewrite` is enabled and that `.htaccess` overrides are allowed (`AllowOverride All` in Apache vhost).

### Phase 2: Domain Routing & Redirection
- [ ] **2.1 Configure Root Redirect**: Add 301 redirect in root `.htaccess` (`/var/www/html/.htaccess`) to forward `https://actschurchconference.com/` to `https://actschurchconference.com/wcc/` without disrupting existing WordPress routes (e.g. `/wp-admin`, `/wp-content`, blog posts).
- [ ] **2.2 Sub-path Routing Verification**: Verify that visiting `/wcc` automatically loads `/wcc/index.html` with correct relative path resolution.

### Phase 3: Functionality & Regression Testing
- [ ] **3.1 Luma Registration Integration**: Verify that clicking the "Register Now" buttons successfully opens the Luma registration flow (`https://luma.com/o6mqew3f`).
- [ ] **3.2 Contact & Mailto Actions**: Test the "Contact Us" email buttons (`mailto:ywa@actschurch.org`) on desktop and mobile clients.
- [ ] **3.3 Media & Asset Integrity**: Confirm all 22 gallery images, logos, background containers, and the YouTube video teaser thumbnail load over HTTPS with 200 OK status.
- [ ] **3.4 Social Media Sharing (OG Tags)**: Validate Open Graph preview cards (`og:image`, `og:title`, `og:description`) on platforms (WhatsApp, Facebook, LinkedIn, iMessage).
- [ ] **3.5 Mobile Responsiveness**: Check viewport responsiveness across iOS Safari, Android Chrome, and desktop browsers.

### Phase 4: Maintenance & Post-Migration
- [ ] **4.1 Carrd Decommissioning / Forwarding**: Update Carrd DNS / custom domain settings (if applicable) or set a redirect banner pointing visitors from Carrd to `actschurchconference.com/wcc`.
- [ ] **4.2 Git Deployment Workflow**: Document standard procedure for pushing updates to `kuancheen/wcc-2026` and pulling changes to `/var/www/html/wcc` on the server.
