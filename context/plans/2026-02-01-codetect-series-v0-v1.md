# Blog Post Plan: codetect v0 & v1 Retrospective Series

Write retroactive blog posts for codetect v0 (Nov 2025) and v1 (Jan 2026), then update v2 post to link back to create a cohesive series.

---

## Blog Post 1: codetect v0 - Building an MCP Code Search Tool

**Published:** November 2025 (retroactive)
**Target audience:** Developers interested in LLM tooling, MCP protocol, local-first tools

### Structure

1. **The Problem** (full origin story - will be abbreviated in v2)
   - Agentic development token costs going through the roof
   - Claude vs Cursor performance gap
   - The insight: codebase indexing is the differentiator
   - Why existing solutions didn't fit: cloud dependencies, vendor lock-in

2. **The Goal**
   - Build MCP-native code search for ANY LLM
   - Local-first (no token costs, no cloud dependencies)
   - Fast enough to be useful in real development
   - Open source

3. **The MVP Approach**
   - Keep it simple: SQLite + ripgrep + ctags + embeddings
   - Line-based chunking (512 lines, 50-line overlap)
   - nomic-embed-text (768 dimensions) via Ollama
   - Why these choices (speed to ship vs perfection)

4. **Implementation Details**
   - MCP server architecture
   - Three search modes: keyword, symbol, semantic
   - Local embeddings with Ollama (no cloud API costs)
   - Simple SQLite database

5. **What Worked**
   - Got something working in weeks, not months
   - MCP integration worked beautifully
   - Local-first approach validated (no token costs!)
   - Keyword + symbol search were fast and useful

6. **What Didn't Work**
   - Line-based chunking split functions awkwardly
   - Semantic search quality was inconsistent
   - No incremental updates (full re-index every time)
   - Single-repo limitation

7. **Key Learnings**
   - Ship fast, learn fast
   - Line-based chunking is naive but gets you started
   - Local embeddings are viable for code search
   - Code has structure that text-chunking ignores

8. **What's Next**
   - Scale to larger codebases (PostgreSQL)
   - Better embedding models
   - Multi-repo support
   - Incremental indexing

**Tone:** Honest, pragmatic, "here's what I built and learned"
**Length:** 1000-1200 words
**Tags:** `llm-tooling`, `mcp`, `code-search`, `mvp`

---

## Blog Post 2: codetect v1 - When Better Models Aren't Enough

**Published:** January 2026 (retroactive)
**Target audience:** Developers who read v0, interested in scaling challenges

### Structure

1. **Hook** - Scaling v0 hit a wall
   - Larger codebases = slower everything
   - Better models didn't improve results as expected
   - Why?

2. **What We Added in v1**
   - PostgreSQL + pgvector for scale
   - HNSW indexing (60x faster search at 10K+ vectors)
   - Better embedding models (bge-m3, 1024 dimensions)
   - Multi-repo database architecture (early attempts)
   - Eval framework to measure quality

3. **Performance Wins**
   - 60x faster semantic search with HNSW
   - Handles 10K+ file codebases
   - Better model support (bge-m3, mxbai-embed-large)

4. **The Surprise**
   - Better models didn't improve retrieval quality much
   - Still ~65% accuracy (up from 60% in v0)
   - Why? **Chunking was still the bottleneck**

5. **What the Eval Framework Revealed**
   - Functions split across chunks (40% of search results incomplete)
   - Context lost at chunk boundaries
   - Model quality couldn't compensate for structural problems
   - The insight: **We were treating code like text**

6. **The Realization**
   - Line-based chunking fundamentally doesn't respect code structure
   - No amount of model sophistication fixes bad chunks
   - Need to chunk by semantic units (functions, classes)
   - This realization set the stage for v2

7. **What We Learned**
   - Measure what matters (eval framework was critical)
   - Better models ≠ better results if input quality is bad
   - Scale and performance are different problems than quality
   - Structure matters more than sophistication

8. **Teaser for v2**
   - Next: AST-based chunking
   - Parse code before chunking it
   - Chunk by functions, not lines

**Tone:** Reflective, data-driven, "we thought X but data showed Y"
**Length:** 1000-1200 words
**Tags:** `llm-tooling`, `mcp`, `scaling`, `embeddings`

---

## Update to v2 Post

**Changes to make:**

1. **Abbreviate origin story** (replace full version with 2-3 paragraphs)
   - Brief mention of token costs and performance gap
   - Link to v0 post: "I wrote about the origin story in my v0 post"
   - Focus more on the technical evolution

2. **Add series context**
   - Add callout box at top linking to v0 and v1
   - "This is part 3 of a series on building codetect"

3. **Reference v1 learnings**
   - When discussing "better models don't fix bad chunks"
   - Link to v1 post for full context

**Example callout box:**

```html
<div style="background: rgba(66, 175, 250, 0.1); border-left: 4px solid #42affa; padding: 1.5rem; margin: 2rem 0; border-radius: 4px;">
    <p style="margin: 0 0 0.5rem 0; font-weight: 600;">This is part 3 of the codetect series:</p>
    <ul style="margin: 0; padding-left: 1.5rem;">
        <li><a href="/blog/codetect-v0/" style="color: #42affa;">Part 1: Building an MCP Code Search Tool (v0)</a></li>
        <li><a href="/blog/codetect-v1/" style="color: #42affa;">Part 2: When Better Models Aren't Enough (v1)</a></li>
        <li><strong>Part 3: From Line Chunks to AST-Based Understanding (v2)</strong> ← You are here</li>
    </ul>
</div>
```

---

## Blog Index Updates

Update `blog/index.html` to show all three posts:
- v2 (Feb 1, 2026) - featured
- v1 (Jan 2026) - regular card
- v0 (Nov 2025) - regular card

---

## Series Benefits

1. **Better storytelling:** Evolution narrative with depth
2. **SEO:** Three posts instead of one (more entry points)
3. **Reader engagement:** Multi-post series encourages returning
4. **Reusability:** Can link to specific topics (origin story, scaling, AST)
5. **Community building:** Series format invites discussion/feedback

---

## Timeline

**Retroactive dating:**
- v0 post: November 15, 2025 (mid-month)
- v1 post: January 6, 2026 (right after New Year)
- v2 post: February 1, 2026 (already published)

This creates realistic spacing: ~7 weeks between v0→v1, ~4 weeks between v1→v2

---

## Meta

**SEO Strategy:**
- v0: "Building an MCP Code Search Tool from Scratch"
- v1: "Why Better Embedding Models Didn't Improve Code Search Quality"
- v2: "How AST-Based Chunking Made Code Search 15x Faster" (current)

**Cross-linking:**
- Each post links to previous/next in series
- v2 references v0 origin story and v1 learnings
- Creates cohesive narrative arc
