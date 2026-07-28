# Overleaf Skill

[![npm version](https://img.shields.io/npm/v/@aloth/olcli.svg)](https://www.npmjs.com/package/@aloth/olcli)
[![Homebrew](https://img.shields.io/badge/homebrew-aloth%2Ftap-orange)](https://github.com/aloth/homebrew-tap)
[![AgentSkills](https://img.shields.io/badge/AgentSkills-compatible-blue)](https://agentskills.io)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

An [Agent Skill](https://agentskills.io) for managing Overleaf LaTeX projects.

<p align="center">
  <img src="https://raw.githubusercontent.com/aloth/olcli/main/screenshots/demo.gif" alt="olcli demo" width="600">
</p>

## What it does

- Pull Overleaf projects to work locally
- Push local changes back to Overleaf
- Compile PDFs and download them
- Download `.bbl` files for arXiv submissions
- Bidirectional sync with conflict detection
- Upload figures and assets to projects

## Installation

### For agents (via skills CLI)

```bash
npx skills add aloth/overleaf-skill
```

### CLI tool only

**Homebrew (macOS/Linux):**
```bash
brew tap aloth/tap && brew install olcli
```

**npm (all platforms):**
```bash
npm install -g @aloth/olcli
```

## Quick start

```bash
olcli auth --cookie "SESSION_COOKIE"  # One-time setup
olcli list                             # See all projects
olcli pull "My Paper"                  # Download project
cd My_Paper && vim main.tex            # Edit locally
olcli sync                             # Push changes
olcli pdf --timeout 120000             # Download PDF (large papers need higher timeout)
olcli output bbl                       # Get .bbl for arXiv
```

> 💡 **Timeouts:** Commands like `pull`, `pdf`, `compile`, and `output` default to a 10-second timeout. For large projects, increase it with `--timeout 120000` (2 minutes) or higher.

## arXiv Submission Workflow

Complete workflow for submitting to arXiv:

```bash
# 1. Pull and compile your paper
olcli pull "My Paper"
cd My_Paper
olcli pdf

# 2. Download .bbl (required by arXiv instead of .bib)
olcli output bbl -o main.bbl

# 3. Package for arXiv: .tex files + .bbl + figures
zip arxiv-submission.zip *.tex main.bbl figures/*

# 4. Upload to arxiv.org
```

## Links

- [olcli on GitHub](https://github.com/aloth/olcli)
- [olcli on npm](https://www.npmjs.com/package/@aloth/olcli)
- [Homebrew tap](https://github.com/aloth/homebrew-tap)
- [Agent Skills specification](https://agentskills.io/specification)

## License

MIT

## Troubleshooting

### TLS / proxy issues

If you see `Client network socket disconnected before secure TLS connection was established`, it's likely because you're behind a VPN or proxy app (Shadowrocket, Clash, Surge, etc.). Node.js does **not** automatically use system proxy settings. Add `overleaf.com` to your proxy's **DIRECT** routing rule.

Verify connectivity first:
```bash
curl -sI https://www.overleaf.com/project
```

### Homebrew install fails

If `brew install olcli` fails with npm cache errors, install via npm instead:
```bash
npm install -g @aloth/olcli
```
