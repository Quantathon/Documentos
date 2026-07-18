# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository purpose

This is the reference and strategy library for a team's entry to **Quantathon CR 2026, Challenge 1** ("Red Eléctrica Sostenible, Resiliente y Verde" — fault-zone partitioning of an electrical grid, modeled as Max-Cut → QUBO → QAOA, benchmarked against Goemans-Williamson). It is a document collection, not a software project — there is no source code, build system, package manifest, or test suite. The actual QAOA/QUBO implementation for the challenge does not live in this repo (yet).

## Structure

- `papers/` — original PDFs, flat directory, filenames are the paper's arXiv ID, DOI-derived identifier, or short citation key (e.g. `2504.06253v2.pdf`, `colucci-2023-power.pdf`).
- `markdown-papers/` — plain-text/Markdown conversions of the same PDFs, sorted into three topic subfolders:
  - `computacion-cuantica/` — quantum computing fundamentals and algorithms (QAOA, warm-starts, quantum optimization theory).
  - `contexto-red-electrica/` — power grid / electrical network context and applications.
  - `optimizacion/` — general (classical and quantum) optimization theory.
- `Propuesta/` — strategy documents (Spanish) reasoning about the official Challenge 1 brief and refining the team's proposal, meant to be read in order:
  - `01-analisis-challenge.md` — reverse-engineers the challenge brief and its scoring rubric; identifies where points actually come from (implementation 30%, comparison/scaling 20%, classical baseline 15%, reproducibility 10% with a cross-criterion penalty clause, SDG impact only 5%).
  - `02-comparacion-isla-verde.md` — audits an earlier team proposal ("ISLA VERDE") against the brief, flagging critical gaps (missing GW/greedy baseline, no Quantinuum H2 emulator run, no reproducibility plan, a warm-start framing that contradicts the brief's mandatory limitations section).
  - `03-propuesta-mejorada-isla-verde-2.md` — the corrected proposal: a layered architecture (mandatory canonical Max-Cut/QUBO/QAOA core → official-platform + warm-start layer → geospatial/constrained-partition extensions), with a day-by-day plan and a pre-written limitations section.
- `doc-*.docx` (top-level, e.g. `doc-1784337876281-78ecd714.docx`) — the official Challenge 1 brief exported from the organizers' platform. Treat as the authoritative source when it conflicts with the `Propuesta/` analysis (the analysis is a reading of it, not a replacement).

Each markdown file in `markdown-papers/` shares its basename with the source PDF in `papers/` (e.g. `papers/135393.pdf` ↔ `markdown-papers/computacion-cuantica/135393.md`), just relocated into the matching topic folder. When adding a new paper, add both the PDF (flat under `papers/`) and its Markdown conversion (under the appropriate `markdown-papers/<topic>/` subfolder), keeping the basenames identical.

## Working with the Markdown conversions

The `.md` files are raw PDF-to-text/Markdown extractions, not hand-authored notes. Expect OCR/extraction artifacts: headers split mid-sentence, numbered section titles separated from their text, inline citations and footnote markers interleaved with body text, and broken reading order around multi-column layouts or figures. Treat them as a searchable/full-text approximation of the source PDF, not a clean document — cross-check against the PDF in `papers/` when precision matters (e.g. quoting an equation or exact figure).

## Common tasks

Since there is no code, typical work here is: locating which paper(s) discuss a topic (grep across `markdown-papers/`), summarizing or comparing papers, adding newly collected papers in the PDF+Markdown pair described above, and iterating on the `Propuesta/` strategy documents as the team's plan evolves. When editing `Propuesta/`, keep claims traceable to the official brief (`doc-*.docx`) or to a cited paper in `papers/`/`markdown-papers/`, matching the existing documents' style of grounding every claim in a textual source.
