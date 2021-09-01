![GitHub](https://img.shields.io/github/license/florian-huber/python-introduction)

# Code examples for Python introduction course (data science bachelor at HSD)
Code examples for Python introduction course

## Set up Python environment
The simplest way to get the right Python environment to work with during this course is to install [Anaconda](anaconda.org/).

When working with Python it is good practice to work with **environments**, which allow to have separate installations for different projects.
If you want to create a new environment for this course, you can do so by entering the following in your terminal:
```
conda env create -f environment.yml
```
This will create an new environment called 'python_introduction' with all packages named in the `environment.yml` file.
You can also create an environment from scratch and then install all needed packages manually. Do do this, run:
```
conda create --name python_introduction python=3.8
```
This will create an new environment called 'python_introduction' and it will have python 3.8 installed.
Once created, you can activate the new environment by running
```
conda activate python_introduction
```

For more on how to operate **conda**, see for instance this [conda cheat sheet](https://docs.conda.io/projects/conda/en/4.6.0/_downloads/52a95608c49671267e40c689e0bc00ca/conda-cheatsheet.pdf).
