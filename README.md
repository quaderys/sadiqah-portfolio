# Sadiqah Quadery — Personal Portfolio

Built with [Astro 6](https://astro.build/) and deployed via GitHub Pages.

---

## Running Locally

**Prerequisites:** Node.js 22+ (install at [nodejs.org](https://nodejs.org))

```sh
# 1. Install dependencies
npm install

# 2. Start the development server
npm run dev
```

Open [http://localhost:4321/sadiqah-portfolio](http://localhost:4321/sadiqah-portfolio) in your browser.
The site reloads automatically when you save changes.

---

## Adding a New Project

Projects live as JSON files in `src/content/projects/`. To add a new one:

1. Create a new file, e.g. `src/content/projects/my-project.json`
2. Fill it in with this format:

```json
{
  "title": "Project Title",
  "description": "A sentence or two describing what you built and why it matters.",
  "date": "2025-09",
  "tags": ["Tag One", "Tag Two"],
  "link": "https://github.com/yourusername/my-project"
}
```

- `date` — year and month in `YYYY-MM` format (used for sorting)
- `tags` — optional array of labels
- `link` — optional URL shown as "View Project →"

Save the file and the project card appears automatically on the site.

---

## Deploying to GitHub Pages (Step-by-Step)

This is a complete guide — no prior GitHub experience required.

### Step 1 — Create a GitHub Account

Go to [github.com](https://github.com) and sign up for a free account if you don't have one.

### Step 2 — Create a New Repository

1. Click the **+** icon in the top-right corner of GitHub, then **New repository**.
2. Name it exactly: `sadiqah-portfolio`
3. Leave it **Public** (required for free GitHub Pages).
4. Do **not** check "Add a README" (you already have one).
5. Click **Create repository**.

### Step 3 — Connect Your Local Project to GitHub

Open a terminal in the project folder and run these commands one at a time.
Replace `YOUR_USERNAME` with your actual GitHub username.

```sh
# Initialize git (tracks your changes)
git init

# Stage all files for your first commit
git add .

# Save your first commit
git commit -m "Initial commit"

# Tell git which branch to use (GitHub's default is 'main')
git branch -M main

# Connect to your GitHub repo
git remote add origin https://github.com/YOUR_USERNAME/sadiqah-portfolio.git

# Push your code to GitHub
git push -u origin main
```

If GitHub asks for your username and password during push, use your GitHub username
and a **Personal Access Token** (not your password). To create one:
1. Go to GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Generate a new token with the **repo** scope.
3. Copy it and paste it when prompted for a password.

### Step 4 — Update Your Site URL

Open `astro.config.mjs` and update the `site` field with your real GitHub username:

```js
export default defineConfig({
  site: 'https://YOUR_USERNAME.github.io',
  base: '/sadiqah-portfolio',
});
```

Save the file, then commit and push:

```sh
git add astro.config.mjs
git commit -m "Update site URL"
git push
```

### Step 5 — Enable GitHub Pages with GitHub Actions

1. On your repository page, click the **Settings** tab.
2. In the left sidebar, click **Pages**.
3. Under **Source**, select **GitHub Actions**.
4. Back in your project, create the file `.github/workflows/deploy.yml` with this content:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 22
          cache: npm
      - run: npm install
      - run: npm run build
      - uses: actions/upload-pages-artifact@v3
        with:
          path: dist/

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - id: deployment
        uses: actions/deploy-pages@v4
```

5. Commit and push this file:

```sh
git add .github/workflows/deploy.yml
git commit -m "Add GitHub Pages deployment workflow"
git push
```

### Step 6 — Watch it Deploy

1. Go to your repository on GitHub.
2. Click the **Actions** tab.
3. You'll see a workflow run in progress. It takes about 1–2 minutes.
4. Once it shows a green checkmark, your site is live at:
   `https://YOUR_USERNAME.github.io/sadiqah-portfolio`

---

## Updating the Site

Every time you push to `main`, GitHub Actions automatically rebuilds and redeploys the site.

```sh
# After making changes:
git add .
git commit -m "Describe your change"
git push
```

---

## Project Structure

```
sadiqah-portfolio/
├── public/               # Static files (favicon, images)
├── src/
│   ├── components/       # Page sections (Nav, Hero, About, etc.)
│   ├── content/
│   │   └── projects/     # Add new projects here as .json files
│   ├── layouts/
│   │   └── Layout.astro  # Shared HTML shell & global styles
│   ├── pages/
│   │   └── index.astro   # Main page — assembles all sections
│   ├── styles/
│   │   └── sections.css  # Shared section layout styles
│   └── content.config.ts # Defines the projects collection schema
├── astro.config.mjs      # Astro configuration (site URL, base path)
└── package.json
```

## Commands

| Command           | Action                                      |
| :---------------- | :------------------------------------------ |
| `npm install`     | Install dependencies                        |
| `npm run dev`     | Start dev server at `localhost:4321`        |
| `npm run build`   | Build production site to `./dist/`          |
| `npm run preview` | Preview the production build locally        |
