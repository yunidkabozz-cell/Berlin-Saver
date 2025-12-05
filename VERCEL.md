# Berlin Savers - Vercel Deployment

To deploy this site to Vercel:

## Quick Start

1. **Push to GitHub** (if not already):
   ```bash
   git add .
   git commit -m "Add Vercel configuration"
   git push origin main
   ```

2. **Import to Vercel**:
   - Go to https://vercel.com/import
   - Connect your GitHub repository
   - Select "Berlin-Saver"
   - Click "Deploy" (no additional configuration needed)

3. **Your site is live** at a Vercel URL (e.g., `berlin-savers.vercel.app`)

## Custom Domain

To use a custom domain:

1. Go to your Vercel project settings
2. Navigate to "Domains"
3. Add your domain
4. Follow DNS configuration instructions

## Environment

- **Framework**: Static HTML (no build step required)
- **Entry Point**: `index.html`
- **Routing**: Hash-based routing (client-side only)

## How It Works

The `vercel.json` file configures:
- Static file serving via `@vercel/static`
- All routes redirect to `index.html` (SPA support)
- Version 2 (Vercel's latest configuration format)

No build process is needed—your site is served as-is.

## File Structure

```
berlin-savers/
├── index.html          (Your main app)
├── vercel.json         (Deployment config)
├── .github/            (GitHub workflows & instructions)
│   └── copilot-instructions.md
└── .gitignore          (Git configuration)
```

## Deployment Logs & Debugging

After deployment, check Vercel's dashboard for:
- Build logs
- Deployment status
- Edge Functions (if added in future)
- Analytics

## Local Testing (Optional)

Install Vercel CLI and test locally:
```bash
npm install -g vercel
vercel dev
```

This runs your site on `localhost:3000` with Vercel's production environment simulation.
