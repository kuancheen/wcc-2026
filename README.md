# World Changers Conference 2026 (WCC-2026)

Static landing page and digital asset distribution for the World Changers Conference 2026 (`actschurchconference.com/wcc`), self-hosted on Apache alongside WordPress.

---

## 1. Overview
- **Production URL**: `https://actschurchconference.com/wcc`
- **Root Redirect**: `https://actschurchconference.com/` -> `/wcc/`
- **Source Migration**: Converted from Carrd Pro static export into an independent static site.
- **Key Integrations**:
  - Event registration via Luma (`https://luma.com/o6mqew3f`)
  - Contact links (`mailto:ywa@actschurch.org`)
  - Optimized Open Graph / social media meta preview tags

---

## 2. Directory Structure
```
.
├── assets/
│   ├── images/
│   │   ├── card.jpg             # Open Graph preview card
│   │   ├── container02.jpg      # Section texture background
│   │   ├── container08.jpg      # Footer texture background
│   │   ├── image01.png          # Conference branding / logo
│   │   ├── image02-05.png       # Partner / highlight icons
│   │   └── gallery04/           # High-resolution conference gallery photos
├── cdn-cgi/                     # Static scripts & fallback handlers
├── index.html                   # Core conference landing page
├── implementation_plan.md       # Living task list and pending deployment items
├── walkthrough.md               # Technical overview, architectural history, and notes
├── new_conversation.md          # Agent bootstrap and instructions
└── deployment_checklist.md      # Multi-machine setup and deployment guide
```

---

## 3. Submodule Development Workflow
To work with this repository as part of the `bash` parent repo:

```bash
# Clone parent with submodules initialized:
git clone --recurse-submodules https://github.com/kuancheen/bash.git

# Pull upstream changes for this submodule:
git submodule update --remote wcc
```

For detailed deployment and multi-machine setup instructions, see [`deployment_checklist.md`](./deployment_checklist.md).
