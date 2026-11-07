---
name: overleaf
description: Sync and manage Overleaf LaTeX projects from the command line. Pull projects locally, push changes back, diff against Overleaf, make a marked-up latexdiff PDF, use Overleaf as a git remote, compile PDFs, and download compile outputs like .bbl files for arXiv submissions. Works on the free Overleaf plan. Use when working with LaTeX, Overleaf, academic papers, or arXiv.
license: MIT
metadata:
  author: aloth
  version: "1.2"
  cli: olcli
  install: brew tap aloth/tap && brew install olcli
---

# Overleaf Skill

Manage Overleaf LaTeX projects via the `olcli` CLI.

## Installation

```bash
# Homebrew (recommended)
brew tap aloth/tap && brew install olcli

# npm
npm install -g @aloth/olcli
```

## Authentication

Sign in once through a browser window (needs olcli 0.16 or newer and Node 22 or newer). Works with SSO and two-factor logins; olcli keeps only the session cookie:

```bash
olcli auth --browser
```

Without a display (a server, a container), paste the session cookie instead: log into [overleaf.com](https://www.overleaf.com), open DevTools (F12), Application, Cookies, and copy the value of `overleaf_session2`:

```bash
olcli auth --cookie "YOUR_SESSION_COOKIE"
```

Check the setup (exits 1 when not signed in, so `olcli auth status || olcli auth --browser` signs in only when needed):
```bash
olcli auth status
```

Clear stored credentials:
```bash
olcli logout
```

## Common Workflows

### Pull a project to work locally

```bash
olcli pull "My Paper"
cd My_Paper/
```

### Edit and sync changes

```bash
# After editing files locally
olcli push              # Upload changes only
olcli sync              # Bidirectional sync (pull + push)
```

### See what a push would change

```bash
olcli diff                     # unified diff against Overleaf as it is right now
olcli diff --name-only         # just the changed paths
olcli diff --latexdiff --pdf   # marked-up PDF of the revision, compiled on Overleaf
```

`olcli push` replaces whole files on Overleaf. Run `olcli diff` first when coauthors edit the same project.

### Use Overleaf as a git remote

```bash
git clone overleaf::https://www.overleaf.com/project/<id> paper
cd paper
git pull                       # bring in coauthors' edits
git commit -am "Rewrite results"
git push                       # refused while Overleaf has edits you have not pulled
```

Needs olcli 0.16.1 or newer. If a push is refused, run `git pull`, resolve any conflict, commit and push again. `git push --force` overwrites Overleaf on purpose.

### Compile and download PDF

```bash
olcli pdf                      # Compile and download
olcli pdf -o paper.pdf         # Custom output name
olcli compile                  # Just compile (no download)
```

### Download .bbl for arXiv submission

```bash
olcli output bbl               # Download compiled .bbl
olcli output bbl -o main.bbl   # Custom filename
olcli output --list            # List all available outputs
```

### Upload figures or assets

```bash
olcli upload figure1.png "My Paper"          # Upload to project root
olcli upload diagram.pdf                      # Auto-detect project from .olcli.json
```

### Download specific files

```bash
olcli download main.tex "My Paper"           # Download single file
olcli zip "My Paper"                          # Download entire project as zip
```

## arXiv Submission Workflow

Complete workflow for preparing an arXiv submission:

```bash
# 1. Pull your project
olcli pull "Research Paper"
cd Research_Paper

# 2. Compile to ensure everything builds
olcli compile

# 3. Download the .bbl file (arXiv requires .bbl, not .bib)
olcli output bbl -o main.bbl

# 4. Download any other needed outputs
olcli output aux -o main.aux    # If needed

# 5. Package for submission
zip arxiv.zip *.tex main.bbl figures/*.pdf

# 6. Verify the package compiles locally (optional)
# Then upload arxiv.zip to arxiv.org
```

## Commands Reference

| Command | Description |
|---------|-------------|
| `olcli auth --browser` | Sign in through a browser window |
| `olcli auth --cookie <value>` | Authenticate with session cookie |
| `olcli auth status` | Check authentication status and credential source |
| `olcli logout` | Clear stored credentials |
| `olcli list` | List all projects |
| `olcli info [project]` | Show project details |
| `olcli pull [project] [dir]` | Download project files |
| `olcli push [dir]` | Upload local changes |
| `olcli sync [dir]` | Bidirectional sync |
| `olcli diff [project] [dir]` | Diff local files against Overleaf (`--latexdiff --pdf` for a marked-up PDF) |
| `olcli upload <file> [project]` | Upload a single file |
| `olcli download <file> [project]` | Download a single file |
| `olcli zip [project]` | Download as zip archive |
| `olcli compile [project]` | Trigger compilation |
| `olcli pdf [project]` | Compile and download PDF |
| `olcli output [type]` | Download compile outputs |

## Tips

- **Auto-detect project**: Run commands from a synced directory (contains `.olcli.json`) to skip the project argument
- **Dry run**: Use `olcli push --dry-run` to preview changes before uploading
- **Force overwrite**: Use `olcli pull --force` to overwrite local changes
- **Project ID**: You can use project ID instead of name (24-char hex from URL)
- **Debug auth**: Run `olcli auth status` to see the account and where credentials are loaded from
- **Before a push**: Run `olcli diff` to see what the push would overwrite
