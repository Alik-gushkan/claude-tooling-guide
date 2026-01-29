# Claude Code Tooling Guide

Configure Claude Code to use optimal tools for different contexts.

## When to Use What

| Context | Tools | Why |
|---------|-------|-----|
| Exploration | Built-in (Glob, Grep, Read) | Fastest for AI |
| Scripts, automation, pipelines | CLI (fd, jq, yq) | Better tooling |
| User asks for CLI | CLI | Explicit request |

## Quick Start

```bash
brew install fd fzf bat tree yq jq ffmpeg
```

## CLAUDE.md Template

Create `~/.claude/CLAUDE.md`:

```markdown
## CLI Tools
Use Bash with these tools when:
- Building scripts or automation
- Complex pipelines (multiple commands)
- User asks for CLI solution

Default to built-in tools (Glob, Grep, Read) for exploration.

Available: fd (find), bat (cat), jq, yq, tree, ffmpeg
```

## Tool Reference

| Tool | Replaces | Advantage |
|------|----------|-----------|
| fd | find | 5x faster, respects .gitignore |
| bat | cat | Syntax highlighting |
| jq | — | JSON processing |
| yq | — | YAML processing |
| fzf | — | Fuzzy selection (Ctrl+R) |

<details>
<summary>Examples</summary>

```bash
fd -e php                         # Find PHP files
jq '.dependencies' package.json   # Extract JSON field
yq '.services' docker-compose.yml # Extract YAML field
```

</details>

## License

MIT
