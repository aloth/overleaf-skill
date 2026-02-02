# Overleaf Skill

An [Agent Skill](https://agentskills.io) for managing Overleaf LaTeX projects.

## What it does

- Pull Overleaf projects to work locally
- Push local changes back to Overleaf
- Compile PDFs and download them
- Download `.bbl` files for arXiv submissions
- Bidirectional sync with conflict detection

## Installation

### For agents (via skills CLI)

```bash
npx skills add aloth/overleaf-skill
```

### CLI tool only

```bash
brew tap aloth/tap && brew install olcli
```

## Quick start

```bash
olcli auth --cookie "SESSION_COOKIE"  # One-time setup
olcli pull "My Paper"                  # Download project
cd My_Paper && vim main.tex            # Edit locally
olcli sync                             # Push changes
olcli pdf                              # Download PDF
olcli output bbl                       # Get .bbl for arXiv
```

## Links

- [olcli on GitHub](https://github.com/aloth/olcli)
- [olcli on npm](https://www.npmjs.com/package/@aloth/olcli)
- [Agent Skills specification](https://agentskills.io/specification)

## License

MIT
