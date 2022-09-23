# Template / Example

Look at an example by downloading [this](https://drive.google.com/file/d/19nIBBuFWWd3YY6WygNx6L820paZ6cSjX/view?usp=sharing)

## Subfiles, preprint vs journal templates

First focus on `main.tex`. It contains the main template.

The actual text is itself contained in other files, intro.tex, model.tex, etc. which are called by main.tex using `\input{}`. It’s a good idea to divide up the paper that way so that different people can edit independently different parts of the paper. Here even figure calls have their own tex files.

But the other reason for doing this division is this: main.tex is formatted with RevTex, which we use for preprint posting. For actual journal submission, we’ll need to change the formatting to the particular journal. In this example, that’s done in main_nar.tex (for Nucleic Acids Research). That file contains a different template, but uses the same text intro.tex etc through \input{}. That way we can edit the text once, and easily re-create the preprint and a ready-to-submit version. And also easily change journals once we’re rejected (almost never happens!).

Another advantage: in the preprint version we merge main text and SI, by calling \input{si} in main.tex. But journals don’t want that. Instead, they want a separate pdf for the SI. With this system we do this by creating a third template nar_si.tex, which calls si.tex and only that.

## Bibliography

Bibliography is done with `bibtex`. We can include multiple bib files together using `\bibliography`. We use the PNAS style for references, which is the cleanest, for which pnas.bst is needed.

You can use a bibliography software to automatically generate your bib file (e.g. [Zotero](https://www.zotero.org/)).

## Figures

They should all be `PDF`, unless it’s really not possible otherwise (or too big). I prefer if you keep all figures in the root directory, because most journals don’t deal with subdirectories. If you really really want them in a ‘Figures/’ subdirectory, use \graphicspath{{Figures/}} right after calling the include{graphicx, graphics} packages.

All figures should be standalone. One figure = one file. If you have subfigures, merge them together into a single pdf.

## Packages

Packages: don’t add fancy formatting latex packages unless you really need to, such as subfigure, xr, hyperref, etc. Many of those screw up the journal packages. Keep them to a minimum.

## Scripts

We have an amazing script, ltxclean.pl, which creates a single, standalone tex file that merges together all the subfiles called by main (or main_nar in our case), as well as the formatted bibliography. We’ll use that to submit to arxiv and to the journal (along with the pdf of the figures, and that’s it! No need for auxiliary files). Only to be used at then end, at submission: 
```
./ltxclean.pl main.tex > main_clean.tex
```
**NEVER EDIT THE CLEAN VERSION**.

There’s another script, copy_figures.sh, which finds figure called in the whole document (including subfigures), and copy the corresponding pdf files into a directory of choice. Useful for submitting to arXiv. Use with e.g. ./copy_figures.sh main.tex arXiv/

