# Overleaf API Reference

## Authentication

olcli uses session cookie authentication via `overleaf_session2`.

### Getting the cookie

1. Open browser DevTools (F12)
2. Go to Application → Cookies → www.overleaf.com
3. Copy the value of `overleaf_session2`

The cookie is a URL-encoded signed session, starting with `s%3A`.

### Cookie storage

Credentials are checked in order:
1. `OVERLEAF_SESSION` environment variable
2. `.olauth` file in current directory
3. Global config: `~/.config/olcli-nodejs/config.json`

## Project sync metadata

When you `pull` a project, olcli creates `.olcli.json`:

```json
{
  "projectId": "64a1b2c3d4e5f6789abcdef0",
  "projectName": "My Paper",
  "lastPull": 1706868000000
}
```

This enables auto-detection for subsequent commands.

## Compile outputs

The `output` command downloads files from the compile endpoint:

| Type | Description |
|------|-------------|
| `bbl` | Bibliography output (for arXiv) |
| `log` | LaTeX compilation log |
| `aux` | Auxiliary file |
| `blg` | BibTeX log |
| `pdf` | Compiled PDF |
| `out` | Hyperref output |

## Rate limiting

Overleaf has rate limits. olcli spaces requests automatically, but if you hit 429 errors, wait a few seconds.
