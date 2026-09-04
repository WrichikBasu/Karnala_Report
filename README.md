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

## Layout

| Path | Purpose |
| --- | --- |
| `karnala_report.tex` | The entire document. Single file, no `\input` splitting. |
| `misc-macros.sty` | Page geometry, headers/footers, date format, `\bfnt`/`\ifnt`. |
| `mathsettings.sty` | Math and table packages, `hyperref` setup, column types. |
| `*.jpg`, `*.jpeg`, `*.png` | Figures, referenced by bare filename from the project root. |

Both `.sty` files are personal, reused across projects; treat changes to them as affecting
other documents too. `mathsettings.sty` is pulled in by `misc-macros.sty`, so the main file
only loads the latter.

Note that `misc-macros.sty` calls `\newgeometry` (1.5 cm margins, 2 cm at the bottom) as a
side effect of being loaded.

## Conventions used in the source

- `\ifnt{...}` for italics — used for all binomial names, e.g. `\ifnt{Gnetum}`.
- `\ifnt{...}` and `\bfnt{...}` rather than `\emph`/`\textbf`; both are defined in
  `mathsettings.sty`.
- `\bfnt{Label:}` opens each item in a description-style `itemize`.
- Hyphenation is **disabled document-wide** — `misc-macros.sty` notes that
  `\usepackage[none]{hyphenat}` must be loaded by the main file, and it is. Expect overfull
  boxes on long words; fix them by rewording, or with an explicit `\hspace{0pt}` break as
  used in `semi-\hspace{0pt}evergreen`.
- Captions are set with `\captionof{figure}{...}` inside a `center` environment rather than
  with floating `figure` environments, so figures stay exactly where they are placed.
- Where a caption needs a short form for the list of figures, it is given as
  `\captionof{figure}[short]{long}`.
- The topographic-map figure uses a `capfn` counter to place real footnotes inside a
  caption, which `\footnotemark`/`\footnotetext` cannot do unaided. If you add footnotes
  before it, that block still numbers correctly, but check the output.

## Document structure

1. **Itinerary** — timetable of the day.
2. **Karnala Bird Sanctuary** — location and topography, objectives of the visit,
   observations of flora and abiotic features, and why no birds were sighted.
3. **Yusuf Meherally Center** — history, village industries, and development philosophy.
4. **Conclusion**

## Known issues

- `trail12.jpg` is referenced by the source but is **not tracked by git**, along with six
  other photographs in the working tree. A fresh clone will not build until it is added.
- The repository stores full-resolution photographs directly; `.git` is about 1.3 GB.
  Downsampling the images, or moving them to Git LFS, would make this far more manageable.
- Generated PDFs are ignored by `.gitignore`, as are the usual LaTeX auxiliary files.
