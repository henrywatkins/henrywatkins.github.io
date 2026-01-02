# AI Coding Agent Instructions

## Project Overview

This is a Quarto-based academic website publishing physics research, a plasma physics book, and technical coding notes. The site deploys to GitHub Pages at henrywatkins.github.io. Content is authored in QMD (Quarto Markdown) and rendered to static HTML.

## Architecture

**Three Independent Quarto Projects:**
- `blog/` - Main website (type: website) with blog posts and code notes
- `book/` - Plasma physics book (type: book) with ordered chapters
- `monograph/` - Manuscript project (type: manuscript) for academic papers

**Output Strategy:**
- All projects render to subdirectories of `docs/` (configured via `output-dir` in each `_quarto.yml`)
- `blog/` → `docs/` (root)
- `book/` → `docs/book/`
- `monograph/` → `docs/monograph/`
- The `docs/` directory is served as GitHub Pages

**Navigation Integration:**
- The blog navbar links to rendered book/monograph HTML files (e.g., `../book/index.html`)
- Cross-links use relative paths between rendered outputs, not source files

## Key Workflows

### Rendering Content

Render a specific project:
```bash
cd blog && quarto render
cd book && quarto render
cd monograph && quarto render
```

Preview with live reload:
```bash
cd blog && quarto preview
```

**Critical:** Always render from within the project directory (blog/, book/, or monograph/). The `_quarto.yml` files use relative `output-dir` paths that assume you're in the project directory.

### Creating New Content

**Blog Posts:**
- Create directory in `blog/posts/<post-name>/`
- Add `index.qmd` with YAML frontmatter (title, description, author, date, categories, bibliography if needed)
- Add `references.bib` if citing papers
- Post metadata in `blog/posts/_metadata.yml` applies defaults: `freeze: true`, `title-block-banner: true`

**Code Notes:**
- Single QMD files in `blog/code_notes/`
- Examples: [python-notes.qmd](blog/code_notes/python-notes.qmd), [git-notes.qmd](blog/code_notes/git-notes.qmd)
- Include toc, license, and copyright in frontmatter

**Book Chapters:**
- Add QMD file to `book/`
- Update `book/_quarto.yml` chapters list in desired order
- Use `bibliography: references.bib` for citations

### Draft Management

Drafts live in `drafts/` directory (outside blog/, book/, monograph/). Set `draft: true` in frontmatter to exclude from listings. Move to `blog/posts/` when ready to publish.

## Content Conventions

### YAML Frontmatter

Required for blog posts:
```yaml
---
title: "Post Title"
description: "Brief description"
author: "Henry Watkins"
date: "MM/DD/YYYY"
draft: false
toc: true
license: "CC BY"
copyright: "Copyright © 2025 Henry Watkins. All Rights Reserved."
categories:
  - physics
  - computing
bibliography: references.bib  # if citing papers
---
```

### Mathematical Content

- Use LaTeX math with `$$` blocks for display equations
- Inline math with `$` 
- Heavy use of quantum field theory, plasma physics notation
- Example from [renormalization/index.qmd](blog/posts/renormalization/index.qmd): Lagrangians, path integrals, Feynman diagrams

### Citations

- BibTeX references in local `references.bib` files
- Cite with `@key` notation (e.g., `@zee2010quantum`)
- Book uses `book/references.bib`

### Images

- Store images alongside content in post directories
- Reference with relative paths: `image: tracks.jpg` in frontmatter

## Directory Structure Rules

- `docs/` is generated output - don't edit directly
- Each project has `.gitignore` excluding `/.quarto/` and `**/*.quarto_ipynb`
- Generated files like `index_files/` contain figure outputs from computational posts

## Project-Specific Patterns

### Physics Content Focus

This is a theoretical/computational physics site. Posts cover:
- Quantum field theory (renormalization, Higgs mechanism, path integrals)
- Plasma physics (MHD, transport, laser-plasma interactions)
- Statistical field theory
- Applied scientific computing workflows

### Code Notes Organization

Technical references organized by language/tool:
- [python-notes.qmd](blog/code_notes/python-notes.qmd) - Development process, libraries, testing
- [git-notes.qmd](blog/code_notes/git-notes.qmd) - Commands, DVC workflows
- [bash-notes.qmd](blog/code_notes/bash-notes.qmd), [sql-notes.qmd](blog/code_notes/sql-notes.qmd), etc.

**Pattern:** Command reference + workflow guidance + best practices

### Git Workflow

Based on [git-notes.qmd](blog/code_notes/git-notes.qmd) and commit history:
1. Create branches for new posts/features
2. Commit messages: short summary line, detailed explanation in body
3. Render locally before committing
4. Merge to main when complete
5. Typical commits: "new post", "completed [topic] post", "re-rendered"

## Common Tasks

### Adding a New Blog Post

```bash
mkdir -p blog/posts/new-topic
cd blog/posts/new-topic
# Create index.qmd with frontmatter
# Add references.bib if needed
cd ../../  # back to blog/
quarto render
git add posts/new-topic ../docs
git commit -m "new post on topic"
```

### Updating Book Content

```bash
cd book
# Edit existing chapter or create new QMD
# Update _quarto.yml chapters list if new chapter
quarto render
cd ..
git add book/ docs/book/
git commit -m "updated chapter on [topic]"
```

### Testing Changes Locally

```bash
cd blog  # or book, or monograph
quarto preview  # opens browser with live reload
```

## Dependencies

- Quarto CLI (primary build tool)
- Python (for computational content, code execution frozen by default)
- BibTeX (for citations)
- Git (version control)

## Things AI Agents Should NOT Do

- Don't edit files in `docs/` directly - always edit source QMD files and re-render
- Don't move rendered HTML files between directories - the output structure is controlled by `_quarto.yml`
- Don't run `quarto render` from repository root - always cd into the specific project directory
- Don't create posts directly in `blog/posts/` root - always use a subdirectory

## Quick Reference

- Main config files: `blog/_quarto.yml`, `book/_quarto.yml`, `monograph/_quarto.yml`
- Default post metadata: `blog/posts/_metadata.yml`
- Navbar structure: Defined in `blog/_quarto.yml` website section
- GitHub Pages source: `docs/` directory on main branch
