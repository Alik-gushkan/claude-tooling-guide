# Workflow: Finding PHP Errors in a Laravel Project

This example demonstrates a complete debugging workflow using modern CLI tools.

## Scenario

You need to find PHP files with syntax errors or deprecated function usage in a Laravel project.

## Traditional Approach (find + cat + grep)

```bash
# Find PHP files
find app -name "*.php" -type f

# Search for deprecated mysql_* functions
find app -name "*.php" -type f -exec grep -l "mysql_" {} \;

# View context around matches
find app -name "*.php" -type f -exec grep -n -B2 -A2 "mysql_" {} \;
```

Problems:
- Slow on large codebases
- Searches node_modules, vendor, etc.
- No syntax highlighting in output
- Verbose command syntax

## Modern Approach (fd + bat + rg)

```bash
# Find PHP files (automatically ignores vendor, node_modules)
fd -e php

# Search for deprecated mysql_* functions
rg "mysql_" -t php

# View matches with syntax highlighting and context
rg "mysql_" -t php -C 3 | bat -l php

# Find files and preview each with bat
fd -e php -x bat {}
```

Benefits:
- Faster execution
- Respects .gitignore
- Syntax-highlighted output
- Cleaner syntax

## Complete Debugging Session

### Step 1: Get overview of PHP files

```bash
$ fd -e php | wc -l
247

$ fd -e php | head -10
app/Console/Kernel.php
app/Exceptions/Handler.php
app/Http/Controllers/Controller.php
...
```

### Step 2: Search for potential issues

```bash
# Deprecated functions
$ rg "mysql_query|mysql_connect|mysql_fetch" -t php
app/Legacy/DatabaseHelper.php:45:    $result = mysql_query($sql);

# Error suppression
$ rg "@\$" -t php
app/Helpers/FileHelper.php:23:    $content = @file_get_contents($path);
```

### Step 3: Review problematic files

```bash
$ bat app/Legacy/DatabaseHelper.php -r 40:50
───────┬────────────────────────────────────────
       │ File: app/Legacy/DatabaseHelper.php
───────┼────────────────────────────────────────
  40   │     public function query($sql)
  41   │     {
  42   │         // TODO: Migrate to PDO
  43   │         $connection = mysql_connect(
  44   │             $this->host,
  45   │             $this->user,
  46   │             $this->pass
  47   │         );
  48   │         $result = mysql_query($sql);
  49   │         return $result;
  50   │     }
───────┴────────────────────────────────────────
```

### Step 4: Check for related issues

```bash
# Find all files referencing this helper
$ rg "DatabaseHelper" -t php -l
app/Legacy/DatabaseHelper.php
app/Services/ReportService.php
app/Console/Commands/GenerateReport.php

# See the usage context
$ rg "DatabaseHelper" -t php -C 2
```

## Tool Summary

| Task | Tool | Command |
|------|------|---------|
| Find files | fd | `fd -e php` |
| Search content | rg (ripgrep) | `rg "pattern" -t php` |
| View files | bat | `bat file.php` |
| View line range | bat | `bat -r 10:20 file.php` |
| Search + highlight | rg + bat | `rg "pattern" -t php | bat -l php` |

## Tips

1. **Start broad, narrow down**: Use `fd` to understand scope, then `rg` to find specifics
2. **Use file types**: `-t php` is cleaner than `-g "*.php"`
3. **Preview with context**: `rg -C 3` shows 3 lines before/after matches
4. **Pipe to bat**: Syntax highlighting makes patterns easier to spot
