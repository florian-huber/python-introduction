[![Jupyter Book Badge](https://jupyterbook.org/badge.svg)](https://florian-huber.github.io/python-introduction/)
![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/florian-huber/python-introduction/CI_tests.yml)

<p align="center">
    <img src="https://github.com/florian-huber/python-introduction/blob/main/images/cover_sketching.png" width="600" alt="Course cover">
</p>

# Einführung Programmieren mit Python (Data Science related bachelor at HSD)
Skript zur Vorlesung "Einführung Programmieren für Data Science und Künstliche Intelligenz" mit Code Beispielen und Erklärungen.
# Python introduction course (data science related bachelor at HSD)
Code examples for Python introduction course (english version yet to come...).

## Set up Python environment
The simplest way to get the right Python environment to work with during this course is to install [Anaconda](anaconda.org/).

When working with Python it is good practice to work with **environments**, which allow to have separate installations for different projects.

### Set up pre-defined environment for this course
If you want to create a new environment for this course, you can do so by entering the following in your terminal:
```
conda env create -f environment.yml
```
This will create an new environment called 'python_introduction' with all packages named in the `environment.yml` file.

### Or: Set up own environment from scratch
You can also create an environment from scratch and then install all needed packages manually. Do do this, run:
```
conda create --name python_introduction python=3.12
```
This will create an new environment called 'python_introduction' and it will have python 3.12 installed.

### Activate environment
Once created, you can activate the new environment by running
```
conda activate python_introduction
```

For more on how to operate **conda**, see for instance this [conda cheat sheet](https://docs.conda.io/projects/conda/en/4.6.0/_downloads/52a95608c49671267e40c689e0bc00ca/conda-cheatsheet.pdf).


# Credits, resources, references
The code examples are tested using [pytest-codeblocks](https://github.com/nschloe/pytest-codeblocks/).
