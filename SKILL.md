---
name: lib-scout
description: >
  Discover high-quality open-source libraries before building or modifying projects.
  Searches GitHub, PyPI, CRAN, and Bioconductor for the best existing tools.
  USE THIS SKILL whenever the user asks to build, create, or scaffold a project, tool, or pipeline;
  asks to add features that likely have library support; mentions "find a library for" or "best package for";
  wants to build anything involving data analysis, bioinformatics, web scraping, APIs, or domains with mature libraries;
  says "build me a...", "create a...", "help me make a...";
  or is working on Python/R projects that could benefit from existing packages.
  Even if not explicitly asked — if about to write significant code, check first.
  DO NOT use for trivial tasks, when user wants to write from scratch, pure conceptual questions, or debugging existing code.
---

# Lib Scout — Find Before You Build

When you're about to build something non-trivial, **search first**. The best code is code you don't have to write.

## Core Workflow

### Step 1: Assess the Task

When the user asks you to build something, pause and think:

1. **What is the core functionality needed?** Break it down into components.
2. **Which components likely have mature library support?** Most domains do — data processing, visualization, API clients, file format handling, scientific computing, web frameworks, etc.
3. **What language/ecosystem is this?** Determines which platforms to search.

### Step 2: Search Strategically

Search across the relevant platforms for the user's ecosystem. Be smart about your queries:

**GitHub**: Best for finding complete tools, frameworks, templates, and reference implementations.
- Search with domain keywords + quality filters: `image segmentation deep learning language:python stars:>100`
- Great for: finding boilerplate projects, CLI tools, framework comparisons, and "awesome-X" lists

**PyPI**: Best for installable Python packages.
- Search with functional keywords: `async web scraping`, `dataframe validation`
- Check the package's PyPI page for download stats, last release date, and dependency count

**CRAN / r-universe**: Best for R statistical packages.
- Search with statistical/methodological terms: `survival analysis`, `mixed effects models`
- Check reverse dependencies count as a signal of community adoption

**Bioconductor**: Best for bioinformatics R packages.
- Search with biological terms: `single cell RNA-seq`, `ChIP-seq peak calling`, `flow cytometry`
- Especially useful for genomics, proteomics, and imaging analysis pipelines

**Tips for effective searching:**
- Start broad, then narrow. `"image segmentation"` first, then `"image segmentation microscopy cellpose"` if needed.
- Search GitHub and the language-specific registry separately — they surface different things.
- For niche domains, also try searching for "awesome-X" lists on GitHub (e.g., `awesome-single-cell`).
- When comparing options, open the GitHub repos and skim the README, recent issues, and commit frequency.

### Step 3: Evaluate Quality

After getting results, filter by these quality signals (in order of importance):

1. **Active maintenance**: Updated within the last 6–12 months. Stale repos are red flags.
2. **Community adoption**: Stars, forks, download counts. More usage = more battle-tested.
3. **Documentation quality**: Good README, examples, API docs? If you can't figure out how to use it quickly, skip it.
4. **License compatibility**: MIT/Apache/BSD are safe. GPL may have implications. Note any license concerns.
5. **Dependency footprint**: Fewer deps = fewer things to break. Flag packages that pull in heavy dependency trees.
6. **Python version / R version support**: Must be compatible with the user's environment.

### Step 4: Present Recommendations

Present your findings to the user in a concise format **before writing any code**:

```
## Library Recommendations

For [task description], I found these options:

**Recommended: [package-name]** (⭐ X stars, updated YYYY-MM)
- Why: [1-2 sentences on why this is the best fit]
- Install: `pip install X` / `install.packages("X")`
- Docs: [url]

**Alternative: [package-name]** (⭐ Y stars)
- Why you might prefer this: [trade-off explanation]

**Also considered but not recommended:**
- [package]: [brief reason — e.g., "last updated 2021", "GPL license", "overkill for this task"]

Shall I proceed with [recommended package]?
```

### Step 5: Build with the Library

Once the user confirms (or if the choice is obvious and low-stakes), proceed to build using the selected library. Refer to the library's documentation for best practices.

## Search Strategy by Domain

Here are effective search patterns for common domains:

| Domain | GitHub query | PyPI query | CRAN/Bioc query |
|--------|-------------|------------|-----------------|
| Data viz | `visualization dashboard language:python stars:>200` | `interactive visualization` | `ggplot2 extensions` |
| ML/DL | `machine learning framework language:python stars:>500` | `scikit-learn compatible` | `caret tidymodels` |
| Bioinformatics | `bioinformatics pipeline language:python` | `biopython scanpy` | Bioc: `single cell RNA` |
| Web scraping | `web scraping async language:python stars:>100` | `async scraping` | — |
| File processing | `pdf extraction language:python` | `pdf parsing` | — |
| API clients | `[service-name] api client language:python` | `[service-name]` | — |
| Statistics | `statistical analysis language:r stars:>50` | `scipy statsmodels` | `survival lme4` |
| Image analysis | `image processing analysis language:python` | `scikit-image opencv` | Bioc: `EBImage` |

## Important Notes

- **Don't over-search**: For simple tasks where you're confident about the standard tool (e.g., `requests` for HTTP, `pandas` for dataframes), just use it. Search when you're in unfamiliar territory or when there might be a better specialized tool.
- **Respect the user's preferences**: If they already specified a library or approach, don't second-guess it unless you see a clear issue.
- **Be honest about trade-offs**: No library is perfect. Mention the downsides too.
- **Check for breaking changes**: If recommending a major version (v2.0 etc.), note whether migration from older versions is an issue.
