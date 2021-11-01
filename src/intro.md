# Introduction

In many software development projects, automation tools are used to support code development quality processes.

One class of tools are "formatting" tools that can be used to enforce style rules in the layout of programming code. These tools do not change the behaviour of the code, just the way that it appears on-screen. Another class of tools are "linting" tools that will perform static analysis of code to check it for syntactic correctness and identify simple errors.

Tools are also available for performing quality checks on more general texts, including formatters that can be applied to the underlying raw text or spell checkers that perform a "linting" role on the text content. Inline spell-checking tools that can be found in many word processors, and that highlight typographical errors as the user types, might be thought of as a "live" linting tool. Many text editors, such as the VS Code editor, provide support for a wide range on "inline" support tools, including spell checkers and document formatters, through application extensions.

As well as "inline" or "live" support, many of the tools can be applied via automation scripts to check the quality of documents as they pass through a pipeline. In this document, we will review various tools that are available to support quality processes in the publication of educational content authored in a reproducible generative document workflow. Such workflows include workflows based around the `sphinx` based [Executable Books/Jupyter publishing workflow](https://jupyterbook.org/intro.html), the [Rmd / `bookdown` / `knitr` workflow](https://bookdown.org/yihui/bookdown/) or the [Quarto / `pandoc` publishing workflow](https://quarto.org/).

## Intended Audience

The intended audience for this document includes:

- *authors*, looking for tools to support the authoring process;
- *editors*, looking for tools to support the editing process;
- *publishers*, looking for tools to support quality processes within automated document publication pipelines.