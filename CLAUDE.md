# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Build System

This is a LaTeX Beamer theme project. It **must** be compiled with `xelatex` because the theme depends on `ctex` for Chinese typography support.

- **Recommended**: `latexmk njubeamer-demo.tex` — `.latexmkrc` is pre-configured with `$pdf_mode = 5` (xelatex).
- **Manual**: Run `xelatex` at least twice to resolve cross-references and generate the table of contents:
  ```bash
  xelatex njubeamer-demo.tex
  xelatex njubeamer-demo.tex
  ```
- **Clean build artifacts**: `latexmk -c njubeamer-demo.tex`

## Architecture

### Single-file Theme

Unlike standard Beamer themes that split into `beamercolortheme`, `beameroutertheme`, and `beamerinnertheme` files, this project inlines everything into a single `njubeamer.sty`. It is loaded with `\usepackage{njubeamer}`, **not** `\usetheme{njubeamer}`.

### TikZ Overlay Rendering

All custom page elements (title page, section page, table of contents background, headline logo, frametitle, footline) are drawn with TikZ `[remember picture, overlay]`. This means:

- Positions are calculated relative to `current page` anchors (e.g., `current page.north west`).
- Elements do not consume standard Beamer box space; they are painted on top of the canvas.
- The frametitle template uses `\vbox to 1.5cm` to reserve vertical space, while the actual title text and underline are placed via TikZ nodes at absolute coordinates.

### Key Customization Points

**Color palette** (`njubeamer.sty`, top of file):
- `NJUPurple` (`RGB{92, 0, 92}`) is the primary brand color. Changing this value propagates to all titles, blocks, list markers, and decorative bars.

**Frametitle sizing** (`njubeamer.sty`, near bottom):
- `\nju@frametitlesize` controls the frame title font size (default `\fontsize{16pt}{20pt}`). The title is vertically centered inside the 1.5cm top banner by TikZ coordinate math (`-0.8cm` center line).

**Auto section pages**:
- `\AtBeginSection[]{\begin{frame}[plain] \sectionpage \end{frame}}` automatically inserts a section title frame before each `\section`. To disable this for a specific section, use `\section*{...}`.

**TOC background injection**:
- `\pretocmd{\tableofcontents}{...}` prepends a TikZ overlay that draws the left purple sidebar and top logo before Beamer renders the actual toc entries. This is why `\tableofcontents` must be called inside a `\begin{frame}[plain]` environment.

### Assets

Logo files live in `assets/logos/` and are referenced with relative paths (`assets/logos/nju-emblem-purple.pdf`). There are two variants of each logo:
- Black (`nju-emblem.pdf`, `nju-name.pdf`) — used on the title page white banner.
- Purple (`nju-emblem-purple.pdf`, `nju-name-purple.pdf`) — used on body pages, section pages, and toc pages.

When modifying logo placement, note that emblem (0.9cm) and name (0.7cm) are vertically center-aligned via manual `\raisebox` offsets.
