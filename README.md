# academic-writing-framework

Framework to help you write and format academic papers using Claude.

## Skills

| Skill | Description | Source |
|-------|-------------|--------|
| [academic-writing](./academic-writing/) | Research design, literature reviews, IMRaD structure, peer review responses, grant proposals | [jamditis/claude-skills-journalism](https://github.com/jamditis/claude-skills-journalism/tree/main/academic-writing) |
| [content-access](./content-access/) | Legal access to paywalled papers via Unpaywall, CORE, Semantic Scholar, and interlibrary loan | [jamditis/claude-skills-journalism](https://github.com/jamditis/claude-skills-journalism/tree/main/content-access) |
| [ai-writing-detox](./ai-writing-detox/) | Eliminate AI-generated patterns that erode scholarly credibility | [jamditis/claude-skills-journalism](https://github.com/jamditis/claude-skills-journalism/tree/main/ai-writing-detox) |
| [quarto-publishing](./quarto-publishing/) | Render papers to PDF, Word (docx), HTML, or book format using Quarto | (new) |

## Usage

### Setting up with Claude Code

Clone this repository into your Claude skills directory:

```bash
git clone https://github.com/pchelle/academic-writing-framework.git ~/.claude/skills/academic-writing-framework
```

Or copy individual skills:

```bash
cp -r academic-writing ~/.claude/skills/
cp -r quarto-publishing ~/.claude/skills/
```

### Writing a paper

Use the **academic-writing** skill when:
- Developing a research question (FINER criteria)
- Structuring a literature review
- Writing IMRaD sections (introduction, methods, results, discussion)
- Responding to peer reviewers
- Drafting NSF/NIH grant proposals

### Finding sources

Use the **content-access** skill when:
- Looking for free legal copies of paywalled papers
- Checking open access status by DOI (Unpaywall API)
- Searching 295M open access papers (CORE API)
- Requesting papers via interlibrary loan

### Polishing prose

Use the **ai-writing-detox** skill when:
- Reviewing any draft before submission
- Removing AI-pattern language (banned words, throat-clearing, hedging)
- Ensuring scholarly credibility

### Formatting and exporting

Use the **quarto-publishing** skill when:
- Converting `.qmd` or `.md` files to PDF, Word, HTML, or book format
- Setting up citation management (BibTeX, CSL)
- Configuring journal-specific templates (Elsevier, PLOS, PNAS, etc.)
- Publishing to GitHub Pages, Quarto Pub, or Netlify

## Output formats supported

| Format | Use case |
|--------|----------|
| PDF (LaTeX) | Journal submission, formal reports |
| PDF (Typst) | Fast render, no LaTeX required |
| Word (docx) | Collaborative editing, tracked changes |
| HTML | Web publication, interactivity |
| HTML book | Multi-chapter dissertations, technical reports |
| EPUB | E-reader distribution |

## Citation styles

APA 7 · Chicago 17 · MLA 9 · IEEE · Vancouver · Harvard · and any CSL file from [zotero.org/styles](https://www.zotero.org/styles)

## Workflow

```
Research question → Literature search → Draft → Polish → Export
      ↓                    ↓               ↓         ↓        ↓
academic-writing    content-access   academic-  ai-     quarto-
                                     writing  writing-  publishing
                                             detox
```
