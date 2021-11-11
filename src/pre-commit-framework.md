# Pre-Commit

*Git pre-commit hooks* are client side events that are raised and handled by simple scripts whenever a commit is made to a `git` repository.

Simple pre-commit scripts were originally based around executable shell scripts placed in a hidden `.git/hooks/pre-commit` file inside a `git` repository. These scripts would be invoked whenever a commit was made into the local repository

Writing pre-commit scripts can be a complicated affair, but the `pre-commit` Python package provides a much simpler and more powerful framework for writing and installing your own automation scripts.

## Motivation

One of the problems associated with running quality assurance tools over a set of files is that it's easy to *not get round to it*.

In a live editing environment, if the editing tool provides "live" error checking, such as code linting or inline spell-checking, you are often more likely to pick up on issues at the time they are created. However, when errors do make it through to the saved version of a file, as they inevitably will, they can often build up over time. When a quality process is invoked, there may be a significant backlog of issues. What's worse is that with multiple errors or issues in a document, whether it's a text document or a code file, fixing one may have consequential side effects on others, or on the rest of the document.

If files are being managed under a version control system such as `git`, files are often committed to the repository on a regular basis. In some workflows, each substantive change may be committed as a separate check-in or commit with a comment describing the nature of the change. In other workflows, large numbers of undistinguished and independent changes may be committed at the same time. The latter approach may be less than useful when trying to track down a very specific change, but it comes without the overhead of trying to comment on each particular change, each with its own change note, at the time the change is made.

In workflows producing text documents, it may be unclear as to what makes a sensible commit chunk. In such a case, commits may be made at natural breaks in the workflow, such as at the end of a particular section of the document, or at the end a particular work session, such as at the end of the day.

Check-ins may also reflect the completion of a particular task, such as completing the editing a particular section, or running a spell-checker over a complete document.

## Pre-commit Tasks

Pre-commit workflows are established by running a `pre-commit install` command to configure the `.git/hooks/pre-commit` file. The pre-commit tasks themselves are defined via a `.pre-commit-config.yaml` configuration file. This file specifies what tasks should be performed on the set of files that are submitted as part of the commit process.

*The first time a commit is made using a particular action, there may be some delay as the code implementing the action is downloaded from the specified action repository and then cached locally.*

### Example `markdownlint` pre-commit Action

We can use a simple GitHub Action to check a Markdown file, as identified using a set of declared file suffixes, using `markdownlint`.

```yaml
-   repo: https://github.com/markdownlint/markdownlint
    rev: v0.11.0
    hooks:
    -   id: markdownlint
        name: Markdownlint
        description: Run markdownlint on your Markdown files
        entry: mdl -r ~MD013
        language: ruby
        files: \.(md|mdown|markdown|myst)$
```

Specific lint error codes can be ignored by passing them prefixed with a `~` character via the `mdl` command-line command `-r` switch.

### Example `codespell` pre-commit Action

The following example of a `.pre-commit-config.yaml` file shows how to define a `codespell` pre-commit action that will spell check all committed files.

```yaml
repos:
-   repo: https://github.com/codespell-project/codespell
    rev: v2.1.0  # CURRENT_TAG/COMMIT_HASH
    hooks:
    -   id: codespell
        name: codespell
        description: Checks for common misspellings in text files.
        entry: codespell --skip="*.js,*.html,*.css, *.svg" --ignore-words=.codespell-ignore.txt
        language: python
        types: [text]
```

Note that for this action, the commit is prevented if the spell-check fails.

![](images/codespelling-action.png)

 The output report shows which files contained detected spelling errors, and what those errors were. It is then up to the user to fix those errors before trying to commit the file again.

 ```{note}
 This idea of blocking a commit until the error is addressed is reminiscent of the __jidoka__ idea in the Toyota Production System / lean manufacturing process.
 ```

### Example `nbqa black` Example

The `nbqa` repository provides [many examples](https://github.com/nbQA-dev/nbQA/blob/master/.pre-commit-hooks.yaml) of how to configure `pre-commit` to apply various `nbqa` actions over checked in notebooks.

For example, the following configuration example shows how to use `nbqa` to apply the `black` formatter to notebook code cells:

```yaml
-   repo: https://github.com/nbQA-dev/nbQA
    rev: 1.1.1
    hooks:
    -   id: nbqa-black
        name: nbqa-black
        description: "Run 'black' on a Jupyter Notebook"
        entry: nbqa black
        language: python
        require_serial: true
        types: [jupyter]
        additional_dependencies: [black]
```

Here's an example of the report that is generated by trying to commit a notebook file that contains at least one code cell that `black` has seen fit to correct:

![](images/nbqa-black-precommit.png)

The commit is halted as the `black` formatter runs over the file that we initially committed. The file is updated as a result of the failed commit action and if we now try to commit it again, it will pass.

Note that under this workflow, we take it on trust that the `black` formatter has updated the code cell(s) (on our behalf) in a way that we approve of, but aren't necessarily able to bear witness to.

```{danger}
The user experience using the GitHub application and the `jupytext` commit hook is horrible, and [virtually unusable](https://github.com/mwouts/jupytext/issues/831#issuecomment-890318731), compared to the original, and now deprecated, `jupytext --pre-commit` workflow.

In that original case, evoked using a simple `jupytext --from ipynb --to .md//markdown --pre-commit` line in a `.git/hooks/pre-commit` file, paired notebook files would be automatically synched if either one as committed.

Using the `pre-commit` framework, trying to commit files in the Github application seems to be to be a painful, confusing and unreliable experience. (I may be doing something wrong, of course...)

I get the feeling this a [`wont-fix`](https://github.com/mwouts/jupytext/issues/831#issuecomment-899353065) because the `pre-commit` framework folk think everyone should be typing `git` commands on the command line whenever they use `git`...

So I just hope that the old `--pre-commit` jupytext flag continues to work because the approved pre-commit route doesn't work in any useable sense at all for me.
```
