# LLM Wiki Schema

This document defines the conventions and workflows for maintaining the OSCP LLM Wiki.

## Directory Structure
- `sources/`: Immutable raw source documents (PDFs, Markdown clippings, images).
- `sources/assets/`: Images and attachments from sources.
- `wiki/`: LLM-maintained knowledge base.
- `wiki/index.md`: Content-oriented catalog.
- `wiki/log.md`: Chronological operation log.

## Workflows

### 1. Ingesting a Source
When a new file is added to `sources/`:
1. Read the source file.
2. Discuss key takeaways with the user.
3. Create a summary page in `wiki/` named `Source - [Title].md`.
4. Update `wiki/index.md` under the "Sources" category.
5. Identify existing or new Entity/Concept pages to update or create.
6. Link the source summary to relevant Entity/Concept pages.
7. Append an entry to `wiki/log.md` using the format: `## [YYYY-MM-DD] ingest | [Title]`.

### 2. Querying the Wiki
When asked a question:
1. Consult `wiki/index.md` to identify relevant pages.
2. Read the identified pages to synthesize an answer.
3. If the answer is complex or valuable for long-term reference, propose filing it as a new `wiki/` page.

### 3. Wiki Maintenance (Linting)
Periodically:
- Check for orphan pages (no inbound links).
- Identify contradictions between old and new sources.
- Propose new cross-links or concept consolidations.

## Formatting Conventions
- Use Obsidian-style internal links: `[[Page Name]]`.
- Every wiki page should have a "References" section linking back to the source(s).
- Use hierarchical tags (e.g., `#tool/scanner`, `#protocol/smb`) if needed for Dataview.
