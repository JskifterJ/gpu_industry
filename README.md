# GPU Industry Dashboard Suite

This repository now uses a central dashboard entrypoint at `index.html`.

## Online Hosting (GitHub Pages)

A workflow is included at `.github/workflows/deploy-pages.yml` to deploy this repo to GitHub Pages.

### One-time setup

1. Push this repository to GitHub (`main` branch).
2. In GitHub, open **Settings -> Pages**.
3. Under **Build and deployment**, set **Source** to **GitHub Actions**.
4. Trigger the workflow (push to `main` or run it manually from the **Actions** tab).

After deployment, your site URL will be:

`https://JskifterJ.github.io/gpu_industry/`

The dashboard hub will be available at:

`https://JskifterJ.github.io/gpu_industry/`

## Privacy and sharing behavior

GitHub Pages for a public repo is effectively **unlisted/public-by-link**:

- Anyone with the link can access it.
- There is no built-in way to prevent recipients from re-sharing the link.

If you need access control (true private sharing), use one of these:

1. Cloudflare Access in front of a Pages site.
2. An authenticated static host (for example, Azure Static Web Apps with Entra auth).
3. A private internal web server/VPN.

## Canonical file structure

### Learning track
- `learning_prompt_to_answer.html`
- `learning_inference_compute_master.html`
- `learning_cluster_architecture.html`
- `learning_stack_inference_matrix.html`

### Strategy track
- `strategy_value_chain_master.html`
- `strategy_competitor_intelligence.html`
- `strategy_inference_economics.html`
- `strategy_abstraction_taxonomy.html`

### Entry point
- `index.html`
