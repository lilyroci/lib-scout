# 🔍 Lib Scout

**Find before you build.** A Claude Code Skill that makes Claude search for high-quality open-source libraries before writing code from scratch.

## The Problem

When you ask an AI coding assistant to build something, it often jumps straight into writing code — even when a well-maintained library already does exactly what you need. You end up with a hand-rolled 200-line solution when `pip install thing` would have done the job.

## The Solution

Lib Scout teaches Claude to **search first, build second**. When you ask Claude Code to build something non-trivial, it will:

1. Break the task into components
2. Search GitHub, PyPI, CRAN, and/or Bioconductor for existing packages
3. Evaluate quality (stars, maintenance, docs, license, dependencies)
4. Present top recommendations with trade-offs
5. Build with the best library after you confirm

## Install

Copy the skill folder to your Claude Code skills directory:

```bash
# Global (available in all projects)
cp -r lib-scout ~/.claude/skills/lib-scout

# Or project-level
cp -r lib-scout .claude/skills/lib-scout
```

That's it. No dependencies, no configuration.

## Usage

Just use Claude Code as usual. The skill activates automatically when you ask to build things:

```
> Help me build a Python script that does image segmentation on microscopy data
# Claude searches for cellpose, stardist, etc. before writing code

> Create an R pipeline for survival analysis with competing risks
# Claude finds the right CRAN packages first

> Build me an async web scraper for job listings
# Claude checks for existing scraping frameworks
```

You can also ask explicitly:

```
> What's the best Python library for PDF table extraction?
> Find me an R package for mixed-effects models with good documentation
```

## What it covers

- **GitHub**: Frameworks, templates, reference implementations, awesome-lists
- **PyPI**: Python packages (searched via GitHub discovery + PyPI JSON API)
- **CRAN / r-universe**: R statistical packages
- **Bioconductor**: Bioinformatics packages (genomics, single-cell, proteomics, imaging)

## Quality signals it evaluates

1. Active maintenance (updated within 6–12 months)
2. Community adoption (stars, forks, downloads)
3. Documentation quality
4. License compatibility
5. Dependency footprint
6. Language version support

## When it does NOT trigger

- Trivial tasks (e.g., "write a for loop")
- When you explicitly want to write from scratch
- Pure conceptual or theoretical questions
- Debugging existing code with dependencies already chosen

## Optional: Pair with MCP Server

For more powerful, structured searching (direct API access to GitHub/PyPI/CRAN with filters), you can pair this skill with the companion [lib-scout-mcp](lib-scout-mcp/) MCP server. The skill works great on its own though — Claude Code's built-in web search handles the discovery just fine.

### MCP Server Setup

1. Copy the `lib-scout-mcp/` folder somewhere on your machine (e.g., `~/lib-scout-mcp`)
2. Install dependencies: `pip install mcp httpx pydantic`
3. Add the server to `~/.claude.json` (**not** `~/.claude/claude_code_config.json`):

**macOS / Linux:**
```json
{
  "mcpServers": {
    "lib_scout_mcp": {
      "type": "stdio",
      "command": "python",
      "args": ["/path/to/lib-scout-mcp/server.py"],
      "env": {
        "GITHUB_TOKEN": "ghp_your_token_here"
      }
    }
  }
}
```

**Windows:**
```json
{
  "mcpServers": {
    "lib_scout_mcp": {
      "type": "stdio",
      "command": "cmd",
      "args": ["/c", "py", "C:\\path\\to\\lib-scout-mcp\\server.py"],
      "env": {
        "GITHUB_TOKEN": "ghp_your_token_here"
      }
    }
  }
}
```

> **Note:** On Windows, you must use `cmd /c` to wrap the command. Setting `GITHUB_TOKEN` is optional but recommended — it raises the GitHub API rate limit from 60 to 5,000 requests/hour.

4. Restart Claude Code and verify with `/mcp` command

## License

MIT
