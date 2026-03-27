# Academic writing framework

Framework to help write and format academic papers using Claude.

## Skills in this framework

| Skill | Purpose |
|-------|---------|
| [academic-writing](./academic-writing/) | Research design, IMRaD structure, literature reviews, peer review, grant proposals |
| [content-access](./content-access/) | Legal access to paywalled papers via Unpaywall, CORE, Semantic Scholar, ILL |
| [ai-writing-detox](./ai-writing-detox/) | Eliminate AI patterns that erode scholarly credibility |
| [quarto-publishing](./quarto-publishing/) | Render papers to PDF, Word (docx), HTML, or book format using Quarto |

## When to activate each skill

### Writing a paper
Use **academic-writing** for:
- Developing a research question (FINER criteria)
- Structuring a literature review (systematic search, Boolean operators)
- Writing IMRaD sections (intro, methods, results, discussion)
- Responding to peer review
- Drafting grant proposals (NSF/NIH style)

### Finding sources
Use **content-access** for:
- Finding free legal copies of paywalled papers (Unpaywall, CORE, Semantic Scholar)
- Checking open access status by DOI
- Requesting papers via interlibrary loan
- Contacting authors for preprints

### Polishing prose
Use **ai-writing-detox** when:
- Any draft text needs reviewing before submission
- Removing hedging, throat-clearing, and AI-pattern language
- Ensuring scholarly credibility by eliminating banned words and phrases

### Formatting and exporting
Use **quarto-publishing** for:
- Converting `.qmd` or `.md` files to PDF, Word, HTML, or book format
- Setting up citation management (BibTeX, CSL)
- Configuring journal-specific Quarto templates
- Publishing to GitHub Pages, Quarto Pub, or Netlify

## Workflow

```
Research question → Literature search → Draft (IMRaD) → Review prose → Export
      ↓                    ↓                 ↓               ↓            ↓
academic-writing    content-access    academic-writing  ai-writing-  quarto-
                                                         detox       publishing
```

## Citation styles supported

| Style | Use case |
|-------|----------|
| APA 7 | Psychology, social sciences |
| Chicago 17 | Humanities, history |
| MLA 9 | Literature, arts |
| IEEE | Engineering, computer science |
| Vancouver | Biomedical sciences |
| Harvard | Natural sciences |

## Output formats

| Format | Use case |
|--------|----------|
| PDF (LaTeX) | Journal submission, formal reports |
| PDF (Typst) | Fast render, modern layout |
| Word (docx) | Collaborative editing, tracked changes |
| HTML | Web publication, interactivity |
| HTML book | Multi-chapter dissertations, reports |
| EPUB | E-reader distribution |

## Source

Academic-writing, content-access, and ai-writing-detox skills are sourced from
[jamditis/claude-skills-journalism](https://github.com/jamditis/claude-skills-journalism),
a collection of Claude Code skills for journalism, media, and academia.
