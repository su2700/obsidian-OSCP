# Gemini Context: obsidian-OSCP

This project is an **Obsidian Vault** dedicated to OSCP (Offensive Security Certified Professional) study notes, lab writeups, and cybersecurity research. It follows the "LLM Wiki" pattern for building a persistent, compounding knowledge base.

## Project Overview

- **Type:** Knowledge Base / Obsidian Vault (Non-Code) with embedded Python/Bash utilities.
- **Goal:** To maintain a structured, interlinked collection of markdown files for cybersecurity learning, utilizing LLMs to assist in ingestion, synthesis, and maintenance.
- **Core Philosophy:** Refer to `llm-wiki.md` for the "LLM Wiki" architecture (Raw sources -> Wiki -> Schema).

## Directory Structure

- `OSCP-Obs/`: The primary Obsidian vault directory.
    - `Notion/`: Hierarchical notes imported from Notion (e.g., `Common Tools`, `HTB`, `OSCP`, `Service and Protocols`).
    - `Inbox/`: A landing zone for new, unsorted notes and snippets.
    - `Attachments/`: Centralized folder for images and binary attachments.
    - `Canvas/`: Obsidian Canvas files for visual mapping.
    - `writeup-converter/`: A Python/Bash utility suite for exporting Obsidian notes to external formats or Jekyll-based websites.
- `llm-wiki.md`: Foundational document describing the strategy for maintaining this vault with LLM assistance.

## Key Utilities

### Writeup Converter (`OSCP-Obs/writeup-converter/`)
A tool to copy markdown files and their associated attachments to a target directory, optionally formatting them for a website.

**Basic Usage:**
```bash
python3 OSCP-Obs/writeup-converter/writeup_converter.py \
  <source_folder> \
  <source_attachments_path> \
  <target_folder> \
  <target_attachments_path>
```

**Features:**
- Automatically identifies and copies linked images/attachments.
- Website formatting (`-w`) for Jekyll-friendly output.
- Prefix removal/addition for attachment paths.

## LLM Wiki Operations (from `llm-wiki.md`)

When interacting with this vault via an LLM, aim for these workflows:

1.  **Ingest:** Process new source documents (walkthroughs, reports) by extracting key info and integrating it into existing pages.
2.  **Synthesis:** Create entity pages (for tools, techniques, or machines) that synthesize info from multiple sources.
3.  **Cross-Linking:** Maintain a rich web of internal links between related concepts (e.g., linking a tool to the protocols it exploits).
4.  **Logging:** Track major updates in a `log.md` (to be created) to maintain a timeline of knowledge accumulation.

## Development & Maintenance

- **Obsidian:** Use the Obsidian desktop/mobile app to visualize the graph and edit notes manually.
- **Python:** The `writeup-converter` requires Python 3.8+.
- **Git:** The entire vault is version-controlled. Commit meaningful changes to track the evolution of your knowledge.
