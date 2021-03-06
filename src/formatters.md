# Formatters

*Formatters* are tools that format content source code so that it has a consistent visual style and layout.

Formatters are typically used to enforce style conventions on computer code, but formatters are also available for markdown documents as well as other markup languages such as HTML and XML.

In many cases, formatters will automatically make changes to source documents in place since the "content" of the document is not changed, only its layout. That said, formatting tools may also support a "preview" or "difference" mode, where a preview of the suggested changes is generated, or a difference view is generated of the original document and the revised document with suggested layout changes.

## Formatting Notebook Code Cells Using `nbqa`

The [`nbqa`](https://nbqa.readthedocs.io/en/latest) tool provides a simple command-line wrapper around several code formatting and linting tools, including the [`black`](https://black.readthedocs.io/en/stable/), [`yapf`](https://github.com/google/yapf) and [`autopep8`](https://github.com/hhatto/autopep8) code formatters.

Notebooks can be updated directly, or a preview generated of the differences that would arise if updates were automatically applied.

For example, the following screenshot shows a difference preview of the contents of a particular code cell generated by running the `black` formatter with the `nbqa` command:

`nbqa black --nbqa-diff FILENAME.ipynb`

![Example difference output for nbqa black formatter.](images/nbval_black.png)

Running the command *without* the `--nbqa-diff` flag will cause the code cell in the notebook to be updated automatically.

```text
reformatted FILENAME.ipynb
All done! ✨ 🍰 ✨
1 file reformatted.
```

If there are no changes suggested and made, then a `1 file left unchanged.` report is returned.

Note that by default, `nbqa` only runs against *code* cells, so any code in fenced Markdown code blocks will be ignored.

## Formatting Markdown and Notebook Markdown Cells Using `mdformat`

The [`mdformat`](https://mdformat.readthedocs.io/en/latest) package can be used to format markdown content either from the command line, or via a Python code API.

To format a markdown document, simply run a command of the form:

```bash
# Format file(s) in place
mdformat FILENAME1.md FILENAME2.md

# Format files in place in a directory path recursively
mdformat myfiles/
```

If the [`mdformat-black`](https://github.com/hukkin/mdformat-black) package is installed, then `python` labeled fenced code blocks will be formatted using `black`.

Extensions also exist to format other languages in appropriately fenced code blocks, including `bash`/`sh` ([`mdformat-beautysh`](https://github.com/hukkin/mdformat-beautysh)), `JavaScript`/`CSS`/`HTML`/`XML` ([`mdformat-web`](https://github.com/hukkin/mdformat-web)) and `json`/`toml`/`yaml` ([`mdformat-config`](https://github.com/hukkin/mdformat-config)).

Extensions also exist for formatting YAML frontmatter ([https://github.com/butler54/mdformat-frontmatter](`mdformat-frontmatter`)), and formatting Markdown according to MyST ([`mdformat-myst`](https://github.com/executablebooks/mdformat-myst)) or GitHub Flavored Markdown ([`mdformat-gfm`](https://github.com/hukkin/mdformat-gfm)) standards.

To format the markdown content in a Jupyter notebook, two solutions are possible:

- use `nbqa`;
- convert the notebook `.ipynb` document to a markdown format such as MyST using the [`jupytext`](https://jupytext.readthedocs.io/en/latest/) package, format the document using `mdformat` and then convert the updated document back to `.ipynb`.

### Formatting Notebook Markdown Using `nbqa`

As well as formatting code in code cells, the `nbqa` tool can be used to format Markdown in notebook Markdown cells using [`mdformat`](https://mdformat.readthedocs.io/en/latest).

```bash
nbqa mdformat FILENAME.ipynb --nbqa-md
```

The `--nbqa-diff` flag can be used to preview any suggested changes; without the flag, changes are carried out in place.

Code in Markdown code blocks can also be formatted using `black` via the [`blacken-docs`](https://github.com/asottile/blacken-docs) package:

```bash
nbqa blacken-docs FILENAME.ipynb --nbqa-md
```

### Formatting Notebook Markdown Using `jupytext`

Here's an example workflow using `jupytext`:

```bash
# Pair an ipynb notebook with a MyST markdown document
# The paired MyST documents are in a hidden directory
# Note the code cell outputs are not synched
# to the markdown/MySt paired document.
jupytext --set-formats ipynb,.md//myst FILENAME.ipynb

# Format the markdown document
mdformat .md/formatter-test.myst

# Synch the markdown document updates back to the notebook
# This may be required if, for example, it is important
# that code cell outputs are retained.
jupytext --sync formatter-test.ipynb

# Remove the pairing
jupytext --update-metadata '{"jupytext": null}' FILENAME.ipynb
```

Using the [`mdformat-black`](https://github.com/hukkin/mdformat-black) extension installed, `mdformat` will format `python` fenced code blocks in markdown cells (but *not* code cells).

![Example diff output from mdformat run on Jupytext paired MyST document.](images/mdformat.png)

By default, line breaks are not tidied up. This allows authors to use [*semantic line breaks*](https://mdformat.readthedocs.io/en/latest/users/style.html#paragraph-word-wrapping). However, if line length formatting is required, for example to restrict the maximum number of characters allowed per line, an `mdformat` configuration option can be used to override this default (`"keep"`) behaviour:

```toml
#.mdformat.toml

# Enforce a line length
wrap = "no" # possible values: {"keep", "no", INTEGER}
```

Setting the `wrap` parameter to an integer (e.g. setting `wrap = 40`) sets the maximum markdown line length:

![Example of mdformat wrap parameter limiting line length.](images/mdformat2.png)