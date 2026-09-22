# William Lay's CV

A two-page LaTeX CV/résumé, based on a modified one-column version of the Deedy-Resume template. Built with XeLaTeX.

## Build Status

![Build PDF](https://github.com/laywill/CV/actions/workflows/build-pdf.yml/badge.svg)

## Compiled PDFs

The latest compiled PDFs are available at [github.com/laywill/CV/releases/latest](https://github.com/laywill/CV/releases/latest):
- `main.pdf` — general CV variant
- `main_senior_engineering_manager.pdf` — senior engineering manager-tailored variant
- `main_technical.pdf` — hands-on technical (Staff/Principal IC) variant

These are automatically generated on every push to `main` via CI.

## Variants

This repository maintains three CV variants:
- **`main.tex`** — the general CV variant
- **`main_senior_engineering_manager.tex`** — tailored for a senior engineering manager role
- **`main_technical.tex`** — tailored for hands-on Staff/Principal individual-contributor roles

All three variants share the same content in `body.tex`, with variant-specific sections toggled via the `\ifseniormgr` and `\iftechnical` conditionals, which are declared in the document class. See `CLAUDE.md` for full architecture and build details.
