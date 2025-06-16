---
layout:     post
title:      LaTeX Troubles A Personal Record
subtitle:   Practical Notes on Debugging LaTeX and Related Scripts
date:       2025-06-12
author:     UPOJZSB
header-img: img/post-bg-TexStudio.png
catalog: true
tags:
    - Problem
    - LaTeX
    - XeLaTeX
---

# Prologue

As I am writing (or have written) my Ph.D. dissertation using XeLaTeX, and have also authored several scientific papers using pdfLaTeX, this blog serves as a personal record of the various problems I have encountered when using TeX-based typesetting tools — along with some scripts I created to assist the process.

# Scripts

Most of the following scripts (unless otherwise specified) are intended to be saved as `.bat` files

## Compile

This script uses `latexmk` to compile the dissertation. After each compilation, the user is expected to press any key and compile the `.tex` file again.

```cmd
@echo off
:: Turn off command echoing to keep the output clean

:loop
:: This is the start of the loop — it will repeat after each compilation

cls
:: Clear the screen for readability

echo Compiling LZhao.tex with SyncTeX enabled...

:: Compile the LaTeX document using latexmk
:: -xelatex    → Use XeLaTeX (a LaTeX engine with Unicode and font support)
:: -bibtex     → Run BibTeX to process the bibliography
:: -silent     → Suppress most output, including warnings and other verbose messages
:: -synctex=1  → Enable SyncTeX for editor/viewer synchronization (e.g., click in PDF → jump to .tex)
latexmk -xelatex -bibtex -synctex=1 texfile.tex


echo.
echo Press any key to recompile or close this window to exit...
pause >nul
:: Wait for the user to press any key before continuing (">nul" hides the default message)

goto loop
:: Go back to the start of the loop to allow recompilation

```

## Clear

This script clear most of the auxilary files which are generated when compiling a `.tex` file.

```cmd
@echo off
title LaTex Cleaner
del /a /f /q /s  "*.aux"
del /a /f /q /s  "*.bak"
del /a /f /q /s  "*.bbl"
del /a /f /q /s  "*.bcf"
del /a /f /q /s  "*.blg"
del /a /f /q /s  "*.dvi"
del /a /f /q /s  "*.fdb_latexmk"
del /a /f /q /s  "*.fls"
del /a /f /q /s  "*.glg"
del /a /f /q /s  "*.glo"
del /a /f /q /s  "*.gls"
del /a /f /q /s  "*.hd"
del /a /f /q /s  "*.idx"
del /a /f /q /s  "*.ilg"
del /a /f /q /s  "*.ind"
del /a /f /q /s  "*.ist"
del /a /f /q /s  "*.lof"
del /a /f /q /s  "*.log"
del /a /f /q /s  "*.lot"
del /a /f /q /s  "*.out"
del /a /f /q /s  "*.run.xml"
del /a /f /q /s  "*.sav"
del /a /f /q /s  "*.synctex.gz"
del /a /f /q /s  "*.synctex\(busy\)"
del /a /f /q /s  "*.thm"
del /a /f /q /s  "*.toc"
del /a /f /q /s  "*.slg"
del /a /f /q /s  "*.slo"
del /a /f /q /s  "*.sls"
del /a /f /q /s  "*.toe"
del /a /f /q /s  "*.xdy"
del /a /f /q /s  "*.listing"
```

# XeLaTeX

## xdvipdfmx

### xdvipdfmx:warning: Trying to include PDF file with version (1.7), which is newer than current output PDF setting (1.5).

This warning indicates that a PDF file being included (e.g., via `\includegraphics`) uses a newer PDF version (e.g., 1.7) than the version of the PDF that XeLaTeX is set to produce (e.g., 1.5).

To resolve this, either downgrade the input PDF version, or instruct `hyperref` to set the output PDF version to match (or exceed) the input file's version:

```latex
\usepackage[pdfversion=1.7]{hyperref}
```

### xdvipdfmx:warning: Can't begin an annotation when one is pending.

The full warning is:
``` cmd
[60
xdvipdfmx:warning: Can't begin an annotation when one is pending.
xdvipdfmx:warning: Interpreting special command bann (pdf:) failed.
xdvipdfmx:warning: >> at page="60" position="(228.335, 439.578)" (in PDF)
xdvipdfmx:warning: >> xxx "pdf:bann<</Type/Annot/Subtype/Link/Border[0 0 0]/H/I/C[1 0 0..."
xdvipdfmx:warning: >> Reading special command stopped around >><</Type/Annot/Subtype/Link/Border[0 0 0]/H/I/C[1 0 0]/A<</S/...<<
][61
xdvipdfmx:warning: Tried to end an annotation without starting one!
xdvipdfmx:warning: Interpreting special command eann (pdf:) failed.
xdvipdfmx:warning: >> at page="61" position="(85.656, 272.291)" (in PDF)
xdvipdfmx:warning: >> xxx "pdf:eann"
]
```
where **60** and **61** are pages that encountered the warning. The output figures are clickable and refer to some incorrect targets.

Here’s an example that caused the warning in my case:

```latex
\begin{figure}[p]
	\centering
	\includegraphics[width=0.8\textwidth]{./Figures/ccc.eps}
	\caption{
    xxx\ref{fig:aaa}xxx
	}
	\label{fig:ccc}
\end{figure}

\begin{figure}[p]
	\centering
	\includegraphics[width=0.8\textwidth]{./Figures/ddd.eps}
	\caption{
    xxx\ref{fig:bbb}xxx
	}
	\label{fig:ddd}
\end{figure}
```

This issue likely arises when `xdvipdfmx` tries to handle consecutive figure captions with `\ref` links and insufficient vertical spacing, which disrupts the internal annotation state machine.

The simples solution for this problem is to add vertical gap between two figures, like:
```latex
\begin{figure}[p]
	\centering
	\includegraphics[width=0.8\textwidth]{./Figures/ccc.eps}
	\caption{
    xxx\ref{fig:aaa}xxx
	}
	\label{fig:ccc}
\end{figure}

\vspace{1cm} 

\begin{figure}[p]
	\centering
	\includegraphics[width=0.8\textwidth]{./Figures/ddd.eps}
	\caption{
    xxx\ref{fig:bbb}xxx
	}
	\label{fig:ddd}
\end{figure}
```

Somethime, change the floating mode from `[htb!]` to `[htb]` may work, and users could control the place of figures via `\FloatBarrier` from package `\usepackage{placeins}`.

