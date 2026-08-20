# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A two-page LaTeX CV/résumé for William Lay, based on a modified one-column version of the Deedy-Resume template. The repository is edited both locally and in Overleaf (commits like "Updates from Overleaf" sync the two), so keep changes Overleaf-compatible: no build scripts or non-standard toolchains that Overleaf can't run.

## Building

**Must compile with XeLaTeX** — the document uses `fontspec` with fonts vendored under `fonts/` and referenced by *relative* `Path`, so the build must run from the repository root.

```sh
latexmk -xelatex main.tex
```

`latexmkrc` only pins the timezone (`Europe/London`) so `\today` / `\lastupdated` are stable; it does not select the engine — pass `-xelatex` explicitly. If a font error appears, the working directory is wrong (not repo root) rather than the font being absent.

There are three entry points, representing tailored CV variants:
- `main.tex` — the general variant
- `main_senior_engineering_manager.tex` — tailored to senior engineering manager roles
- `main_technical.tex` — tailored to hands-on Staff/Principal individual-contributor roles

All three are thin wrappers: they carry the (identical) preamble, set every variant flag explicitly, and `\input{body}`. **All shared content lives in `body.tex`** — a single source of truth. The variants do not drift: edit `body.tex` once, and wrap any variant-specific difference in `\ifseniormgr` or `\iftechnical`. Keep the preamble in the three wrappers identical to each other. (Build any wrapper with XeLaTeX; they select their variant via the flags.)

**The flags themselves are declared in the document class**, not in the wrappers, so every entry point sees every flag and a conditional in `body.tex` is never an undefined control sequence. Adding a fourth variant is one `\newif\if<name>\<name>false` line in `deedy-resume-openfont-wjl.cls` plus a new wrapper that sets each flag.

The canonical compiled PDFs are now produced automatically via GitHub Actions CI (`.github/workflows/build-pdf.yml`) on every push to `main` and published to the rolling `latest` GitHub Release. Overleaf remains available for optional WYSIWYG spacing and layout tweaks, but is no longer required to obtain a compiled PDF.

## Architecture

- **`deedy-resume-openfont-wjl.cls`** — the custom document class. Declares the CV variant flags (`\ifseniormgr`, `\iftechnical`), and defines all colours, fonts (Lato / Raleway / Source Sans Pro, vendored under `fonts/`), and the semantic commands used throughout the content files:
  - `\namesection{first}{last}{contact}` — header block
  - `\runsubsection{...}` + `\descript{...}` + `\location{...}` — a job/role entry's title, descriptor line, and date/place line
  - `\section` / `\subsection` — styled section headers
  - `tightemize` — an `itemize` variant with reduced spacing (used for bullet lists)
  - `\sectionsep`, `\custombold`, `\lastupdated`
- **`body.tex`** — the assembly file. It `\input`s content fragments from `Content/` in order, shared by all three variants. **Sections and individual entries are toggled by commenting/uncommenting `\input` lines** — much of the content is intentionally commented out to keep the CV within two pages and tailored to a target role. This is the primary mechanism for changing what appears. Content that differs between variants (currently the sub-title/profile paragraph, the Skills-vs-Competencies-vs-Technical-Skills input, the Advent of Code entry, the open-source section, and the four blocks the technical variant drops for space) is wrapped in `\ifseniormgr` / `\iftechnical`. Three-way sites nest as `\ifseniormgr … \else\iftechnical … \else … \fi\fi`.
- **`main.tex` / `main_senior_engineering_manager.tex` / `main_technical.tex`** — thin wrappers (see "Building"); they set each variant flag and `\input{body}`. Don't put content here.
- **`Content/`** — one `.tex` fragment per CV entry, grouped by area (`Employment/`, `Education/`, `Personal_Projects/`, `Activities/`, etc.). Employment/project files are named by date range and role. Each fragment is a self-contained block using the class's commands; it assumes the class is already loaded via the entry-point wrapper.
- **`horizontal_rule.tex`** — reusable divider, `\input` between sections.
- **`\needspace{N\baselineskip}`** appears before entries to prevent a heading being orphaned at a page break — preserve these when reordering content, since fitting within two pages is a hard constraint (the "Known Issues" comment in `main.tex` still says one page; it is out of date).

## Linting

CI runs **MegaLinter** (`.github/workflows/mega-linter.yml`, config `.mega-linter.yml`) on push and PR. On non-`main` branches it auto-commits fixes back. Relevant behaviour:
- **cspell** spell-checks with dictionary **en-GB**. Add new proper nouns (company names, tools, acronyms) to the `words` list in `.cspell.json` — otherwise CI fails. Use British spelling in prose.
- **chktex** runs for LaTeX with warning 13 suppressed (`LATEX_CHKTEX_ARGUMENTS: --nowarn 13`).
- `fonts/` is excluded from all linting.
