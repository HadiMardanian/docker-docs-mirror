# Docker Documentation Mirror for GitHub & GitLab Pages

This repository contains a complete mirror of the official [Docker Documentation](https://docs.docker.com/) website, configured to be served via **GitHub Pages** or **GitLab Pages**.

## Overview

- **Source:** https://docs.docker.com/
- **Mirror Contents:** Complete static mirror of Docker documentation
- **Hosting Options:** GitHub Pages / GitLab Pages

## Repository Structure

```
.
├── .github/
│   └── workflows/
│       └── deploy-pages.yml    # GitHub Actions workflow for GitHub Pages
├── .gitlab-ci.yml              # GitLab CI/CD configuration for GitLab Pages
├── README.md                   # This file
└── public/                     # Mirrored Docker documentation
    ├── index.html              # Main entry point
    ├── get-started/            # Getting started guides
    ├── guides/                 # Docker guides and tutorials
    ├── manuals/                # Docker manuals
    ├── reference/              # API and CLI reference
    ├── desktop/                # Docker Desktop documentation
    ├── engine/                 # Docker Engine documentation
    ├── compose/                # Docker Compose documentation
    ├── build/                  # Docker Build documentation
    ├── docker-hub/             # Docker Hub documentation
    └── ...                     # Additional documentation sections
```

## Deployment

### Option 1: GitHub Pages Setup

1. **Push to GitHub:** Push this repository to a GitHub repository
2. **Enable GitHub Pages:**
   - Go to repository **Settings** → **Pages**
   - Under "Build and deployment", select **GitHub Actions** as the source
3. **Deploy:** The workflow runs automatically on push to `main` or `master` branch
4. **Access:** Once deployed, your documentation will be available at:
   - `https://<username>.github.io/<repository-name>/`

#### GitHub Actions Workflow

The included `.github/workflows/deploy-pages.yml` file:
- Automatically deploys the `public/` directory to GitHub Pages
- Triggers on push to `main` or `master` branch
- Can also be triggered manually via the Actions tab

### Option 2: GitLab Pages Setup

1. **Push to GitLab:** Push this repository to a GitLab repository
2. **Enable GitLab Pages:** The `.gitlab-ci.yml` file is already configured
3. **Access:** Once the CI/CD pipeline completes, your documentation will be available at:
   - `https://<username>.gitlab.io/<repository-name>/`
   - Or `https://<group>.gitlab.io/<repository-name>/`

#### GitLab CI/CD Configuration

The included `.gitlab-ci.yml` file:
- Deploys the `public/` directory to GitLab Pages
- Runs automatically on pushes to the default branch

## Updating the Mirror

To update the mirror with the latest Docker documentation, run:

```bash
cd public
wget \
  --mirror \
  --convert-links \
  --adjust-extension \
  --page-requisites \
  --no-parent \
  --no-host-directories \
  --directory-prefix=. \
  -e robots=off \
  --wait=0.5 \
  --random-wait \
  --limit-rate=5M \
  --user-agent="Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36" \
  --reject="*.exe,*.msi,*.dmg,*.zip,*.tar.gz,*.deb,*.rpm" \
  --timeout=30 \
  --tries=3 \
  --continue \
  "https://docs.docker.com/"
```

### wget Options Explained

| Option | Description |
|--------|-------------|
| `--mirror` | Enable mirroring options (recursive, timestamping, infinite depth) |
| `--convert-links` | Convert links to work locally offline |
| `--adjust-extension` | Add `.html` extension to files |
| `--page-requisites` | Download all files needed to display pages (CSS, images, etc.) |
| `--no-parent` | Don't follow links outside the initial URL |
| `--no-host-directories` | Don't create directories for hostname |
| `-e robots=off` | Ignore robots.txt restrictions |
| `--wait=0.5` | Wait 0.5 seconds between requests |
| `--random-wait` | Randomize wait time (0.5x to 1.5x) |
| `--limit-rate=5M` | Limit download speed to 5MB/s |
| `--continue` | Resume partially downloaded files |

## Mirror Statistics

- **Total Files:** ~1,550+
- **Total Size:** ~460 MB
- **Sections Included:**
  - Getting Started
  - Guides (Language-specific, Use cases)
  - Docker Desktop (Windows, Mac, Linux)
  - Docker Engine
  - Docker Compose
  - Docker Build / BuildKit
  - Docker Hub
  - Extensions
  - Security
  - Enterprise features
  - API Reference
  - CLI Reference
  - And more...

## Setting Up Automatic Updates (Optional)

### GitHub Actions (Scheduled)

Add a schedule trigger to `.github/workflows/deploy-pages.yml`:

```yaml
on:
  push:
    branches: ["main", "master"]
  schedule:
    # Run weekly on Sunday at midnight
    - cron: '0 0 * * 0'
  workflow_dispatch:
```

### GitLab CI/CD (Scheduled)

1. Go to your GitLab project
2. Navigate to CI/CD → Schedules
3. Create a new schedule (e.g., weekly)
4. The pipeline will run and update the documentation

## Custom Domain (Optional)

### GitHub Pages Custom Domain

1. Go to repository **Settings** → **Pages**
2. Under "Custom domain", enter your domain
3. Configure DNS:
   - For apex domain: Add `A` records pointing to GitHub's IPs
   - For subdomain: Add `CNAME` record pointing to `<username>.github.io`
4. (Optional) Enable "Enforce HTTPS"

### GitLab Pages Custom Domain

1. Go to your GitLab project → **Settings** → **Pages**
2. Add your custom domain
3. Configure DNS settings as instructed
4. (Optional) Enable SSL/TLS

## License & Attribution

- The Docker documentation is copyright Docker, Inc.
- This mirror is for personal/internal use
- Original documentation: https://docs.docker.com/
- Docker trademark and brand guidelines apply

## Troubleshooting

### GitHub Pages not loading
- Ensure the GitHub Actions workflow completed successfully (check Actions tab)
- Verify GitHub Pages is enabled in repository Settings → Pages
- Check that the source is set to "GitHub Actions"
- Wait a few minutes after deployment for propagation

### GitLab Pages not loading
- Ensure the CI/CD pipeline completed successfully
- Check that the `public/` directory exists and contains `index.html`
- Verify GitLab Pages is enabled in project settings

### Broken links after mirroring
- The `--convert-links` option should handle most cases
- Some dynamic JavaScript-based navigation may not work in the static mirror
- Re-run the wget command to fix any missing files

### Large file sizes
- The mirror excludes binary downloads (exe, dmg, zip, etc.)
- To reduce size further, consider excluding images:
  ```bash
  --reject="*.png,*.jpg,*.gif,*.svg"
  ```

## Contributing

To contribute improvements to this mirror setup:
1. Fork the repository
2. Make your changes
3. Submit a pull request (GitHub) or merge request (GitLab)

---

**Note:** This is an unofficial mirror of Docker documentation for offline access and static hosting. For the official and most up-to-date documentation, always refer to https://docs.docker.com/
