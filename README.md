creol-documentation

## Installation

### Pre Requisites

- Python

### Build instructions

Install Sphinx

```
pip install sphinx
```
Verify the installation with

```
sphinx-build -h
```
Ensure that you have the sphinx_rtd_theme installed

```
pip install sphinx_rtd_theme
```

If you are on Windows and running ```pyenv-win```, and you are running into issues after installing modules. run
```pyenv rehash``` should clear most issues.

Run the following command

```
sphinx-build -M html source build
```

In the newly created build folder, the index.html file will be usable and you can navigate all docs from that.



