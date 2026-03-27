---
name: quarto-publishing
description: Render and publish academic papers using Quarto. Use when converting manuscript content to PDF, Word (docx), HTML, or multi-chapter book format. Covers project setup, YAML frontmatter, citation management, journal templates, and publishing to GitHub Pages or Quarto Pub.
---

# Quarto publishing for academic papers

[Quarto](https://quarto.org) is an open-source scientific publishing system built on Pandoc.
It renders `.qmd` (Quarto Markdown) or plain `.md` files to multiple formats from a single source.

## Installation

```bash
# Download from https://quarto.org/docs/get-started/
# Verify installation
quarto check
```

## Output formats

| Format | Command flag | Use case |
|--------|-------------|----------|
| PDF (LaTeX) | `--to pdf` | Journal submission, formal reports |
| PDF (Typst) | `--to typst` | Fast render, modern layout |
| Word | `--to docx` | Collaborative editing, tracked changes |
| HTML | `--to html` | Web publication, interactivity |
| HTML book | `--to html` (book project) | Dissertations, multi-chapter reports |
| EPUB | `--to epub` | E-reader distribution |
| JATS | `--to jats` | XML for journal deposit |
| Reveal.js | `--to revealjs` | Presentation slides |

## Single document setup

### Minimal YAML frontmatter

```yaml
---
title: "Paper title"
author:
  - name: "First Author"
    affiliation: "Institution"
    email: author@institution.edu
date: last-modified
bibliography: references.bib
csl: apa.csl
format:
  pdf: default
  docx: default
  html: default
---
```

### Render a single file

```bash
# Render to all formats defined in frontmatter
quarto render paper.qmd

# Render to a specific format only
quarto render paper.qmd --to pdf
quarto render paper.qmd --to docx
quarto render paper.qmd --to html

# Live preview (auto-refreshes on save)
quarto preview paper.qmd
```

## Format-specific configuration

### PDF via LaTeX

```yaml
format:
  pdf:
    documentclass: article       # article, report, book
    classoption: [12pt, a4paper]
    geometry:
      - top=25mm
      - bottom=25mm
      - left=30mm
      - right=25mm
    linestretch: 1.5             # 1.5 for double-spaced text
    fontfamily: libertinus
    number-sections: true
    colorlinks: true
    toc: false
    keep-tex: false              # set true to inspect .tex
```

**LaTeX engine selection:**

```yaml
format:
  pdf:
    latex-engine: xelatex        # xelatex | pdflatex | lualatex
```

### PDF via Typst (faster, no LaTeX required)

```yaml
format:
  typst:
    papersize: a4
    margin:
      top: 25mm
      bottom: 25mm
      left: 30mm
      right: 25mm
    fontsize: 11pt
    columns: 1
    number-sections: true
    toc: false
```

```bash
quarto render paper.qmd --to typst
```

### Word (docx)

```yaml
format:
  docx:
    reference-doc: custom-reference.docx  # custom styles template
    number-sections: false
    toc: false
    highlight-style: github
```

**Creating a custom Word template:**

```bash
# Export default reference doc, then modify styles in Word
quarto pandoc -o custom-reference.docx --print-default-data-file reference.docx
```

### HTML (standalone)

```yaml
format:
  html:
    theme: cosmo          # cosmo | flatly | journal | litera | lux | materia | minty | pulse | sandstone | simplex | sketchy | slate | solar | spacelab | superhero | united | yeti
    toc: true
    toc-depth: 3
    toc-location: right
    number-sections: true
    code-fold: false
    self-contained: true  # bundle all resources into single HTML file
    embed-resources: true
```

## Multi-format output (publish all at once)

```yaml
---
title: "My paper"
author: "Author Name"
bibliography: references.bib
csl: apa.csl
format:
  pdf:
    toc: false
    number-sections: true
  docx:
    number-sections: false
  html:
    toc: true
    theme: cosmo
---
```

```bash
quarto render paper.qmd  # renders all three formats
```

## Quarto book project (dissertations, reports)

### Create a book project

```bash
quarto create project book my-dissertation
cd my-dissertation
```

### Book structure (`_quarto.yml`)

```yaml
project:
  type: book
  output-dir: _book

book:
  title: "Dissertation title"
  author: "Author Name"
  date: "2025"
  chapters:
    - index.qmd          # front matter / preface
    - intro.qmd
    - literature.qmd
    - methods.qmd
    - results.qmd
    - discussion.qmd
    - conclusion.qmd
    - references.qmd

bibliography: references.bib
csl: apa.csl

format:
  html:
    theme: cosmo
    toc: true
  pdf:
    documentclass: book
    classoption: [oneside, 12pt]
  docx:
    toc: true
    number-sections: true
```

### Render the book

```bash
# Render to all formats
quarto render

# Render to HTML only (fast preview)
quarto render --to html

# Live preview
quarto preview
```

### Book directory layout

```
my-dissertation/
├── _quarto.yml          # project configuration
├── references.bib       # BibTeX bibliography
├── apa.csl              # citation style
├── index.qmd            # preface / front matter
├── intro.qmd
├── literature.qmd
├── methods.qmd
├── results.qmd
├── discussion.qmd
├── conclusion.qmd
├── references.qmd       # {bibliography}
└── _book/               # rendered output (gitignore this)
    ├── index.html
    ├── my-dissertation.pdf
    └── my-dissertation.docx
```

## Citation management

### BibTeX integration

```markdown
---
bibliography: references.bib
csl: apa.csl
---

Citing in text: [@smith2020] or [see @smith2020, p. 23]

Multiple citations: [@smith2020; @jones2021]

Author in text: @smith2020 found that...

Suppress author: [-@smith2020]
```

### Getting CSL files

```bash
# Common styles — download from https://zotero.org/styles
# or install via the Zotero citation style repository

# Examples:
curl -O https://www.zotero.org/styles/apa
curl -O https://www.zotero.org/styles/chicago-author-date
curl -O https://www.zotero.org/styles/ieee
curl -O https://www.zotero.org/styles/vancouver
```

### Zotero → BibTeX workflow

```bash
# In Zotero: File > Export Library > BibTeX format
# Save as references.bib in your project root

# Or use Better BibTeX plugin for continuous sync:
# https://retorque.re/zotero-better-bibtex/
```

### Reference list placement

```markdown
## References

::: {#refs}
:::
```

Or use a `references.qmd` chapter in book projects.

## Journal-specific templates

Quarto has official extensions for many journals:

```bash
# Install a journal template (example: PLOS)
quarto use template quarto-journals/plos

# Other available templates:
# quarto-journals/acm
# quarto-journals/acs
# quarto-journals/agu
# quarto-journals/biophysical-journal
# quarto-journals/elsevier
# quarto-journals/jasa
# quarto-journals/jss
# quarto-journals/plos
# quarto-journals/pnas

# List all available journal formats
quarto use template --list
```

### Using a journal template

```yaml
format:
  elsevier-pdf:
    keep-tex: true
    journal:
      name: "Journal of Example Studies"
      formatting: preprint
      cite-style: authoryear
```

## Abstract, keywords, and author metadata

```yaml
---
title: "Full paper title"
subtitle: "Optional subtitle"
abstract: |
  This paper investigates... We find that... These results suggest...
  (150-300 words)
keywords:
  - keyword one
  - keyword two
  - keyword three
author:
  - name: "First Author"
    orcid: 0000-0000-0000-0000
    corresponding: true
    email: author@institution.edu
    affiliations:
      - name: University Name
        department: Department Name
        city: City
        country: Country
  - name: "Second Author"
    affiliations:
      - ref: univ1
affiliations:
  - id: univ1
    name: Partner University
---
```

## Tables and figures

### Cross-referencing

```markdown
See @tbl-results for descriptive statistics and @fig-scatter for the relationship.

| Variable | M | SD |
|----------|---|-----|
| Age | 34.2 | 12.1 |
| Score | 0.67 | 0.23 |

: Descriptive statistics {#tbl-results}

![Scatter plot of age and score](figures/scatter.png){#fig-scatter width=80%}
```

### Numbered equations

```markdown
The model is specified as:

$$
y_i = \alpha + \beta x_i + \varepsilon_i
$$ {#eq-model}

where $\varepsilon_i \sim N(0, \sigma^2)$.

See @eq-model for the full specification.
```

## Executable code (optional)

Quarto supports embedded R, Python, and Julia code cells that run on render:

````markdown
```{python}
#| label: fig-histogram
#| fig-cap: "Distribution of scores"
#| echo: false

import matplotlib.pyplot as plt
import numpy as np

data = np.random.normal(0.67, 0.23, 200)
plt.hist(data, bins=20)
plt.xlabel("Score")
plt.ylabel("Count")
plt.show()
```
````

Disable execution for pure writing projects:

```yaml
execute:
  eval: false   # don't run code
  echo: false   # hide code blocks in output
```

## Publishing

### GitHub Pages

```bash
# Publish HTML book to GitHub Pages
quarto publish gh-pages

# Or configure in _quarto.yml
```

```yaml
publish:
  gh-pages:
    branch: gh-pages
```

### Quarto Pub (free hosting)

```bash
quarto publish quarto-pub
```

### Netlify

```bash
quarto publish netlify
```

## Common issues and fixes

### LaTeX errors

```bash
# Install TinyTeX (minimal LaTeX distribution)
quarto install tinytex

# Install a missing package
quarto install tinytex --update
tlmgr install <package-name>

# Check what's missing from the log
quarto render paper.qmd --to pdf 2>&1 | grep "Error"
```

### Font issues in PDF

```yaml
format:
  pdf:
    mainfont: "Times New Roman"    # requires XeLaTeX or LuaLaTeX
    sansfont: "Arial"
    monofont: "Courier New"
    latex-engine: xelatex
```

### Long tables spanning pages

```markdown
::: {.landscape}
[Wide table here]
:::
```

### Word styles not applying

```bash
# Regenerate the reference doc from scratch
quarto pandoc -o reference.docx --print-default-data-file reference.docx
# Then open in Word, modify styles, save, reference in frontmatter
```

## Quick-start templates

### Single research article

```bash
# Create a new paper
quarto create project default my-paper
cd my-paper
```

Minimal `paper.qmd`:

```markdown
---
title: "Paper title"
author:
  - name: "Author Name"
    affiliation: "Institution"
bibliography: references.bib
csl: apa.csl
format:
  pdf:
    number-sections: true
  docx: default
  html:
    toc: true
---

## Introduction

[Introduction text]

## Methods

[Methods text]

## Results

[Results text]

## Discussion

[Discussion text]

## References

::: {#refs}
:::
```

```bash
quarto render paper.qmd --to pdf
quarto render paper.qmd --to docx
quarto render paper.qmd --to html
```

### Dissertation book

```bash
quarto create project book my-dissertation
cd my-dissertation
# Edit _quarto.yml to add chapters
quarto render --to html
quarto render --to pdf
```
