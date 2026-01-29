# Claude Tooling Guide

CLI tool recommendations for Claude Code. Copy-paste ready.

## Quick Start

```bash
brew install fd fzf bat tree yq jq ffmpeg
```

Add to `~/.zshrc`:
```bash
eval "$(fzf --zsh)"
```

## CLAUDE.md Template

Create `~/.claude/CLAUDE.md`:

```markdown
## CLI Tools
Prefer for Bash commands:
- fd over find (faster, .gitignore)
- bat over cat (for user display)
- jq for JSON, yq for YAML
- tree for directories
- ffmpeg for media

Scripts: use standard tools for portability.
```

## Tool Reference

| Tool | Replaces | Advantage |
|------|----------|-----------|
| fd | find | 5x faster, ignores .gitignore |
| bat | cat | Syntax highlighting (for humans) |
| jq | — | JSON processing |
| yq | — | YAML processing |
| tree | ls -R | Directory visualization |
| fzf | — | Fuzzy selection (Ctrl+R history) |

<details>
<summary>Examples</summary>

**fd vs find**
```bash
fd -e php           # Find PHP files
fd "controller"     # Find files matching pattern
```

**jq**
```bash
jq '.name' package.json
jq '.dependencies | keys' package.json
```

**yq**
```bash
yq '.services' docker-compose.yml
yq -i '.version = "2"' config.yml
```

</details>

## When Standard Tools

Scripts for other systems should use portable commands:
- `find` instead of `fd`
- `cat` instead of `bat`
- `grep` instead of `rg`

## License

MIT
