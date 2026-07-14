# William Lay's CV

A single-page LaTeX CV/résumé, based on a modified one-column version of the Deedy-Resume template. Built with XeLaTeX.

## Build Status

![Build PDF](https://github.com/laywill/CV/actions/workflows/build-pdf.yml/badge.svg)

## Compiled PDFs

The latest compiled PDFs are available at [github.com/laywill/CV/releases/latest](https://github.com/laywill/CV/releases/latest):
- `main.pdf` — general CV variant
- `main_senior_engineering_manager.pdf` — senior engineering manager-tailored variant

These are automatically generated on every push to `main` via CI.

## Variants

This repository maintains two CV variants:
- **`main.tex`** — the general CV variant
- **`main_senior_engineering_manager.tex`** — tailored for a senior engineering manager role

Both variants share the same content in `body.tex`, with variant-specific sections toggled via the `\ifseniormgr` conditional. See `CLAUDE.md` for full architecture and build details.
