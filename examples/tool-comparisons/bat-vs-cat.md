# bat vs cat: Feature Comparison

## Summary

`bat` is a `cat` replacement with:
- Syntax highlighting
- Line numbers
- Git integration (shows modified lines)
- Automatic paging
- File concatenation (like cat)

## Feature Comparison

| Feature | bat | cat |
|---------|-----|-----|
| Display file contents | Yes | Yes |
| Syntax highlighting | Yes (300+ languages) | No |
| Line numbers | Yes (default) | No (use -n flag) |
| Git diff markers | Yes | No |
| Automatic paging | Yes (for long files) | No |
| Concatenate files | Yes | Yes |
| POSIX compliant | No | Yes |
| Available everywhere | No (needs install) | Yes |

## Visual Comparison

### cat output
```
$ cat example.py
def hello():
    print("Hello, World!")

if __name__ == "__main__":
    hello()
```

### bat output
```
$ bat example.py
───────┬────────────────────────────────
       │ File: example.py
───────┼────────────────────────────────
   1   │ def hello():
   2   │     print("Hello, World!")
   3   │
   4   │ if __name__ == "__main__":
   5   │     hello()
───────┴────────────────────────────────
```

(With actual syntax highlighting in terminal - keywords colored, strings highlighted, etc.)

## Useful bat Options

```bash
# Plain output (no decorations, like cat)
bat -p file.py

# Show line range
bat -r 10:20 file.py

# Show non-printable characters
bat -A file.py

# Force syntax highlighting for stdin
echo '{"key": "value"}' | bat -l json

# List available themes
bat --list-themes

# Use specific theme
bat --theme="Dracula" file.py
```

## When to Use Each

**Use bat for:**
- Reading code files (syntax highlighting helps)
- Reviewing changes (git markers)
- Exploring unfamiliar code
- Showing code to users

**Use cat for:**
- Piping to other commands (bat's decorations can interfere)
- Scripts (portability)
- Binary file handling
- Simple concatenation

## bat as cat Replacement

Add to `~/.zshrc` if you want bat as default:

```bash
alias cat='bat'
```

Or for plain output by default:

```bash
alias cat='bat -p'
```

## Installation

```bash
brew install bat
```

## See Also

- [bat GitHub](https://github.com/sharkdp/bat)
- [when-to-use-standard-tools.md](../edge-cases/when-to-use-standard-tools.md)
