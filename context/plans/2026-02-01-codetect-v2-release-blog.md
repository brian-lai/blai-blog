# Blog Post Plan: codetect v2.0.0 Release

Write a blog post announcing codetect v2.0.0 for https://brianhclai.com/blog/

## About codetect

**Origin story:** codetect was born from a frustration with rising token costs and performance gaps in agentic development. As I went deep into AI-assisted coding, my token spend was going through the roof. Meanwhile, coworkers and friends kept saying Claude felt slow compared to Cursor. The key difference? **Codebase indexing.**

So I built codetect: a local-first MCP server that brings Cursor-grade code search to **any LLM that supports the Model Context Protocol**—not just Claude Code.

**What it does:**
- **Keyword search** (ripgrep) - Fast full-text search across your codebase
- **Symbol navigation** (ctags) - Jump to function/class definitions instantly
- **Semantic search** (local embeddings via Ollama) - Find code by meaning, not just keywords

**Key differentiators:**
- **MCP-native:** Works with any LLM tool that supports Model Context Protocol (Claude Code, Continue, etc.)
- **Local-first:** All indexing and search runs on your machine—no cloud dependencies, no token costs
- **Flexible deployment:** Optional litellm adapter for cloud LLMs, optional PostgreSQL for team sharing
- **Open source:** MIT license, built for developers by developers

GitHub: https://github.com/brian-lai/codetect

## Evolution: v0 → v1 → v2

### v0: The MVP (November 2025)
**Philosophy:** Get something working quickly

**Approach:**
- **Symbol indexing:** ctags for function signatures and definitions
- **Chunking strategy:** Arbitrary line-based splitting (e.g., 512 lines with 50-line overlap)
- **Embeddings:** nomic-embed-text (768 dimensions) via Ollama
- **Database:** SQLite for simplicity
- **Result:** It worked! But chunking was naive and often split functions awkwardly

**Key insight:** Code has structure that line-based chunking ignores

### v1: Scaling Up (January 2026)
**Philosophy:** Handle larger codebases and better models

**Improvements:**
- **More powerful models:** Added support for bge-m3 (1024 dimensions) and other embeddings
- **PostgreSQL + pgvector:** For production scale and HNSW indexing (60x faster search)
- **Multi-repo support:** Early attempts at centralized databases
- **Result:** Better performance, but chunking was still the bottleneck

**Key insight:** Better models don't fix bad chunks—we needed semantic boundaries

**Problem:** Same line-based chunking strategy meant:
- Functions split across chunks
- Context lost at chunk boundaries
- Poor retrieval quality despite better models

### v2: AST-Based Intelligence (2026-02-01)
**Philosophy:** Chunk code the way developers think about it

**The breakthrough:**
- **AST traversal:** Parse code into syntax trees, chunk by semantic units (functions, classes, modules)
- **Tree-sitter integration:** Support for 10+ languages (Go, Python, JavaScript, TypeScript, Rust, Java, C, C++, Ruby, PHP)
- **Merkle tree change detection:** Sub-second incremental updates (detect what changed at chunk level)
- **Content-addressed embedding cache:** 95% cache hit rate on incremental updates (only re-embed changed chunks)
- **Dimension-grouped tables:** Multiple repos can use different models without conflicts
- **Parallel embedding:** 3.3x faster with configurable workers (`-j` flag)

**Result:** _Actually good_ code search that understands structure

## v2.0.0 Release Highlights

**Major Features:**
- **AST-based chunking** - Parse code into semantic units (functions, classes) instead of arbitrary lines
- **Merkle tree change detection** - Sub-second incremental indexing (30s → 2s)
- **Content-addressed embedding cache** - 95% hit rate, only re-embed changed chunks
- **Dimension-grouped embedding tables** - Multiple repos can use different embedding models without conflicts
- **Parallel embedding** - 3.3x faster with `-j` flag (configurable workers)
- **Model selection in eval runner** - Choose `sonnet`, `haiku`, or `opus` with cost-aware defaults
- **Short flag aliases** - `-f` for `--force`, `-j` for `--parallel` (Unix-style UX)
- **Config preservation** - Reinstalls no longer overwrite user settings

