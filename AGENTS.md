# AGENTS.md — ppgec-abntex2

## Project Overview

LaTeX dissertation/thesis template for **PPGEC** (Programa de Pos-Graduacao em Engenharia de Computacao) at **UPE** (Universidade de Pernambuco), built on **abnTeX2** — the standard Brazilian LaTeX class for ABNT-compliant academic documents.

- **License**: LPPL v1.3+
- **Primary language**: Brazilian Portuguese (all content, comments, filenames)
- **Document class**: `ppgec-abntex2.cls` (extends `abntex2`, which extends `memoir`)
- **Main file**: `ppgec-abntex2-modelo.tex`

## Directory Structure

```
ppgec-abntex2-modelo.tex   # Main document entry point
ppgec-abntex2.cls          # Custom class (cover, title page, label overrides)
referencias.bib            # BibTeX bibliography database
Makefile                   # Only has a `clean` target
pretextuais/               # Pre-textual elements (cover, abstracts, dedication, etc.)
conteudo/                  # Chapters (introducao, fundamentacao, desenvolvimento, etc.)
imagens/                   # Image assets (brasao.pdf, sample figures)
```

## Build Commands

### Full compilation (multi-pass, required for cross-references)

```bash
pdflatex ppgec-abntex2-modelo.tex
bibtex ppgec-abntex2-modelo.aux
makeindex ppgec-abntex2-modelo.idx
makeindex ppgec-abntex2-modelo.nlo -s nomencl.ist -o ppgec-abntex2-modelo.nls
pdflatex ppgec-abntex2-modelo.tex
pdflatex ppgec-abntex2-modelo.tex
```

### Using latexmk (recommended shortcut)

```bash
latexmk -pdf ppgec-abntex2-modelo.tex
```

### Quick single-pass (for syntax checking only, will have broken refs)

```bash
pdflatex -interaction=nonstopmode ppgec-abntex2-modelo.tex
```

### Clean auxiliary files

```bash
make clean
```

This removes: `*.out *.aux *.alg *.brf *.acr *.dvi *.gls *.log *.bbl *.blg *.ntn *.not *.lof *.lot *.toc *.loa *.lsg *.nlo *.nls *.ilg *.ind *.ist *.glg *.glo *.xdy *.acn *.idx *.loq *.synctex.gz *~`

### Prerequisites

- Full LaTeX distribution (TeX Live or MiKTeX)
- **abnTeX2** package installed (https://github.com/abntex/abntex2/wiki/Instalacao)

## Testing / Validation

There is no automated test suite. Validation is done by:

1. Compiling the full document (all passes) and checking for errors/warnings in the log
2. Visually inspecting the generated PDF for correct formatting
3. Checking `pdflatex` exit code (0 = success)

To check for LaTeX errors without full compilation:

```bash
pdflatex -interaction=nonstopmode -halt-on-error ppgec-abntex2-modelo.tex
```

## Code Style Guidelines

### File Encoding

- All `.tex`, `.cls`, and `.bib` files MUST be **UTF-8** encoded
- Declared via `\usepackage[utf8]{inputenc}` in the main document

### File Naming

- **Lowercase only**, no uppercase characters in filenames
- Hyphens (`-`) as separators for top-level files: `ppgec-abntex2-modelo.tex`
- Single Portuguese words for content files: `introducao.tex`, `conclusao.tex`
- Compound words joined without separators: `siglasesimbolos.tex`
- **No accented characters** in filenames (use `introducao` not `introdução`)
- Directory names: lowercase single Portuguese words (`conteudo/`, `pretextuais/`, `imagens/`)

### LaTeX Formatting

- **Indentation**: Tabs for indentation inside environments and command definitions
- **Section separators**: Use `% ---` comment blocks to delimit logical sections
- **Inline comments**: Use `%` with explanation after commands, e.g. `12pt, % tamanho da fonte`
- **Package imports**: Group by purpose with `% ---` separators and a brief comment per package
- **Blank lines**: One blank line between logical blocks; two blank lines before major sections
- **Line length**: No strict limit, but keep lines readable

### Comments

- All comments in **Brazilian Portuguese**
- Use `% ---` bars to visually separate sections within a file
- Use `%%` for file-level headers and license blocks
- Use `%` for inline explanations

### Labels and Cross-References

Labels use prefixes to indicate element type, though separator style is mixed:

| Prefix | Element | Example |
|--------|---------|---------|
| `cap_` | Chapter | `\label{cap_exemplos}` |
| `sec-` | Section | `\label{sec-divisoes}` |
| `tab-` | Table | `\label{tab-nivinv}` |
| `fig_` | Figure | `\label{fig_circulo}` |

When adding new labels, prefer underscore (`_`) as separator for consistency with the class conventions.

### BibTeX Citation Keys

- Author surname (lowercase) + year: `guizzardi2005`, `macedo2005`
- Standards: uppercase abbreviation + year: `NBR14724:2011`
- Tools/packages: lowercase name: `babel`, `memoir`
- Compound keys with hyphens: `abntex2-wiki-como-customizar`

### Document Settings (do not change without reason)

| Setting | Value | Location |
|---------|-------|----------|
| Font size | 12pt | `\documentclass` options |
| Paper | A4 | `\documentclass` options |
| Font | Times New Roman | `\usepackage{times}` |
| Citation style | Numeric ABNT, brackets | `\usepackage[num]{abntex2cite}` + `\citebrackets[]` |
| Paragraph indent | 1.3cm | `\setlength{\parindent}{1.3cm}` |
| Paragraph skip | 0.2cm | `\setlength{\parskip}{0.2cm}` |
| Primary language | brazil | Last in `\documentclass` babel options |
| Page numbering | Roman (pre-textual), Arabic (textual) | Custom `\pretextual`/`\textual` |

### Class File (`ppgec-abntex2.cls`)

- Extends `abntex2` via `\LoadClass{abntex2}`
- Overrides cover page (`\imprimircapa`) and title page (`\folhaderostocontent`)
- Renames list labels to Portuguese (e.g., "Indice de Figuras" instead of "Lista de Figuras")
- Uses bold chapter fonts (`\ABNTEXchapterfont` -> `\bfseries`)
- Uses `default` chapter style instead of abnTeX2's custom style

## Git Workflow

- **Branching model**: Git Flow
- **Main branch**: `main` (production)
- **Development branch**: `develop` (active development)
- **Feature branches**: `feature/<name>`
- **Bugfix branches**: `bugfix/<name>`
- **Release branches**: `release/<name>`
- **Hotfix branches**: `hotfix/<name>`
- **Commit messages**: English, conventional-commit style (e.g., `feat: Add ...`)

## Common Tasks for Agents

### Adding a new chapter

1. Create `conteudo/<chaptername>.tex` (lowercase, no accents)
2. Start with `\chapter{Title}\label{cap_<name>}`
3. Add `\include{conteudo/<chaptername>}` in `ppgec-abntex2-modelo.tex` in the textual section

### Adding a new package

1. Add `\usepackage{<package>}` in `ppgec-abntex2-modelo.tex` after existing package groups
2. Include a `%` comment explaining the package purpose

### Adding a bibliography entry

1. Add the entry in `referencias.bib` following existing BibTeX format
2. Use lowercase author surname + year as the citation key
3. Cite with `\cite{key}` or `\citeonline{key}` in text

### Modifying cover/title page formatting

Edit `ppgec-abntex2.cls` — specifically `\imprimircapa` or `\folhaderostocontent`

### Changing document metadata (title, author, advisor)

Edit `pretextuais/capa.tex`
