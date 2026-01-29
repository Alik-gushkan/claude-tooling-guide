# Claude Tooling Guide

CLI tool recommendations for Claude Code workflows. Ready-to-use configuration with rationale.

## Quick Start

### 1. Install Tools

```bash
brew install fd fzf bat htop tree yq ffmpeg wget
```

### 2. Enable fzf Shell Integration

Add to `~/.zshrc`:
```bash
eval "$(fzf --zsh)"
```

### 3. Configure Claude Code

Create `~/.claude/CLAUDE.md`:

```markdown
## CLI Tools
Installed: YYYY-MM-DD | Upgrade: `brew upgrade fd fzf bat htop tree yq ffmpeg wget`

- Use `fd` instead of `find` for file searches
- Use `bat` instead of `cat` when showing code to user
- Use `tree` for directory structure visualization
- Use `ffmpeg` directly for media tasks
- Use `yq` for YAML manipulation
- Use `htop` for process monitoring

Note: Generated scripts should use standard tools (find, cat, grep) for portability.
```

---

## Design Decisions

### 1. Documentation Location

**Global `~/.claude/CLAUDE.md`**

A global config ensures consistency across all projects. Tool preferences like "use fd instead of find" are universal—they don't change project to project.

### 2. Enforcement Level

**Soft guidance (no hard denies)**

Soft guidance provides benefits without friction. There are edge cases where standard tools are appropriate (generated scripts, portability). Hard denies would require constant exception handling.

### 3. fzf Shell Integration

**Yes, with keybindings**

Ctrl+R (fuzzy history), Ctrl+T (file finder), Alt+C (directory jump) dramatically improve terminal productivity.

### 4. Documentation Style

**Directive-based**

Claude doesn't need explanations of why fd is faster—it needs clear instructions on what to use. Explanatory text wastes context window space.

### 5. Generated Code

**Use standard tools for portability**

A bash script using fd won't work on systems without fd installed. Claude's own commands benefit from modern tools, but generated artifacts should be portable.

### 6. Maintenance Tracking

**Minimal timestamp + upgrade command**

One line with install date and the upgrade command. Version tracking is overkill—brew handles that.

### 7. CLAUDE.md Position

**Top (high priority)**

Tool preferences should be established before any file operations occur. Putting them at the top ensures they're seen before the first `find` command.

---

## Examples

- **[tool-comparisons/](examples/tool-comparisons/)** — Performance and feature comparisons
  - [fd-vs-find.md](examples/tool-comparisons/fd-vs-find.md)
  - [bat-vs-cat.md](examples/tool-comparisons/bat-vs-cat.md)

- **[workflows/](examples/workflows/)** — Real-world usage patterns
  - [find-php-errors.md](examples/workflows/find-php-errors.md)

- **[edge-cases/](examples/edge-cases/)** — When standard tools are appropriate
  - [when-to-use-standard-tools.md](examples/edge-cases/when-to-use-standard-tools.md)

## License

MIT
