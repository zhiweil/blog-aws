---
title: Python Virtual Environment
tags: [python]
keywords: python virtualenv
last_updated: March 9, 2019
summary: "How to setup virtual environment for python"
sidebar: mydoc_sidebar
permalink: python_virtualenv.html
folder: python
---

## Installation
* Upgrade your current pip by the following command.

```bash
pip install --upgrade pip --user
```
* Install virtualenv by pip.

```bash
pip install virtualenv --user
``` 

## Built-in functions

* Create a new virtual environment.

```bash
# event here is a directory which holds a Python virtual envrionment, it is
# isolated from the other environments
virtualenv ENV
```

* To activate and deactivate a Python virtual environment

```bash
# This command will modify your .bashrc and activate ENV
source path/to/ENV/bin/activate
# the following command will remove the changes made the instruction above
deactivate
```

## --system-site-packages
If run virtualenv by this option, the virtual environment created will inherits system site packages. Otherwise,
it is completely isolated from system packages. 

```bash
# this instruction tells you where site packages are located.
python -m site
```

## create a environment by inheriting system site packages
```bash
virtualenv --system-site-packages ENV
```

## --extra-search-dir
This option will provide another directory which contains setup tools, pip or wheels. 
```bash
virtualenv --extra-search-dir /path/to/another/dis
```

## Help!
```bash
# -h or --help
virtualenv --help
```

## References
* [Virtualenv Official Site](https://virtualenv.pypa.io/en/latest/) 