# Introduction

In many authoring environments, productivity tools such as a spell-checkers help identify, and suggest fixes for, errors introduced as you type, or as part of a document review process. If you work with simple text documents, simple "offline" spell checking commands might also be available that can be run over the file from the command line.

If you think of a typical document processing pipeline - authoring, editing, rendering, publishing - one or more documents are typically worked on at each stage and then passed to the next stage in the publishing pipeline.

There may even be several passes between an author and their editor:

```{uml}
@startuml
concise "Process" as WU

@0
WU is Author

@100
WU is Waiting

@200
WU is Editor

@300
WU is Waiting

@400
WU is Author

@500
WU is Waiting

@600
WU is "..."
@enduml
```

Many of the steps carried out by an editor are high value additions, such as helping the structure and flow of materials and checking consistency across them. But some are likely to be mundane copy editing tasks, such as a spell-checking and typographical style enforcing, that can be supported (in part at least) through automation.

One way of automating the checks is to situate them as part of a document workflow. For example, when passing the document from author to editor, various checks might be run over the document that can signal *back* to the author that certain things may need addressing in advance of passing the document on to the editor, or *feed forward* and identify *in advance* issues that the editor may need to address in the handed over document.

In this report, we will be reviewing quality process supporting automation in a (Jupyter) notebook mediated publishing process. Jupyter notebooks are capable of representing HTML styled content written using simple Markdown text, computer code, and the executed code outputs. Notebooks can also embed a wide variety of rich media asset types (images, video / video players, audio / audio players, interactive tables, interactive maps), as well as provide a medium for supporting the generation of diagrams from simple text descriptions.

```{note}
For examples of various editing environments and extensions that can be used to support notebook based authoring and publication workflows, see [*`OpenJALE`, the Open Jupyter Authoring and Learning Environment*](https://opencomputinglab.github.io/OpenJALE/).

For examples of authoring static and interactive content for a wide range of academic subject areas in the sciences, arts and humanities, see [*Subject Matter Authoring Using Jupyter Notebooks*](https://opencomputinglab.github.io/SubjectMatterNotebooks/).
```

The ability of notebooks to represent text as well as code means that our automation pipeline should be able to address code quality, as well as text quality, concerns.

In many software development projects, quality automation tools are also used to support code development quality processes.

One class of tools are "formatting" tools that can be used to enforce style rules in the layout of programming code. These tools do not change the behaviour of the code, just the way that it appears on-screen. Another class of tools are "linting" tools that will perform static analysis of code to check it for syntactic correctness and identify simple errors.

Tools are also available for performing quality checks on more general texts, including formatters that can be applied to the underlying raw text or spell checkers that perform a "linting" role on the text content. Inline spell-checking tools that can be found in many word processors, and that highlight typographical errors as the user types, might be thought of as a "live" linting tool. Many text editors, such as the VS Code editor, provide support for a wide range on "inline" support tools, including spell checkers and document formatters, through application extensions.

As well as static analysis, checks can also be run to test for correct code execution. Using a reference notebook with run cells, a second version of the notebook can be run and its cell outputs compared with the reference notebook outputs. In this way, any negative side effects regarding the execution of legacy notebooks arising from updating the code execution environment can be detected.

This document describes a range of tools that can be used to support quality processes:

- *document transformers*, whereby documents are transformed from one state to another, for example through the application of a formatter that changes the visual appearance of a piece of text without changing its content or a transformer that transforms documents of one type to another;
- *document reporters*, in which a function generates a report based on the static analysis of one or more documents; examples include spellcheckers and linters;
- *code evaluation checks*, where code cells are executed and checked against reference values or output tests.

## Automation Pathways

Whilst automation can be invoked manually (for example, running test issued on the command line), many automation scripts can be triggered automatically if the documents are being worked on in a managed document system that can raise triggering events as documents are saved into (or retrieved from) the system.

One such document management system is the `git` version control system, which can supports offline working at the local level (for example, on an individual author or editor's own computer) as via a shared document repository (for example, [GitHub](https://github.com)).

```{note}
Many of the automation examples described herein will relate to `git` and GitHub related workflows.

A brief discussion GitHub related workflows will also be provided as part of these materials.
```

Three main flavours of automation are identified, along with a manual command-line process where individuals can manually run commands against one or more target files:

- __*editor based automation*__, whereby editor extensions are used to provide interactive, inline help, such as identifying spelling mistakes as you type;
- __*`pre-commit` hooks*__: in a `git` workflow on the client side, `pre-commit` automation can be used to modify and update checked in files (for example, running them throuhg a formatter) or generate reports (for example, spelling error reports). In addition, `pre-commit` actions may be blocking, and prevent a commit, for example if one or more files contains spelling errors;
- __*GitHub actions*__: when using an online `git` repository such as GitHub, automation in the form of GitHub Actions (or other CI tools) can be used to process files in response to a wide variety of events, including pushes, pull requests and releases. Actions may be used to update files, for example by applying a formatter, or generate blocking (action failing) or non-blocking reports, such as spell-checking or linter reports.

## Scope

The scope of this document is primarily limited to considering quality process steps, rather than automation associated with generating publishing pipeline output artefacts (for example, Jupyter Book publishing pipelines, of `knitr`/`bookdown` publishing pipelines). Publishing pipeline automation will be reviewed in a separate document.

## Intended Audience

The intended audience for this document includes:

- __*authors*__, looking for tools to support the authoring process;
- __*editors*__, looking for tools to support the editing process;
- __*developers*__, looking for tools or code fragments that can be used to support the deployment of an automated quality process;
- __*publishers*__, looking for tools to support quality processes within automated document publication pipelines.