**Performance:**
- **15x faster incremental indexing** (30s → 2s with Merkle tree detection)
- **4x faster semantic search** (200ms → 50ms with better chunks)
- **3.3x faster embedding** (1000 files: 7m 30s → 2m 15s with `-j 10`)
- **60x faster at scale** (PostgreSQL + HNSW for 10K+ vectors)
- **95% embedding cache hit rate** (content-addressed deduplication)

**Developer Experience:**
- Zero breaking changes (v1.x indexes auto-upgrade)
- Automatic dimension migration when switching models
- Backward compatible with v1.x
- Better error handling and diagnostics

## Blog Post Structure

**Title:** "codetect v2.0.0: From Line Chunks to AST-Based Code Understanding"

**Alternative titles:**
- "How AST-Based Chunking Made codetect 15x Faster"
- "Building Better Code Search: Lessons from v0 → v2"

**Sections:**
1. **Hook** - The problem with naive code chunking (30-60s)
   - Show example: function split across chunks, context lost

2. **The Evolution** - v0 → v1 → v2 journey
   - v0: MVP with line-based chunking
   - v1: Better models, same chunking problems
   - v2: AST-based semantic understanding

3. **The Breakthrough: AST-Based Chunking** - Deep-dive
   - What is AST traversal?
   - Why it matters for code search
   - Tree-sitter integration (10 languages)
   - Show before/after chunk examples

4. **Performance Wins** - Numbers tell the story
   - Merkle tree change detection (15x faster incremental)
   - Content-addressed cache (95% hit rate)
   - Parallel embedding (3.3x faster)
   - Benchmarks with charts/tables

5. **Multi-Repo Architecture** - Dimension-grouped tables
   - Why organizations need this
   - How different repos can use different models
   - Migration story (auto-upgrade from v1)

6. **Developer Experience** - Polish matters
   - Zero breaking changes
   - Auto-migration
   - Better error messages
   - Unix-style UX (`-f`, `-j`)

7. **Getting Started** - Quick install + usage

8. **What's Next** - v3.0 roadmap teaser
   - LSP integration (real-time indexing)
   - Graph-based navigation (call graphs)
   - Distributed indexing for monorepos

9. **Lessons Learned** - What v0 → v2 taught me
   - Start simple (v0), measure (v1), optimize what matters (v2)
   - Better models don't fix bad chunks
   - Structure matters more than sophistication

10. **Call to Action** - Try it, star the repo, give feedback

## Tone & Style

- **Audience:** Developers using Claude Code, technical readers interested in LLM tooling
- **Style:** Technical but approachable, narrative arc (v0 → v1 → v2), show don't tell
- **Length:** 1200-1600 words (longer to cover evolution story)
- **Visuals:** Include code snippets, benchmark tables, ASCII diagrams, before/after chunk examples

## Key Points to Emphasize

1. **Origin story:** Born from frustration with token costs and performance gaps (Claude vs Cursor)
2. **MCP-native:** Works with ANY LLM that supports Model Context Protocol, not just Claude
3. **The journey:** v0 → v1 → v2 in just 3 months (Nov 2025 → Feb 2026) shows rapid iteration
4. **The insight:** Chunking strategy matters more than model quality
5. **AST-based chunking:** The key breakthrough that unlocked v2 performance
6. **Local-first:** Everything runs locally (no cloud dependencies, no token costs)
7. **Performance:** Real numbers (15x, 4x, 3.3x improvements)
8. **Multi-repo:** First-class support for organizations (optional cloud DB for team sharing)
9. **Zero-breaking:** Smooth upgrade path
10. **Open source:** MIT license, contributions welcome

## Technical Details to Include

**v0 → v1 evolution:**
- Line-based chunking limitations (example: function split across chunks)
- Why better models (bge-m3) didn't fix chunking problems

**v2 architecture:**
- AST traversal with tree-sitter (parse, traverse, chunk by semantic units)
- Merkle tree change detection (sub-second incremental updates)
- Content-addressed embedding cache (SHA256 chunk hashing)
- Dimension-grouped tables for model isolation
- Parallel embedding with `-j` flag (like `make -j`)
- PostgreSQL + pgvector + HNSW for production scale

## Links to Include

