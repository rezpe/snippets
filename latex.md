# Overview 

This page shows useful snippets for latex

## Items

```latex
\begin{itemize}
    \item 419 quasars
    \item 1731 spectral type A white dwarfs
    \item 154 spectral type B white dwarfs
\end{itemize}
```

## Figures

```latex
\begin{figure}[H]
    \centering
    \includegraphics[width=1\textwidth]{spectristar}
    \caption{Spectri of Stars}
    \label{figure:spectristar}
\end{figure}
```

## Code with syntax highlighting

It requires to build with the -shell-escape option

```bash 
pdflatex -shell-escape
```

```latex
\usepackage{minted}

\begin{minted}{python}
import numpy as np
a=b+1
\end{minted}
```
