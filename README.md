# Among the Canopy

LaTeX source for *Among the Canopy — An Educational Visit to Karnala Bird Sanctuary and
Yusuf Meherally Center*, a field report for the Open Elective in Environmental Science,
SY BSc CS, SIES College.

Field visit: 22 August 2026. Roughly 38 pages, 31 figures.

## Building

Requires a TeX Live installation (developed against TeX Live 2026) with:

- **LuaLaTeX** — the document uses `fontspec`, so `pdflatex` will not work.
- **C059** — the URW Century Schoolbook clone, shipped in `fonts-urw-base35`
  (`texlive-fonts-recommended` on most distributions).
- **JetBrains Mono** — installed as a system font under the family name `Jet Brains Mono`
  (note the space, as written in `\setmonofont`).

Build with `latexmk`, which handles the reruns needed for the table of contents, the list
of figures and the `lastpage` references:

```sh
latexmk -lualatex karnala_report.tex
```

Or run LuaLaTeX directly, at least twice:

```sh
lualatex karnala_report.tex
lualatex karnala_report.tex
```

### Fast syntax check

The photographs are full-resolution and the finished PDF is around **1.3 GB**, so a normal
build is slow. To check that the source still compiles without embedding any images:

```sh
lualatex -draftmode -interaction=nonstopmode -halt-on-error karnala_report.tex
```

This finishes in seconds and still reports overfull boxes, undefined references and errors.

