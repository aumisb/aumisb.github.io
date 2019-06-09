---
layout: post
title: "Visual Studio Code for LaTeX"
date: 2019-06-09
tags: [latex, tex, xelatex, code, linux]
---

This post covers my LaTeX Workshop extension settings in Visual Studio Code settings for compiling TeX documents. 

TeX files are compiled by calling the "Build LaTeX project" command (<kbd>CTRL</kbd>+<kbd>ALT</kbd>+<kbd>b</kbd>) from the command palette or from the TeX badge. LaTeX Workshop uses the concepts of *recipes*, which, in turn, calls a sequence of *tools*. Recipes are defined in `latex-workshop.latex.recipes` while tools are defined in `latex-workshop.latex.tools`. 

The recipe called by the "Build LaTeX project" command is defined by `latex-workshop.latex.recipe.default`, which can be either `first` or `lastUSed`.

I like to keep a project dependent `settings.json` file in the `.vscode` subdirectory of the root/project directory.  A complete `settings.json` file is shown below. Entries can be taken out as needed for the project.

```json
"latex-workshop.intellisense.package.enabled": "true"
"latex-workshop.latex.autoClean.run": "onFailed",
"latex-workshop.latex.recipe.default": "first",
"latex-workshop.latex.recipes": [
  {
    "name": "latexmk",
    "tools": [
      "latexmk"
    ]
  },
  {
    "name": "pdflatex ➞ bibtex ➞ pdflatex`×2",
    "tools": [
      "pdflatex",
      "bibtex",
      "pdflatex",
      "pdflatex"
    ]
  },
  {
    "name": "xelatex",
    "tools": [
      "xelatex"
    ]
  },  
  {
    "name": "texify",
    "tools": [
      "texify"
    ]
  }
],
"latex-workshop.latex.tools": [
  {
    "name": "latexmk",
    "command": "latexmk",
    "args": [
      "-shell-escape",
      "-synctex=1",
      "-interaction=nonstopmode",
      "-file-line-error",
      "-pdf",
      "-outdir=%OUTDIR%",
      "%DOC%"
    ],
    "env": {}
  },
  {
    "name": "pdflatex",
    "command": "pdflatex",
    "args": [
      "-synctex=1",
      "-interaction=nonstopmode",
      "-file-line-error",
      "%DOC%"
    ],
    "env": {}
  },
  {
    "name": "bibtex",
    "command": "bibtex",
    "args": [
      "%DOCFILE%"
    ],
    "env": {}
  },
  {
    "name": "xelatex",
    "command": "xelatex",
    "args": [
      "-synctex=1",
      "-interaction=nonstopmode",
      "-file-line-error",
      "%DOC%"
    ],
    "env": {}
  },
  {
    "name": "texify",
    "command": "texify",
    "args": [
        "--synctex",
        "--pdf",
        "--tex-option=\"-interaction=nonstopmode\"",
        "--tex-option=\"-file-line-error\"",
        "%DOC%.tex"
    ],
    "env": {}
   }  
]
```
 Variables available for usage in the `args` and `env` sections of a *tool* are:
    - `%DOC%`: The LaTeX root file path and name without the `.tex` extension
    - `%DOCFILE%`: The LaTeX root file name without the `.tex` extension
    - `%DIR%`: The LaTeX root file path
    - `%TMPDIR%`: A temporary folder for storing ancillary files
    - `%OUTDIR%`: The output directory configured in `latex-workshop.latex.outDir`

---

#### Notes
- The `env` setting can be used to modify `TEXMFHOME` to the current project directory if needed!

    ```json
    "env": {
        "TEXMFHOME": "%DIR%/texmf"
    }
    ```

- In some systems (in my case, Mac OS), it may not be possible to use <kbd>CTRL</kbd>+<kbd>ALT</kbd>+<kbd>b</kbd>. In that case, read [this](https://github.com/James-Yu/LaTeX-Workshop/wiki/FAQ#i-cannot-use-ctrlalt-in-a-shortcut). 