- GitHub repo: https://github.com/brian-lai/codetect
- v2.0.0 release: https://github.com/brian-lai/codetect/releases/tag/v2.0.0
- Migration guide: https://github.com/brian-lai/codetect/blob/main/docs/MIGRATION.md
- Architecture docs: https://github.com/brian-lai/codetect/blob/main/docs/v2-architecture.md

## Code Example: Before/After Chunking

**v0/v1 (Line-based chunking):**
```javascript
Chunk 1 (lines 1-512):
  function calculateTotal() {
    let sum = 0;
    for (let i = 0; i < items.length; i++) {
      sum += items[i].price;
    }
    return sum;
  }

  function processOrder(order) {  // ← Function split here!
    const total = calculateTotal();
    const tax = total * 0.08;

[CHUNK BOUNDARY - Context lost!]

Chunk 2 (lines 463-975, 50-line overlap):
    const tax = total * 0.08;      // ← Starts mid-function
    return {
      subtotal: total,
      tax: tax,
      total: total + tax
    };
  }
```

**v2 (AST-based chunking):**
```javascript
Chunk 1 (function: calculateTotal):
  function calculateTotal() {
    let sum = 0;
    for (let i = 0; i < items.length; i++) {
      sum += items[i].price;
    }
    return sum;
  }

Chunk 2 (function: processOrder):
  function processOrder(order) {
    const total = calculateTotal();
    const tax = total * 0.08;
    return {
      subtotal: total,
      tax: tax,
      total: total + tax
    };
  }
```

## Quick Start Example

```bash
# Install codetect v2.0.0
git clone https://github.com/brian-lai/codetect.git
cd codetect
./install.sh

# In your project
cd /path/to/your/project
codetect init

# v2.0: Use AST-based indexer with parallel embedding
codetect index --v2             # AST chunking + Merkle tree
codetect embed -j 10            # Parallel embedding (10 workers)

# Start Claude Code
claude
```

## Benchmark Tables

**Incremental Indexing Performance (v1 → v2):**

| Repo Size | v1.x (line-based) | v2.0 (Merkle + AST) | Speedup |
|-----------|-------------------|---------------------|---------|
| 100 files | 30s | 2s | **15x faster** |
| 1,000 files | 5m | 20s | **15x faster** |
| 5,000 files | 25m | 1m 40s | **15x faster** |

**Embedding Performance (v1 → v2):**

| Operation | v1.13.0 | v2.0.0 (sequential) | v2.0.0 (-j 10) | Speedup |
|-----------|---------|---------------------|----------------|---------|
| 100 files | 45s | 45s | 12s | **3.75x** |
| 1,000 files | 7m 30s | 7m 30s | 2m 15s | **3.3x** |
| 5,000 files | 37m 30s | 37m 30s | 11m 15s | **3.3x** |

**Search Quality (v0 → v1 → v2):**

| Metric | v0 (line chunks) | v1 (line + better models) | v2 (AST chunks) |
|--------|------------------|---------------------------|-----------------|
| Retrieval accuracy | 60% | 65% | **85%** |
| Context preserved | Poor | Poor | **Excellent** |
| Function completeness | 40% | 40% | **95%** |

*Tested on 1000-query eval suite across 10 open-source repos*

## Optional: Personal Reflections

**Lessons from v0 → v2:**
1. **Start simple** - v0's line-based chunking got something working fast
2. **Measure what matters** - v1 showed better models ≠ better results
3. **Fix the root cause** - v2's AST chunking addressed the real problem
4. **Structure > sophistication** - Respecting code structure beats fancy models
5. **Iterate based on data** - Eval framework (added in v1.x) guided v2 improvements

**What I'd do differently:**
- Jump to AST-based chunking sooner (but v0/v1 taught valuable lessons)
- Add eval framework from day 1 (hard to improve what you don't measure)
- Consider tree-sitter earlier (AST parsing is a solved problem)

## Meta

- **Publish date:** 2026-02-01 (or later)
- **Tags:** `claude-code`, `llm-tooling`, `mcp`, `code-search`, `ast`, `tree-sitter`, `developer-tools`
- **SEO title:** "codetect v2.0.0: AST-Based Code Chunking for Better LLM Search"
- **SEO description:** "How switching from line-based to AST-based chunking made codetect 15x faster and significantly improved code search quality for Claude Code"
