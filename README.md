# TransSeg-Thesis-Report
Thesis Report for TransSeg

# LaTeX Project Setup with VS Code

This guide explains how to set up and compile this LaTeX project across Windows, macOS, and Linux using VS Code and the LaTeX Workshop extension.

---

## 1. Install LaTeX distribution

You need a TeX distribution installed:

- **Windows**:  
  - [MiKTeX](https://miktex.org/download) (easy to install, auto-downloads missing packages)  
  - or [TeX Live](https://tug.org/texlive/windows.html) (larger but more stable)  

- **macOS**:  
  - [MacTeX](https://tug.org/mactex/) (full TeX Live distribution for macOS)  

- **Linux** (Debian/Ubuntu example):  
  ```bash
  sudo apt update
  sudo apt install texlive-full biber
  ```
  On other distributions, install `texlive-full` (or equivalent).

Make sure `pdflatex`, `latexmk`, and `biber` run in your terminal (`PATH` check).

---

## 2. Install VS Code & Extensions

1. Install [Visual Studio Code](https://code.visualstudio.com/).  
2. Install the extension: **LaTeX Workshop** (by James Yu).

---

## 3. Project Structure

This repo is organized as follows:

```
project/
 ├── main.tex              # main entry point
 ├── citations.bib         # bibliography file
 ├── cls/                  # custom class/style files
 │    └── iutbscthesis.cls
 ├── figs/                 # figures
 ├── aux/                  # auto-generated junk files (log, aux, bbl, etc.)
 └── main.pdf              # output PDF (compiled)
```

---

## 4. VS Code Settings

Open **Command Palette → Preferences: Open User Settings (JSON)** and add the following.  

Note: paths to `pdflatex` and `biber` differ slightly by OS.

### macOS (MacTeX)

```jsonc
"latex-workshop.latex.tools": [
  {
    "name": "latexmk",
    "command": "latexmk",
    "args": [
      "-pdf",
      "-interaction=nonstopmode",
      "-synctex=1",
      "%DOC%"
    ],
    "env": {
      "TEXINPUTS": "./cls//:"
    }
  },
  {
    "name": "pdflatex",
    "command": "/Library/TeX/texbin/pdflatex",
    "args": ["-interaction=nonstopmode", "-synctex=1", "%DOC%"]
  },
  {
    "name": "biber",
    "command": "/Library/TeX/texbin/biber",
    "args": ["%DOCFILE%"]
  }
],

"latex-workshop.latex.recipes": [
  {
    "name": "latexmk ➜ PDF (PDF in root, aux/ for junk)",
    "tools": ["latexmk"]
  },
  {
    "name": "pdfLaTeX -> Biber -> pdfLaTeX*2",
    "tools": ["pdflatex", "biber", "pdflatex", "pdflatex"]
  }
],

"latex-workshop.latex.autoBuild.run": "onSave",
"latex-workshop.view.pdf.viewer": "tab"
```

---

### Linux (TeX Live)

```jsonc
"latex-workshop.latex.tools": [
  {
    "name": "latexmk",
    "command": "latexmk",
    "args": [
      "-pdf",
      "-interaction=nonstopmode",
      "-synctex=1",
      "%DOC%"
    ],
    "env": {
      "TEXINPUTS": "./cls//:"
    }
  },
  {
    "name": "pdflatex",
    "command": "pdflatex",
    "args": ["-interaction=nonstopmode", "-synctex=1", "%DOC%"]
  },
  {
    "name": "biber",
    "command": "biber",
    "args": ["%DOCFILE%"]
  }
],

"latex-workshop.latex.recipes": [
  {
    "name": "latexmk ➜ PDF (PDF in root, aux/ for junk)",
    "tools": ["latexmk"]
  },
  {
    "name": "pdfLaTeX -> Biber -> pdfLaTeX*2",
    "tools": ["pdflatex", "biber", "pdflatex", "pdflatex"]
  }
],

"latex-workshop.latex.autoBuild.run": "onSave",
"latex-workshop.view.pdf.viewer": "tab"
```

---

### Windows (MiKTeX or TeX Live)

```jsonc
"latex-workshop.latex.tools": [
  {
    "name": "latexmk",
    "command": "latexmk.exe",
    "args": [
      "-pdf",
      "-interaction=nonstopmode",
      "-synctex=1",
      "%DOC%"
    ],
    "env": {
      "TEXINPUTS": "./cls//;"
    }
  },
  {
    "name": "pdflatex",
    "command": "pdflatex.exe",
    "args": ["-interaction=nonstopmode", "-synctex=1", "%DOC%"]
  },
  {
    "name": "biber",
    "command": "biber.exe",
    "args": ["%DOCFILE%"]
  }
],

"latex-workshop.latex.recipes": [
  {
    "name": "latexmk ➜ PDF (PDF in root, aux/ for junk)",
    "tools": ["latexmk"]
  },
  {
    "name": "pdfLaTeX -> Biber -> pdfLaTeX*2",
    "tools": ["pdflatex", "biber", "pdflatex", "pdflatex"]
  }
],

"latex-workshop.latex.autoBuild.run": "onSave",
"latex-workshop.view.pdf.viewer": "tab"
```

---

## 5. Keep Junk Files Separate

Create a `.latexmkrc` file in your project root:

```perl
# Keep intermediate files out of the root
$aux_dir = 'aux';

# Keep final outputs (PDF, synctex) in root
$out_dir = '.';

# Use biber for biblatex
$bibtex = 'biber';
```

This setup will produce:

```
main.pdf       (stays in project root)
aux/           (contains .aux, .log, .bbl, .bcf, .blg, .run.xml, etc.)
```

---

## 6. Building the Project

- **macOS / Linux**: Press `Cmd + Option + B` (macOS) or `Ctrl + Alt + B` (Linux).  
- **Windows**: Press `Ctrl + Alt + B`.  

By default, LaTeX Workshop runs the first recipe (`latexmk`).

---

## 7. Common Issues

- **Class not found (`iutbscthesis.cls`)**  
  Ensure the file is in `cls/` and `TEXINPUTS` is set in settings.  

- **Biber errors**  
  Ensure `biber` is installed and in `PATH`.  

- **PDF not refreshing**  
  Open the LaTeX Workshop sidebar → "View LaTeX PDF" → choose "Tab".  

---

## 8. Git Ignore

Add this to `.gitignore`:

```
aux/
*.synctex.gz
*.fdb_latexmk
*.fls
```

---

You are ready to edit `main.tex` and press the build shortcut (Cmd/Ctrl + Opt/Alt + B) to compile and view your thesis PDF.
