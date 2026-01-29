# When to Use Standard Tools (find, cat, grep)

Modern tools like `fd`, `bat`, and `rg` are excellent for interactive use, but standard POSIX tools remain the right choice in specific situations.

## Rule of Thumb

| Context | Use |
|---------|-----|
| Interactive terminal work | Modern tools (fd, bat, rg) |
| Scripts meant to run elsewhere | Standard tools (find, cat, grep) |
| CI/CD pipelines | Standard tools |
| Docker containers | Standard tools (unless you control the image) |
| Documentation/tutorials | Standard tools (universal audience) |

## Why This Matters

### Scenario: Deployment Script

**Wrong approach:**
```bash
#!/bin/bash
# deploy.sh - runs on production server

# This will FAIL if fd isn't installed
fd -e php -x php -l {}
```

**Correct approach:**
```bash
#!/bin/bash
# deploy.sh - runs on production server

# Works everywhere
find . -name "*.php" -exec php -l {} \;
```

### Scenario: CI Pipeline

**Wrong approach:**
```yaml
# .github/workflows/lint.yml
- name: Check PHP syntax
  run: fd -e php -x php -l {}  # Fails: fd not in ubuntu-latest
```

**Correct approach:**
```yaml
# .github/workflows/lint.yml
- name: Check PHP syntax
  run: find . -name "*.php" -exec php -l {} \;
```

## Specific Situations

### 1. Bash Scripts

If your script will run on other machines:
```bash
# Use standard tools
find . -name "*.log" -mtime +7 -delete
grep -r "TODO" --include="*.py" .
cat config.txt | while read line; do ...; done
```

### 2. Makefiles

Makefiles often run in minimal environments:
```makefile
# Good - portable
lint:
    find src -name "*.js" -exec eslint {} \;

# Risky - requires fd
lint:
    fd -e js -x eslint {}
```

### 3. Docker

Unless your Dockerfile installs the tools:
```dockerfile
# If you don't have this:
RUN apt-get install -y fd-find ripgrep bat

# Then scripts must use:
RUN find /app -name "*.pyc" -delete
```

### 4. Documentation

When writing docs for others:
```markdown
## Finding large files

Use this command to find files over 100MB:

    find . -size +100M -type f

(Works on any Unix system)
```

### 5. Shared Team Scripts

Unless the whole team has the tools:
```bash
# Team script - use portable commands
#!/bin/bash
grep -r "FIXME" --include="*.java" src/
```

## When Modern Tools ARE Appropriate in Scripts

1. **Personal automation** - Scripts only you will run
2. **Project with documented requirements** - Team agrees to install tools
3. **Containerized with tools included** - You control the runtime
4. **Performance critical** - fd's speed matters for the script

Example of appropriate use:
```bash
#!/bin/bash
# personal/cleanup.sh - only runs on my machine

# I have fd installed, and this is much faster
fd -e tmp -x rm {}
```

## The Claude Code Distinction

Claude Code uses modern tools for its own operations (faster, better output) but generates portable code by default:

| Claude Code doing | Uses |
|-------------------|------|
| Finding files for itself | `fd -e php` |
| Generating a script for you | `find . -name "*.php"` |
| Displaying code | `bat file.php` |
| Writing a cat command in a script | `cat file.txt` |

This is why the CLAUDE.md configuration includes:

> Note: Generated scripts should use standard tools (find, cat, grep) for portability.

## Summary

- **Interactive work**: Use fd, bat, rg - they're faster and nicer
- **Scripts/automation**: Use find, cat, grep - they work everywhere
- **When in doubt**: Ask "Where will this code run?"
