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

### Step 0: Should I Search?

Before searching, run this decision gate:

**Skip search if ANY of these are true:**
- The project already has an established dependency for this domain (e.g., project uses FastAPI — don't search for web frameworks)
- The task uses well-known standard tools (see Known Solutions table below)
- You're extending existing code that already imports the relevant library
- The task is pure glue code, configuration, or UI work with no algorithmic complexity
- The user explicitly said to build from scratch

**Search if ANY of these are true:**
- The domain is niche or specialized (bioinformatics, ontology management, PDF parsing, etc.)
- You're unsure whether a mature solution exists
- The user explicitly asks for library recommendations
- Building from scratch would take 100+ lines for something that likely exists as a package

### Known Solutions (Skip Search)

For these common needs, use the standard tool directly — no search needed:

| Need | Python | JavaScript/TS | R |
|------|--------|---------------|---|
| HTTP requests | `requests` / `httpx` | `axios` / `fetch` | `httr2` |
| DataFrames | `pandas` | — | `dplyr` / `tidyverse` |
| JSON | `json` (stdlib) | built-in | `jsonlite` |
| CSV | `pandas` / `csv` | `papaparse` | `readr` |
| Web framework | `FastAPI` / `Flask` / `Django` | `Next.js` / `Express` | `shiny` / `plumber` |
| ORM / DB | `SQLAlchemy` / `asyncpg` | `Prisma` / `Drizzle` | `DBI` / `dbplyr` |
| Testing | `pytest` | `vitest` / `jest` | `testthat` |
| CLI | `click` / `typer` | `commander` | `optparse` |
| Dates | `datetime` / `pendulum` | `date-fns` / `dayjs` | `lubridate` |
| Async | `asyncio` / `aiohttp` | built-in | — |
| Plotting | `matplotlib` / `seaborn` / `plotly` | `d3` / `recharts` | `ggplot2` |
| ML basics | `scikit-learn` | — | `tidymodels` / `caret` |
| Image | `Pillow` / `opencv-python` | `sharp` | — |
| Regex | `re` (stdlib) | built-in | `stringr` |

If the task fits this table, just use the library. Don't waste time confirming it.

### Step 1: Assess the Task

When the user asks you to build something and you passed Step 0 (search is warranted), think:

1. **What is the core functionality needed?** Break it down into components.
2. **Which components likely have mature library support?** Most domains do — data processing, visualization, API clients, file format handling, scientific computing, web frameworks, etc.
3. **What language/ecosystem is this?** Determines which platforms to search.

### Step 2: Search Strategically

Search across the relevant platforms for the user's ecosystem. **Keep queries short and focused.**

#### Query Formatting Rules

**CRITICAL — most empty results come from bad queries:**

| Rule | Bad | Good |
|------|-----|------|
| Max 2-3 keywords | `hierarchical topic taxonomy ontology management python` | `topic ontology` |
| No language in PyPI/CRAN queries | `python async scraping` | `async scraping` |
| Use `language:` filter on GitHub | `python web framework` | `web framework language:python` |
| Functional terms, not descriptions | `tool for managing hierarchies` | `hierarchy management` |
| Split compound needs into separate queries | `pdf parsing and ocr extraction` | Query 1: `pdf parsing`, Query 2: `ocr extraction` |

**Platform-specific tips:**

**GitHub**: Best for complete tools, frameworks, and reference implementations.
- Always use `language:` and `stars:>` filters: `"ontology management" language:python stars:>50`
- Great for: boilerplate projects, CLI tools, framework comparisons, and "awesome-X" lists

**PyPI**: Best for installable Python packages.
- Keep to 1-2 functional keywords: `async scraping`, `dataframe validation`
- Check the package's PyPI page for download stats, last release date, and dependency count

**CRAN / r-universe**: Best for R statistical packages.
- Use methodological terms: `survival analysis`, `mixed effects`
- Check reverse dependencies count as a signal of community adoption

**Bioconductor**: Best for bioinformatics R packages.
- Use biological terms: `single cell RNA`, `ChIP-seq`, `flow cytometry`
- Especially useful for genomics, proteomics, and imaging analysis pipelines

#### Fallback Strategy (When Results Are Empty)

If a search returns 0 results, do NOT give up — retry with progressively broader queries:

1. **Broaden keywords**: Drop the most specific term. `topic ontology python` → `ontology`
2. **Try synonyms**: `hierarchy` → `taxonomy`, `tree structure`, `DAG`
3. **Switch platform**: PyPI empty → try GitHub. GitHub empty → try PyPI with different terms
4. **Search for "awesome" lists**: On GitHub, try `awesome-[domain]` (e.g., `awesome-ontology`)
5. **Give up gracefully**: After 2-3 retries across platforms, tell the user: "No mature library found for this — recommend building from scratch" — this is a valid and useful outcome

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

- **Minimize wasted searches**: Always run Step 0 first. Most tasks in an existing project don't need a library search.
- **Short queries**: 2-3 keywords max. This is the #1 cause of empty results. When in doubt, shorter is better.
- **Respect the user's preferences**: If they already specified a library or approach, don't second-guess it unless you see a clear issue.
- **Be honest about trade-offs**: No library is perfect. Mention the downsides too.
- **Check existing deps first**: Before recommending a new library, check `requirements.txt` / `package.json` / `pyproject.toml` — the project may already have a dependency that covers the need.
- **"No library found" is a valid answer**: Don't force a recommendation. If nothing good exists, say so and move on to building.
- **Check for breaking changes**: If recommending a major version (v2.0 etc.), note whether migration from older versions is an issue.
