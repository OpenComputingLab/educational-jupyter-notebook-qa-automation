# Notebook Cleaning

One of the powerful features of the `.ipynb` notebook format is it's ability to serialise rich code cell outputs as part of the document format, as well as storing other artefacts of cell execution, such as the cell execution count number.

When releasing notebooks, or committing notebooks to a code repository, there may be a requirement to clear notebooks of cell outputs, and potentially other elements.

Various tools exist that can be used to clean notebooks, including:

- [`nbconver`](https://nbconvert.readthedocs.io/en/latest/index.html) : the official `nbconvert` tool can be used to convert notebook documents to other document types. The tool can also be run with a cell pre-processor to rewrite a notebook with cell outputs cleared: `jupyter nbconvert NOTEBOOK.ipynb --to notebook --ClearOutputPreprocessor.enabled=True --output NOTEBOOK`
- [`choldgraf/nbclean`](https://github.com/choldgraf/nbclean): a general command line utility for cleaning notebooks, including clearing cell outputs , removing cells based on tags and cell content and removing text within a cell between delimited start and end strings.
- [`srstevenson/nb-clean`](https://github.com/srstevenson/nb-clean): a command line utility, and Github filters, for removing cell execution counts, cell outputs, and (optionally) empty cells and cell metadata;
- [`innovationOUtside/nb_workflow_tools`](https://github.com/innovationOUtside/nb_workflow_tools): various command-line utilities for working with notebooks, including the ability to create compressed (zipped) archive files with automatically cleaned notebooks.
