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
pdflatex -shell-escape article.latex
```

```latex
\usepackage{minted} 
\usepackage{mdframed}
\definecolor{bg}{rgb}{0.95,0.95,0.95}

\begin{mdframed}[backgroundcolor=bg]
\begin{minted}{python}
from sklearn.cluster import KMeans

X=df[['sepallength', 'sepalwidth', 'petallength', 'petalwidth']]
from sklearn.preprocessing import LabelEncoder
lbl = LabelEncoder()
y=lbl.fit_transform(df["class"])

model = KMeans(n_clusters=3)
model.fit(X)
\end{minted}
\end{mdframed}
```

## Multicolumn

```latex
\usepackage{multicol}

\begin{multicols}{2}
\end{multicols}
```
