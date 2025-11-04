# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a Hugo-based static site generator project that builds a Chinese mathematics notes website. The site uses the [hugo-coder theme](https://github.com/luizdepra/hugo-coder) and is deployed to GitHub Pages.

**Site URL**: https://math.nyan.work/

## Architecture & Structure

### Content Management

- **Primary Content Source**: All math notes are stored in a separate Git repository as a Git submodule at `content/notes/` (points to `https://github.com/LoweNiyan/myMathNotes.git`)
- **Notes Structure**: The notes are organized in Chinese with folder names like `§1 函数与极限` and `知识点`
- **Homepage**: Custom layout in `layouts/index.html` displays the README.md from the notes submodule on the homepage

### Theme & Configuration

- **Theme**: Hugo-coder theme (git submodule at `themes/hugo-coder/`)
- **Configuration**: `hugo.toml` contains site configuration
  - Language: Chinese (zh-cn)
  - Default theme: auto (automatic dark/light mode switching)
  - Math rendering: KaTeX enabled (`math = true`)
  - PWA: Enabled (`enablePWA = true`)
  - Pagination: 20 items per page

### Key Directories

- `content/notes/`: Git submodule containing all math notes (do not edit directly in this repo)
- `layouts/`: Custom Hugo layouts (currently only overrides the homepage)
- `themes/hugo-coder/`: Theme submodule
- `public/`: Generated site output (auto-generated, not committed)

## Common Commands

### Local Development

```bash
# Start local development server with live reload
hugo server -D

# Build the site for production
hugo --minify

# Update submodules (notes and theme)
git submodule update --remote
```

### Deployment

**Automatic Deployment**: Changes pushed to `main` branch trigger GitHub Actions (`.github/workflows/deploy.yml`) which:
1. Checks out the repository with recursive submodules
2. Builds the site using Hugo 0.152.2 (extended version)
3. Deploys to GitHub Pages

**Manual Update Workflow**: Use the PowerShell script to sync notes from your local notes repository:

```powershell
.\update_all_repos.ps1
```

This script:
1. Commits and pushes changes in your local notes repository
2. Updates the notes submodule in this website repository
3. Commits and pushes the submodule update, triggering automatic deployment

### Submodule Management

```bash
# Pull latest changes from the notes repository
git submodule update --remote content/notes

# Initialize submodules (after cloning)
git submodule update --init --recursive

# View submodule status
git submodule status
```

## Git Workflow

The project uses two repositories:
1. **Notes Repository** (`myMathNotes`): Where actual mathematical content is written
2. **Website Repository** (this repo): Hugo site that displays the notes via submodule

Update process:
1. Make changes to notes in the separate myMathNotes repository
2. Run `update_all_repos.ps1` to sync changes to this repo
3. GitHub Actions automatically deploys to https://math.nyan.work/

## Development Notes

- **Hugo Version**: GitHub Actions uses Hugo 0.152.2 extended
- **No Build Dependencies**: This is a pure Hugo site with no npm/node dependencies
- **No Linting/Testing**: No automated linting or test suites are configured
- **Math Support**: KaTeX is enabled for rendering mathematical formulas
- **Dark Mode**: Theme is set to "auto" which switches based on system preference

## Important Files

- `hugo.toml`: Site configuration
- `.gitmodules`: Defines submodules (notes repository and hugo-coder theme)
- `layouts/index.html`: Custom homepage that displays notes README
- `.github/workflows/deploy.yml`: GitHub Pages deployment workflow
- `update_all_repos.ps1`: PowerShell script for syncing notes repository
