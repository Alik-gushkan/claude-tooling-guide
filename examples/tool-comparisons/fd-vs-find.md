# fd vs find: Performance Comparison

## Summary

`fd` is a modern alternative to `find` with:
- Faster performance (parallel execution)
- Simpler syntax
- Sensible defaults (ignores .git, node_modules, etc.)
- Colorized output

## Real Benchmark

Finding all JSON files in ~/Library (11,000+ files):

```bash
# fd: 6.1 seconds
$ time fd -e json . ~/Library 2>/dev/null | wc -l
   11052
fd -e json . ~/Library  0.61s user 3.72s system 71% cpu 6.085 total

# find: 10.8 seconds
$ time find ~/Library -name "*.json" 2>/dev/null | wc -l
   11107
find ~/Library -name "*.json"  0.17s user 3.49s system 33% cpu 10.787 total
```

**Result: fd is ~1.8x faster** on this real-world test.

## Syntax Comparison

| Task | fd | find |
|------|----|----- |
| Find by extension | `fd -e php` | `find . -name "*.php"` |
| Find by name pattern | `fd controller` | `find . -name "*controller*"` |
| Find and execute | `fd -e js -x wc -l` | `find . -name "*.js" -exec wc -l {} \;` |
| Exclude directory | `fd -E node_modules` | `find . -name "*.js" -not -path "*/node_modules/*"` |
| Case insensitive | `fd -i readme` | `find . -iname "*readme*"` |

## When fd Shines

1. **Interactive exploration** - Colorized output, fast feedback
2. **Large codebases** - Parallel execution matters
3. **Respects .gitignore** - No manual exclusions needed
4. **Simple patterns** - Less punctuation than find

## fd Default Behaviors

These are automatic (override with flags if needed):
- Ignores hidden files (use `-H` to include)
- Ignores .gitignore patterns (use `-I` to include)
- Case-smart search (smart-case by default)
- Colorized output

## Installation

```bash
brew install fd
```

## See Also

- [fd GitHub](https://github.com/sharkdp/fd)
- [when-to-use-standard-tools.md](../edge-cases/when-to-use-standard-tools.md) - When find is still the right choice
