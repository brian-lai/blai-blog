# Summary: codetect Blog Series (v0, v1, v2)

**Date:** 2026-02-01
**Task:** Create complete codetect blog series with retroactive v0 and v1 posts

---

## What Was Accomplished

Successfully created a comprehensive 3-part blog series documenting the evolution of codetect from v0 → v1 → v2.

### Deliverables

1. **v0 Blog Post** (`blog/codetect-v0/index.html`)
   - Published: November 15, 2025 (retroactive)
   - Full origin story (token costs, Claude vs Cursor performance gap)
   - MVP approach with line-based chunking
   - What worked, what didn't, key learnings
   - ~1,200 words

2. **v1 Blog Post** (`blog/codetect-v1/index.html`)
   - Published: January 6, 2026 (retroactive)
   - Scaling with PostgreSQL + pgvector + HNSW
   - Better embedding models (bge-m3, 1024 dimensions)
   - Eval framework reveals chunking as bottleneck
   - Key insight: better models don't fix bad chunks
   - ~1,300 words

3. **Updated v2 Post** (`blog/codetect-v2-release/index.html`)
   - Added series navigation callout at top
   - Abbreviated origin story (links to v0 for full version)
   - Condensed v0/v1 evolution section (links to full posts)
   - More focused on AST breakthrough
   - Reduced from ~1,600 to ~1,400 words

4. **Updated Blog Index** (`blog/index.html`)
   - All three posts visible in chronological order
   - v2 featured, v1 and v0 as regular cards
   - Series context in descriptions

---

## Series Structure

**Part 1 (v0):** The Problem & MVP
- Why codetect exists (origin story)
- MVP approach (line-based chunking)
- What worked (MCP integration, local embeddings)
- What didn't (functions split across chunks)

**Part 2 (v1):** Scaling & The Revelation
- Performance improvements (PostgreSQL, HNSW, 60x faster)
- Better models (bge-m3) didn't improve quality much
- Eval framework reveals the real problem
- Insight: chunking strategy > model quality

**Part 3 (v2):** The Breakthrough
- AST-based chunking with tree-sitter
- 15x faster incremental indexing
- 85% retrieval accuracy (up from 60%)
- Multi-repo architecture
- Lessons learned from the journey

---

## Key Benefits of Series Format

1. **Better storytelling:** Each post has depth while maintaining narrative arc
2. **SEO:** Three posts = three entry points (instead of one long post)
3. **Reader engagement:** Multi-post series encourages returning readers
4. **Reusability:** Can link to specific topics (origin story → v0, scaling → v1, AST → v2)
5. **Modularity:** Readers can jump to the part most relevant to them

---

## Timeline (Retroactive Dating)

- **v0:** November 15, 2025 (7 weeks before v1)
- **v1:** January 6, 2026 (4 weeks before v2)
- **v2:** February 1, 2026 (today, already published)

Realistic spacing that reflects actual development timeline.

---

## Git Commits

1. `ec5026e` - Add plan for codetect v0/v1 blog series
2. `91fa11d` - Add codetect v0 blog post (Part 1)
3. `9949a2d` - Add codetect v1 blog post (Part 2)
4. `789e765` - Update v2 post with series navigation and abbreviated origin
5. `c140137` - Update blog index with complete codetect series

All pushed to `origin/main`.

---

## Files Created/Modified

**Created:**
- `blog/codetect-v0/index.html` (261 lines)
- `blog/codetect-v1/index.html` (332 lines)
- `context/plans/2026-02-01-codetect-series-v0-v1.md` (plan document)

**Modified:**
- `blog/codetect-v2-release/index.html` (abbreviated origin, added series nav)
- `blog/index.html` (added v0 and v1 post cards)
- `context/context.md` (todo tracking)

---

## Content Strategy

**v0 emphasis:**
- Origin story (token costs, performance problems)
- MCP-native positioning
- Local-first approach
- Ship fast, learn fast

**v1 emphasis:**
- Data-driven decision making (eval framework)
- Better infrastructure ≠ better quality
- The chunking bottleneck revelation
- Setting up v2

**v2 emphasis:**
- AST-based chunking breakthrough
- Performance numbers (15x, 3.3x)
- Multi-repo architecture
- Lessons from the journey

---

## Cross-Linking Strategy

Each post links to previous/next:
- v0 → "Next: Part 2 (v1)" at bottom
- v1 → "Part 1 (v0)" in callout, "Next: Part 3 (v2)" at bottom
- v2 → Links to v0 (origin story) and v1 (model limitations) in text

Series callout box at top of each post for easy navigation.

---

## Lessons Learned

1. **Series format is powerful:** Breaking one long post into three creates better narrative structure and reader experience.

2. **Retroactive dating works:** Dating v0 and v1 to their actual development timeline (Nov 2025, Jan 2026) creates authenticity.

3. **Linking enhances value:** Abbreviated origin in v2 + link to v0 keeps posts concise while preserving depth.

4. **SEO benefits:** Three posts with different angles (MVP, scaling, breakthrough) capture different search intents.

---

## Next Steps

- Share series on relevant communities (Reddit, HN, Twitter)
- Monitor which post in the series gets most traffic (entry point analysis)
- Consider similar series format for future technical projects
- Update codetect GitHub README to link to blog series

---

## Tools Used

- Read/Write/Edit for file operations
- Bash for git operations
- Followed PARA workflow: Plan → Execute → Summarize → Archive
