# Jupyter Notebook Document Formats

The [Jupyter notebook file format](https://nbformat.readthedocs.io/en/latest/format_description.html) document format, `.ipynb`, is a text based Javascript Object Notation (JSON) document schema that describes:

- input cells (markdown, code)
- code output cells returning none or more of *stream*, *display-data* or *error* style outputs.

Cells may carry arbitrary metadata, as well as tag metadata. Cell tags may be used to modify the rendering of notebook in appropriately extended notebook editors. Tags may also be used to modify the rendering or behaviour of cell content as published using publishing frameworks such as Jupyter Book.

Notebooks may be authored in dedicated notebook editors, such as Jupyter Classic Notebook, RetroLab or JupyterLab notebooks, or in other editing environments. For example, the PyCharm IDE supports notebook editing, and the VS Code Jupyter extension allows `.ipynb` format notebooks to be edited in a VS Code environment.

```{note}
See [*Open Jupyter Authoring and Learning Environment,(OpenJALE)*](https://opencomputinglab.github.io/OpenJALE/) for more discussion of Jupyter notebook authoring environments.
```

Notebooks may also be represented using various simpler text formats inspired by markdown or simple code file formats. These formats may have an important role to play in an automation pipeline where pipeline steps are applied to simple text files, rather than notebooks.

Being able to convert between the `.ipynb` notebook format simple text representations, and back again, means that pipelines can be constructed where notebooks are converted to a text format, one or more automated processing steps are applied to the text representation, and then the processed text file is converted back to the `.ipynb` notebook format.

```{mermaid}
flowchart LR
    .ipynb --jupytext--> .md--> id1{{process Markdown}}--jupytext--> ipynb2[.ipynb]
```

## Converting Between Notebook Text Formats Using `jupytext`

As well as representing notebooks using the `.ipynb` Jupyter notebook file format, notebook *input* content (which is to say, the content of markdown and code cells) and *cell metadata*, __but *not* code cell outputs__, can also be represented by a wide range of simpler markdown and Python (.py) source file inspired text formats.

For example, supported extended markdown flavours include [Jupytext markdown](https://jupytext.readthedocs.io/en/latest/formats.html#jupytext-markdown), [MyST](https://jupytext.readthedocs.io/en/latest/formats.html#myst-markdown), [R Markdown (Rmd)](https://jupytext.readthedocs.io/en/latest/formats.html#r-markdown) and [Quarto](https://jupytext.readthedocs.io/en/latest/formats.html#quarto).

Supported extended code file representations include the [`light` format](https://jupytext.readthedocs.io/en/latest/formats.html#the-light-format) and the [`percent` format](https://jupytext.readthedocs.io/en/latest/formats.html#the-percent-format).

The [`jupytext`](https://jupytext.readthedocs.io/en/latest) package provides a range of tools for supporting two-way conversion between all these formats and to and from the `.ipynb` format. In addition, Jupytext allows the "pairing" of different formats, allowing a MyST markdown file, for example, to be paired with an `.ipynb` notebook. Either file can be edited within a Jupyter Classic notebook, RetroLab or JupyterLab notebook environment and its paired equivalent will also be updated. The advantages of this pairing approach includes:

- the ability to generate simple "input" source text files unencumbered by cell output;
- the ability to preserve cell output in the paired `.ipynb` document.

Jupytext paired document synchronisation is applied automatically when via Jupytext server extensions installed into the Classic Notebook and JupyterLab environments (whenever the document is saved, all paired versions are updated).

In addition, a command-line utility can be used to manually synchronise paired documents, or convert between document formats without pairing.

Local automation is supported in the form of a `pre-commit` hook using a Jupytext command with a `--pre-commit` switch.

```{warning}
A `pre-commit` framework hook is also available also this appears to be largely unusable when working using a `git` workflow mediated by the GitHub Desktop App.
```
