README
======

If you want to create the HTML version of the documentation:

* `cd build`
* `make html`

If you want to create a PDF version of the documentation:

* `cd build`
* `make latex`
* `cd latex`
* Edit the Makefile, and make sure that "xelatex" is used instead of "pdflatex" or "latexmk". 
  Set: "PDFLATEX = xelatex"

* If you do not want to have all the documentation in one file (since the structure is optimised for web), edit docs/index.rst and comment out entries in the toctree as needed.
