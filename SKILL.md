---
name: "arxiv-paper-ingest"
title: "arXiv Paper Ingest — Read PDF → Obsidian Note → Zotero Import"
description: "Automatically ingest academic PDFs: extract text & images, generate structured markdown notes with embedded figures, import to Zotero with smart classification"
category: "academic"
triggers:
  - "ingest this paper"
  - "read this PDF and save to my knowledge base"
  - "process the latest paper on my desktop"
  - "arxiv paper ingest"
  - "help me read this paper"
---

# arXiv Paper Ingest

Automatically process an academic PDF (arXiv or other): extract full text and images, generate a structured Obsidian-style markdown note, and import the paper into Zotero with smart classification.

---

## 🔧 First-Time Setup

This skill needs to know your environment. On the **very first run**, you will be asked:

1. **Obsidian vault path** — e.g. `C:\Users\You\Documents\Obsidian\YourVault`
2. **Zotero MCP URL** — default `http://127.0.0.1:23120/mcp`
3. **Notes directory** — relative path inside vault, e.g. `Literature Notes/PDFs`
4. **Images directory** — relative path inside vault, e.g. `Literature Notes/PDF Figures`

These settings are **memorized** — you only configure them once.

---

## 📥 Input

- **Default**: Latest `.pdf` on your Desktop (`~/Desktop/*.pdf`)
- **Custom**: Any file path you specify

---

## 🔄 Workflow

### Step 1: Locate PDF

Find the most recently modified PDF on your Desktop (or use the user-provided path).

### Step 2: Extract Text & Images

Use PyMuPDF to extract:
- Full text from every page → for AI analysis
- All embedded images → saved to a shared images folder

### Step 3: AI Analysis → Classification → Note Generation

**Read** the full paper, then determine the best Zotero collection by examining your existing library structure. Generate a structured markdown note with:

- **Frontmatter**: Tags (reuse existing Zotero tags where possible), creation date
- **Author table**: All authors with affiliations, marked co-first authors (†) and corresponding author (\*)
- **Abstract**: Chinese (or your preferred language) summary
- **Sections**: Reorganized by topic, with embedded figures
- **Relevance**: Connection to your research

### Step 4: Save Images

All figures go into one **shared folder** with naming convention:

```
YYYYMMDD_PaperShortName_FigN_Description.png
```

Embedded in notes as relative paths:

```markdown
![Fig.1 Description](RelativePath/YYYYMMDD_Name_Fig1.png)
```

### Step 5: Write Obsidian Note

- Path: `{vault}/{notes_dir}/{YYYYMMDD Title}.md`
- Images: `{vault}/{images_dir}/`

### Step 6: Zotero Import

**6a**: Create Zotero item (journalArticle type, metadata populated)

**6b**: Add tags — fetch existing tags from the target collection, add ≤4 tags, prioritize reusing existing ones

**6c**: Import PDF as attachment (the file is copied to Zotero storage; the original is deleted afterward)

**6d**: Add to the classified collection

### Step 7: Report

Confirm completion with paths and summary.

---

## 🏷️ Tagging Rules

1. Fetch existing tags from the target Zotero collection
2. Pick **≤ 4** tags for the new paper
3. **Reuse existing tags** whenever semantically appropriate
4. Common tag patterns: `/unread` (new), `/reading` (in progress), `review`, material names (`LNOI`, `SiN`), technology names (`electro-optic`, `3D printing`)

---

## 📝 Note Template

```markdown
---
tags: [tag1, tag2, tag3]
created: YYYY-MM-DD
---

# YYYYMMDD Paper Title

**arXiv:XXXX.XXXXX** | Date

## Authors & Affiliations

| Author | Affiliation |
|--------|-------------|
| Name† (co-first) | Institution |
| **Corresponding\*** | Institution |

> **Corresponding**: Name — email
> **†Co-first authors**

---

## Abstract

...

## 1. Background

...

## 2. Results

![Description](images_dir/YYYYMMDD_Name_Fig1.png)

...

## N. Relevance to My Research

...
```

---

## 💻 Requirements

| Dependency | Notes |
|------------|-------|
| [Hermes Agent](https://hermes-agent.nousresearch.com) | Required runtime |
| Python 3.10+ with `pymupdf` | `pip install pymupdf` |
| [Zotero](https://www.zotero.org/) | Must be running |
| [Zotero MCP plugin](https://github.com/cookjohn/zotero-mcp) | v1.5.0+, write enabled |

---

## ⚠️ Known Issues

- **PDF path**: Zotero on Windows requires backslashes — use Python raw string `r'C:\Users\...'`
- **Copy vs Move**: Zotero's `import` copies the file; original must be deleted separately
- **Write permission**: Ensure `extensions.zotero.zotero-mcp-plugin.write.enabled=true` in Zotero prefs

---

## 📄 License

MIT
