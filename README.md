# Claude Tooling Guide

A structured approach to selecting and configuring CLI tools for Claude Code workflows. This guide documents an interview-driven methodology for making tooling decisions and provides ready-to-use configurations.

## Who This Is For

- **Claude Code users** who want optimized tool configurations
- **Anyone** interested in a systematic approach to CLI tool selection

## Quick Start

### 1. Install the Tools

```bash
brew install fd fzf bat htop tree yq ffmpeg wget
```

### 2. Enable fzf Shell Integration

Add to `~/.zshrc`:
```bash
eval "$(fzf --zsh)"
```

Then run `source ~/.zshrc` or restart your terminal.

### 3. Configure Claude Code

Copy the template to your global Claude config:

```bash
mkdir -p ~/.claude
curl -o ~/.claude/CLAUDE.md https://raw.githubusercontent.com/Alik-gushkan/claude-tooling-guide/main/templates/CLAUDE.md
```

Or manually create `~/.claude/CLAUDE.md` with the content from [templates/CLAUDE.md](templates/CLAUDE.md).

## The Interview Transcript

Below is the complete annotated interview that produced these recommendations. Each question explores a specific decision point, with commentary explaining the reasoning.

---

### Decision 1: Documentation Location

**Q: Where should tool preferences be documented?**

**Options:**
- A) Global `~/.claude/CLAUDE.md` - Single source of truth, applies everywhere
- B) Per-project `CLAUDE.md` files - Project-specific customization
- C) Both with inheritance

**Decision: A) Global `~/.claude/CLAUDE.md`**

> *Commentary: A global config ensures consistency across all projects. Tool preferences like "use fd instead of find" are universal - they don't change project to project. Per-project configs are better for project-specific conventions.*

---

### Decision 2: Enforcement Level

**Q: Should there be hard denies (tool X is forbidden) or soft guidance (prefer tool Y)?**

**Options:**
- A) Hard denies - Explicitly forbid certain tools
- B) Soft guidance - Recommend preferred tools without forbidding alternatives
- C) Mixed approach

**Decision: B) Soft guidance**

> *Commentary: Soft guidance provides the benefits of better tools without creating friction. There are edge cases where standard tools are appropriate (generated scripts, portability requirements). Hard denies would require constant exception handling.*

---

### Decision 3: fzf Shell Integration

**Q: Should fzf shell integration be installed?**

**Options:**
- A) Yes - Adds Ctrl+R (fuzzy history), Ctrl+T (file finder), Alt+C (directory jump)
- B) No - Use fzf only programmatically

**Decision: A) Yes, install shell integration**

> *Commentary: The keyboard shortcuts dramatically improve terminal productivity. Ctrl+R alone transforms command history from "scroll through recent commands" to "instantly find any command you've ever run."*

---

### Decision 4: Documentation Style

**Q: How verbose should the CLAUDE.md instructions be?**

**Options:**
- A) Directive-based - Concise rules ("Use fd instead of find")
- B) Explanatory - Include rationale with each rule
- C) Example-heavy - Show usage examples for each tool

**Decision: A) Directive-based**

> *Commentary: Claude doesn't need explanations of why fd is faster - it needs clear instructions on what to use. Explanatory text wastes context window space. Examples belong in separate documentation.*

---

### Decision 5: Code Generation Scope

**Q: Should generated code (scripts, configs) also use the modern tools?**

**Options:**
- A) Always use modern tools - Consistency everywhere
- B) Standard tools for generated code - Portability for scripts
- C) Ask each time

**Decision: B) Standard tools for generated code**

> *Commentary: A bash script using fd won't work on systems without fd installed. Claude's own commands benefit from modern tools, but generated artifacts should be portable. This distinction is important.*

---

### Decision 6: Maintenance Tracking

**Q: How should tool installation/updates be tracked?**

**Options:**
- A) Detailed changelog with versions
- B) Minimal timestamp with upgrade command
- C) No tracking

**Decision: B) Minimal timestamp with upgrade command**

> *Commentary: One line with install date and the upgrade command provides just enough information. Version tracking is overkill - brew handles that. The upgrade command makes maintenance trivial.*

---

### Decision 7: CLAUDE.md Priority

**Q: Where in CLAUDE.md should tool preferences appear?**

**Options:**
- A) Top (high priority)
- B) Middle
- C) Bottom

**Decision: A) Top (high priority)**

> *Commentary: Claude processes CLAUDE.md early in context. Tool preferences should be established before any file operations occur. Putting them at the top ensures they're seen before the first `find` command.*

---

### Decision 8: Repository Audience

**Q: Who is the target audience for the public documentation?**

**Options:**
- A) Claude Code users only
- B) Broader audience (anyone interested in CLI tooling)
- C) Both with clear sections

