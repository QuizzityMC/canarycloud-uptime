# GitHub Pages Deployment Guide

This repository is configured to deploy to GitHub Pages automatically when changes are pushed to the `master` branch.

## Setup Instructions

To enable GitHub Pages deployment for this repository:

1. **Enable GitHub Pages in Repository Settings**
   - Go to your repository settings on GitHub
   - Navigate to **Settings** → **Pages**
   - Under **Source**, select **GitHub Actions**

2. **Configure Custom Domain (Optional)**
   - If you want to use a custom domain (like `git.kuma.pet` as configured in the CNAME file):
     - In the same Pages settings, enter your custom domain in the **Custom domain** field
     - Configure your DNS provider to point to GitHub Pages:
       - For apex domain: Create A records pointing to GitHub's IPs
       - For subdomain: Create a CNAME record pointing to `<username>.github.io`
     - See [GitHub's custom domain documentation](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site) for details

3. **Trigger Deployment**
   - Push changes to the `master` branch, or
   - Manually trigger the workflow from the **Actions** tab

## How It Works

- The GitHub Actions workflow (`.github/workflows/deploy-github-pages.yml`) automatically:
  - Installs dependencies
  - Builds the application using `npm run build`
  - Deploys the `dist` folder to GitHub Pages

- **SPA Routing Support**: The configuration includes:
  - A `404.html` that redirects to `index.html` with the path preserved
  - A script in `index.html` that restores the original URL
  - This allows direct navigation to routes like `/dashboard` to work correctly

## Local Testing

To test the build locally:

```bash
npm install
npm run build
```

The built files will be in the `dist` directory.

## Base Path Configuration

The base path is configurable via the `BASE_PATH` environment variable:
- Default: `/` (for custom domains or root deployment)
- For deployment to `username.github.io/repo-name`, set `BASE_PATH=/repo-name/` in the workflow

## Troubleshooting

- **404 errors**: Ensure GitHub Pages is set to use GitHub Actions as the source
- **Assets not loading**: Check the base path configuration in `config/vite.config.js`
- **Routing issues**: Verify that both `404.html` and the SPA redirect script in `index.html` are present
- **Custom domain not working**: Verify DNS settings and that the CNAME file contains the correct domain
