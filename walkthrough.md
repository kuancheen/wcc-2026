# World Changers Conference 2026 (WCC-2026) - Project Living Memory & Walkthrough

## 1. Plan & Objective
The objective of this project is to migrate the World Changers Conference 2026 website from `https://worldchangersconference2026.carrd.co/` to the self-hosted Apache infrastructure at `https://actschurchconference.com/wcc`.

### Key Requirements:
1. **Zero-Cost Migration**: Eliminate Carrd Pro subscription dependency by scraping and reconstituting the full static build locally.
2. **Coexistence with WordPress**: Host the conference landing page under the `/wcc/` sub-path without breaking or interfering with the root WordPress setup on the same Apache instance.
3. **Domain Redirect**: Set up a clean rewrite rule to forward root domain visits (`actschurchconference.com/`) to `/wcc/`.
4. **Third-Party Integrations**: Preserve the Luma event registration URL (`https://luma.com/o6mqew3f`) and restore direct email links (`ywa@actschurch.org`).
5. **Brand Polish**: Remove the Carrd footer badge ("Made with Carrd") and optimize Open Graph preview cards.
6. **Version Control**: Manage all static code and assets via GitHub (`kuancheen/wcc-2026`).

---

## 2. System Architecture & Working Environment

### Environment:
- **Local Machine**: macOS (workspace: `/Users/kuancheen/Documents/bash`)
- **Version Control**: Git + GitHub CLI (`gh`), authenticated to `kuancheen`
- **GitHub Repository**: [https://github.com/kuancheen/wcc-2026](https://github.com/kuancheen/wcc-2026) (Branch: `main`)
- **Production Server Target**: Apache 2.4 on Ubuntu/Debian hosting WordPress at `/var/www/html`
- **Sub-directory Target**: `/var/www/html/wcc/`

---

## 3. Active File Structure

### Local Workspace:
```
/Users/kuancheen/Documents/bash/
├── wcc/                                # Git repository tracking the website
│   ├── .git/
│   ├── index.html                      # Main static landing page (cleaned & patched)
│   ├── assets/
│   │   ├── images/
│   │   │   ├── card.jpg                # Social sharing Open Graph preview image
│   │   │   ├── container02.jpg         # Hero/Section background texture
│   │   │   ├── container08.jpg         # Footer/Section background texture
│   │   │   ├── image01.png             # Conference branding logo
│   │   │   ├── image02.png             # Speaker / Partner icon 1
│   │   │   ├── image03.png             # Speaker / Partner icon 2
│   │   │   ├── image04.png             # Speaker / Partner icon 3
│   │   │   ├── image05.png             # Speaker / Partner icon 4
│   │   │   └── gallery04/              # 22 High-res conference photo gallery images & originals
│   │   │       ├── 0a813a9f.jpg, 0a813a9f_original.jpg ...
│   │   │       └── (22 images total)
│   │   └── external/
│   │       ├── fonts.googleapis.com/   # Google Fonts stylesheet cache
│   │       └── i.ytimg.com/            # YouTube video teaser high-res thumbnail
│   └── cdn-cgi/
│       └── scripts/.../email-decode.min.js
├── wcc_site.zip                        # 12MB standalone zip archive for easy SFTP upload
├── implementation_plan.md              # Living memory implementation checklist
└── walkthrough.md                      # Living memory architecture & setup document
```

---

## 4. Completed & Fully Working Features

| Component / Feature | Status | Implementation Details |
| :--- | :---: | :--- |
| **Complete Asset Extraction** | `COMPLETED` | Downloaded all 35+ assets including 22 gallery photos, container background textures, icons, and logos into self-contained paths. |
| **Path Rewriting** | `COMPLETED` | Converted all hardcoded remote URLs in `index.html` to relative paths (`assets/...`). |
| **Carrd Branding Removal** | `COMPLETED` | Removed `<div id="credits">` and added CSS rules `#credits { display: none !important; }`. |
| **Cloudflare Email De-obfuscation** | `COMPLETED` | Replaced Cloudflare obfuscated hex strings with direct `mailto:ywa@actschurch.org` links. |
| **Luma Registration Form** | `COMPLETED` | Preserved active registration links pointing to `https://luma.com/o6mqew3f`. |
| **Open Graph (Social Sharing)** | `COMPLETED` | Downloaded `card.jpg` and updated metadata tags for social previews. |
| **GitHub Repository Initialized** | `COMPLETED` | Created public repo `kuancheen/wcc-2026` and pushed initial commit to `main`. |
| **Offline Zip Backup** | `COMPLETED` | Created `wcc_site.zip` ready for single-file upload/extraction. |

---

## 5. Lessons Learned & Gotchas

1. **Carrd Asset Scraping**:
   - `wget --mirror` without specific filters missed dynamically loaded relative image paths and single-line minified markup.
   - Using a custom Python parsing script that extracts `src`, `href`, `data-src`, and CSS `url(...)` ensured 100% asset coverage (including full-resolution `_original.jpg` lightbox gallery items).
2. **Cloudflare Email Protection**:
   - Carrd uses Cloudflare's XOR email obfuscation (`/cdn-cgi/l/email-protection#...`). Without Cloudflare's server runtime on standard Apache, emails break into `[email protected]`. Decoding the XOR hex string during migration restored clean `mailto:` links.
3. **Git Large Push Buffer**:
   - High-resolution gallery images (~12MB) caused Git push to fail initially over HTTP (`RPC failed; HTTP 400`). Resolving this required configuring `git config http.postBuffer 524288000` prior to pushing.
4. **Sub-directory vs Root WordPress Integration**:
   - Placing the static site inside `/var/www/html/wcc/` allows Apache to serve static files with maximum performance while letting WordPress continue managing root database routes without conflict.