**Decision: C) Both**

> *Commentary: The methodology (interview-driven tool selection) is universally applicable. The implementation (CLAUDE.md config) is Claude Code-specific. Structuring the repo to serve both audiences maximizes value.*

---

### Decision 9: Repository Focus

**Q: What should the repo primarily contain?**

**Options:**
- A) Process documentation only - The methodology
- B) Implementation only - Just configs and examples
- C) Both process and implementation

**Decision: C) Both process and implementation**

> *Commentary: Process without implementation is theoretical. Implementation without process is cargo-culting. Both together allow readers to understand the reasoning AND get working configs.*

---

### Decision 10: Transcript Format

**Q: How should the interview be presented?**

**Options:**
- A) Raw Q&A only
- B) Annotated with commentary
- C) Summary tables only

**Decision: B) Annotated**

> *Commentary: Raw transcripts lack context. Summaries lose nuance. Annotations explain the reasoning behind each decision, making the document educational rather than just documentary.*

---

### Decision 11: Repository Structure

**Q: How should the repo be organized?**

**Options:**
- A) Single README with everything
- B) README + examples/ folder
- C) Complex multi-directory structure

**Decision: B) README + examples/**

> *Commentary: A single README gets too long. Deep directory hierarchies are hard to navigate. Two levels (README for core content, examples/ for demonstrations) balances comprehensiveness with usability.*

---

### Decision 12: Example Scenarios

**Q: What types of examples should be included?**

**Options:**
- A) Basic - Just tool usage
- B) Comparative - Before/after, tool vs tool
- C) Comprehensive - Comparisons + workflows + edge cases

**Decision: C) Comprehensive**

> *Commentary: Basic examples are available in tool documentation. Comparative examples show why the tools matter. Workflow examples show real-world application. Edge cases prevent misuse. All three together create complete documentation.*

---

### Decision 13: Repository Name

**Q: What should the repo be named?**

**Options:**
- A) `claude-tooling-guide`
- B) `cli-tool-config`
- C) `dev-environment-setup`

**Decision: A) `claude-tooling-guide`**

> *Commentary: Clear, specific, discoverable. Targets the Claude community while indicating the content (tooling guide). Generic names like "cli-config" get lost in search results.*

---

### Decision 14: License

**Q: What license should be used?**

**Options:**
- A) MIT
- B) Apache 2.0
- C) CC BY

**Decision: A) MIT**

> *Commentary: Maximum freedom with minimal legalese. Standard choice for this type of content. No patent concerns (Apache 2.0) or attribution complexity (CC BY) needed.*

---

### Decision 15: Initial Scope

**Q: What should be included in the initial release?**

**Options:**
- A) Minimal - Just README
- B) Standard - README + template
- C) Full - README + template + all examples

**Decision: C) Full package**

> *Commentary: Ship complete. A minimal release would require users to wait for examples that demonstrate the value. Launching with everything lets users immediately see the full picture.*

---

## Decision Summary

| # | Topic | Decision |
|---|-------|----------|
| 1 | Documentation Location | Global `~/.claude/CLAUDE.md` |
| 2 | Enforcement | Soft guidance (no hard denies) |
| 3 | fzf Integration | Yes, with shell keybindings |
| 4 | Doc Style | Directive-based |
| 5 | Generated Code | Use standard tools for portability |
| 6 | Maintenance | Minimal timestamp + upgrade command |
| 7 | CLAUDE.md Position | Top (high priority) |
| 8 | Audience | Both Claude Code + broader |
| 9 | Focus | Process + implementation |
| 10 | Transcript | Annotated with commentary |
| 11 | Structure | README + examples/ |
| 12 | Examples | Comprehensive |
| 13 | Name | `claude-tooling-guide` |
| 14 | License | MIT |
| 15 | Scope | Full package |

## Examples

See the [examples/](examples/) directory for:

- **[tool-comparisons/](examples/tool-comparisons/)** - Performance and feature comparisons
  - [fd-vs-find.md](examples/tool-comparisons/fd-vs-find.md) - Speed benchmarks with real data
  - [bat-vs-cat.md](examples/tool-comparisons/bat-vs-cat.md) - Feature comparison

- **[workflows/](examples/workflows/)** - Real-world usage patterns
  - [find-php-errors.md](examples/workflows/find-php-errors.md) - Complete debugging workflow

- **[edge-cases/](examples/edge-cases/)** - When to use standard tools
  - [when-to-use-standard-tools.md](examples/edge-cases/when-to-use-standard-tools.md) - Portability considerations

## Templates

- [templates/CLAUDE.md](templates/CLAUDE.md) - Copy-paste ready Claude Code configuration

## License

MIT - See [LICENSE](LICENSE)
