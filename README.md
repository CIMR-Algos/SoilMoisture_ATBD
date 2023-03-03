
## CIMR Level-2 Soil Moisture ATBD
A JupyterBook with CIMR DEVALGO Level-2 Soil Moisture ATBD

## Fetch and read the book
If you just want to download the repo and read the book (not building it):
```
cd </path/to/where/you/want/the/repo> # call it /PATH/
git clone <repo>
```
In your web browser load `file:///PATH/book/_build/html/intro.html` in the url field.

## Instructions for building the book

### If needed, install miniconda
See https://docs.conda.io/en/latest/miniconda.html

### Create a conda environment and install the jupyter-book tools
```
conda create -c conda-forge --name JB python=3.8
conda activate JB
conda env list # check that JB is the current env
pip install --upgrade pip #(if needed several times)
pip install jupyter-book
```

### install packages needed for the book (this list might grow as we develop the books)
```
conda install -c conda-forge matplotlib numpy
```

### build the book
```
cd ls
# the root of the repo, with book/ algorithm/ data/ ...
jupyter-book build book/
jupyter-book build --all book/ #(force rebuild everything)
```
