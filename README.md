# Docker Documentation Mirror for GitLab Pages

This repository contains a complete mirror of the official [Docker Documentation](https://docs.docker.com/) website, configured to be served via GitLab Pages.

## Overview

- **Source:** https://docs.docker.com/
- **Mirror Contents:** Complete static mirror of Docker documentation
- **Hosting:** GitLab Pages

## Repository Structure

```
.
├── .gitlab-ci.yml          # GitLab CI/CD configuration for Pages deployment
├── README.md               # This file
└── public/                 # Mirrored Docker documentation (served by GitLab Pages)
    ├── index.html          # Main entry point
    ├── get-started/        # Getting started guides
    ├── guides/             # Docker guides and tutorials
    ├── manuals/            # Docker manuals
    ├── reference/          # API and CLI reference
    ├── desktop/            # Docker Desktop documentation
    ├── engine/             # Docker Engine documentation
    ├── compose/            # Docker Compose documentation
    ├── build/              # Docker Build documentation
    ├── docker-hub/         # Docker Hub documentation
    └── ...                 # Additional documentation sections
```

## Deployment

### GitLab Pages Setup

1. **Push to GitLab:** Push this repository to a GitLab repository
2. **Enable GitLab Pages:** The `.gitlab-ci.yml` file is already configured
3. **Access:** Once the CI/CD pipeline completes, your documentation will be available at:
   - `https://<username>.gitlab.io/<repository-name>/`
   - Or `https://<group>.gitlab.io/<repository-name>/`

### GitLab CI/CD Configuration

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

To enable automatic periodic updates, uncomment the `update_mirror` job in `.gitlab-ci.yml` and set up a GitLab CI/CD schedule:

1. Go to your GitLab project
2. Navigate to CI/CD → Schedules
3. Create a new schedule (e.g., weekly)
4. The `update_mirror` job will run and update the documentation

## Custom Domain (Optional)

To serve the documentation from a custom domain:

1. Go to your GitLab project → Settings → Pages
2. Add your custom domain
3. Configure DNS settings as instructed
4. (Optional) Enable SSL/TLS

## License & Attribution

- The Docker documentation is copyright Docker, Inc.
- This mirror is for personal/internal use
- Original documentation: https://docs.docker.com/
- Docker trademark and brand guidelines apply

## Troubleshooting

### Pages not loading
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
3. Submit a merge request

---

**Note:** This is an unofficial mirror of Docker documentation for offline access and GitLab Pages hosting. For the official and most up-to-date documentation, always refer to https://docs.docker.com/
