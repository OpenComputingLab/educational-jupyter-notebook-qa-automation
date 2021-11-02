# Notebook Code Execution

In certain publication workflows, there may be a need to ensure that code execution integrity in notebooks is preserved as and when a code execution environment is updated.

For example, the code execution environment might have operating system package updates, or Python package updates even if the notebooks are not the driving force behind the updates. In such cases, it is important to be able to test the notebooks to ensure that the package updates have not broken the notebooks, or started to raise warnings within them (such as deprecation warnings, for example).

## The `nbval` Notebook Code Testing Framework

The [`nbval`](https://nbval.readthedocs.io/en/latest/) Python package provides an extension to the widely used [`pytest`](https://docs.pytest.org/) framework TO DO

TO DO fragments eg [Automated Testing of Educational Jupyter Notebook Distributions Using Github Actions](https://blog.ouseful.info/2021/10/26/automated-testing-of-jupyter-notebooks/),  [More Automation Sketches – Creating Student Notebook Releases](https://blog.ouseful.info/2021/10/27/more-automation-sketches-creating-student-notebook-releases/) and [Another Automation Step: Spell-Checking Jupyter Notebooks](https://blog.ouseful.info/2021/10/28/another-automation-step-spell-checking-jupyter-notebooks/)