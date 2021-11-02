# Introduction

In many software development projects, automation tools are used to support code development quality processes.

One class of tools are "formatting" tools that can be used to enforce style rules in the layout of programming code. These tools do not change the behaviour of the code, just the way that it appears on-screen. Another class of tools are "linting" tools that will perform static analysis of code to check it for syntactic correctness and identify simple errors.

Tools are also available for performing quality checks on more general texts, including formatters that can be applied to the underlying raw text or spell checkers that perform a "linting" role on the text content. Inline spell-checking tools that can be found in many word processors, and that highlight typographical errors as the user types, might be thought of as a "live" linting tool. Many text editors, such as the VS Code editor, provide support for a wide range on "inline" support tools, including spell checkers and document formatters, through application extensions.

This document describes a range of tools that can be used as part of two sorts of quality process:

- *document transformers*, whereby documents are transformed from one state to another, for example through the application of a formatter that changes the visual appearance of a piece of text without changing its content or a transformer that transforms documents of one type to another;
- *document reporters*, in which a function generates a report based on the static analysis of one or more documents; examples include spellcheckers and linters.

## Automation Pathways

Three main flavours of automation are identified, along with a manual command-line process where individuals can manually run commands against one or more target files:

- *editor based automation*, whereby editor extensions are used to provide interactive, inline help, such as identifying spelling mistakes as you type;
- *`pre-commit` hooks*: in a `git` workflow on the client side, `pre-commit` automation can be used to modify and update checked in files (for example, running them throuhg a formatter) or generate reports (for example, spelling error reports). In addition, `pre-commit` actions may be blocking, and prevent a commit, for example if one or more files contains spelling errors;
- *GitHub actions*: when using an online `git` repository such as GitHub, automation in the form of GitHub Actions (or other CI tools) can be used to process files in response to a wide variety of events, including pushes, pull requests and releases. Actions may be used to update files, for example by applying a formatter, or generate blocking (action failing) or non-blocking reports, such as spell-checking or linter reports.

## Scope

The scope of this document is primarily limited to considering quality process steps, rather than automation associated with generating publishing pipeline output artefacts (for example, Jupyter Book publishing pipelines, of `knitr`/`bookdown` publishing pipelines). Publishing pipeline automation will be reviewed in a separate document.

## Intended Audience

The intended audience for this document includes:

- *authors*, looking for tools to support the authoring process;
- *editors*, looking for tools to support the editing process;
- *developers*, looking for tools or code fragments that can be used to support the deployment of an automated quality process;
- *publishers*, looking for tools to support quality processes within automated document publication pipelines.
