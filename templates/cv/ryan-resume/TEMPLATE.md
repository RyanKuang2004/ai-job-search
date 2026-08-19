# Template: ryan-resume

- **Type:** CV
- **Source extension:** .tex
- **Engine/toolchain:** lualatex
- **Page limit:** 2 page(s)
- **Fonts:** Calibri (system font — must be installed; present on Windows by default at `C:\Windows\Fonts`)
- **Class/packages:** `article` base class with `fontspec`, `geometry`, `xcolor`, `enumitem`, `hyperref`, `tabularx`, `parskip` (all standard, ships with MiKTeX)

## Compile command

    cd <output dir> && lualatex -interaction=nonstopmode <file>.tex

**Note:** on this machine, bare `lualatex` on PATH resolves to TinyTeX, which lacks several packages this template needs (`enumitem`, etc.). Invoke MiKTeX's binary explicitly instead: `"C:/Users/ryank/AppData/Local/Programs/MiKTeX/miktex/bin/x64/lualatex.exe" -interaction=nonstopmode <file>.tex`. See the repo's `latex-toolchain-miktex-not-tinytex` guidance.

## Style rules

- Single-column layout, no sidebar, no photo/logo.
- Section headings: bold, ALL CAPS, full-width light-blue background band (`#D2EAF1`), produced via the `\cvsection{}` macro — do not reformat headings by hand.
- Entry rows (education/experience/project title lines): bold title on the left, date range flush right on the same line, via the `\cventry{title}{dates}` macro (uses `tabularx`).
- Bullets are tight (`enumitem` spacing: minimal itemsep/parsep) — keep bullet lists dense, no blank lines between items.
- Body text uses no color (`headingfill` background is the only color used); keep links black (`hyperref` colorlinks set to black) to match the original plain, professional look.
- Section order in the original: Education, Relevant Experience, Core Skills, Personal Statement, Awards and Scholarships, Projects. Preserve this order unless the job posting strongly justifies reordering.
- Margins ~0.68in on all sides (matches the source docx's 975-twip margins).

## Known pitfalls

- `\cvsection{}` uses `\colorbox` inside a `\parbox` sized to `\textwidth - 2\fboxsep` — if `\textwidth` changes (e.g. nested in a minipage), the band width must be recalculated or it will overflow the margin.
- Calibri is a proprietary Microsoft font. It renders correctly on this machine because Windows ships it by default, but the compiled PDF is not portable to a machine without Calibri installed — `fontspec` will fail to find the font. If compiling in a different environment, either install Calibri or substitute the metric-compatible free alternative Carlito (`\setmainfont{Carlito}`, available as a MiKTeX package) with no layout changes.
- `tabularx` `X` column combined with `\textbf{}` inside `\cventry{}` — very long job titles will wrap; keep titles under ~45 characters to stay on one line next to the date.
- Bullet and skill text must escape LaTeX special characters: `%`, `#`, `&`, `_`, `$` need `\%`, `\#`, `\&`, `\_`, `\$` (e.g. "C#" → `C\#`, "reduced by 20%" → `reduced by 20\%`). Confirmed by the test compile — an unescaped `%`/`#`/`&` breaks the build with cryptic "misplaced alignment tab" or "macro parameter character" errors.
