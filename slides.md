class: left, middle

🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍
# Python Meta Crash Course
🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍

### *Python tooling*

1. [background + vocab](#background)
2. [pip](#pip)
3. [virtualenv / venv](#venv)
4. [pylint](#pylint)
5. [formatting](#fmt)
6. [python 3](#py3)
7. [setuptools + packaging](#pkg)
8. [hashbang](#hashbang)
9. [pytest / tox](#pytest)
10. [profiling](#prof)
11. [cli scripts](#cli)
12. [cffi](#cffi)

---
name: background

# 1. 🌱 Background + Vocab

## Python module
A python file: `foo.py`.

## Python package
A directory containing 1 or more python modules and an `__init__.py` file:

```plaintext
.
├── __init__.py
└── yolo.py
```

---

## Python import system
Files in our current directory:

```plaintext
.
├── bar.py
├── foo.py
└── a_package
    ├── __init__.py
    └── yolo.py
```

```python
# bar.py
import sys  # import from stdlib or site_packages
import foo  # import from adjacent modules
import a_package.yolo  # import module `yolo` from `a_package`
a_package.yolo.some_function()  # use it

from a_package import yolo  # alternate form
yolo.some_function()  # use it

import foo as foobish  # import module under alias
```

---
name: pip

# 2. 🚚 Pip

Python package manager: *"Pip Installs Packages"*.
Python 2.7+ ships with pip (use `pip3` for explicit Python3; `pip` ok in venv).

*NB: https://pypi.org/ is the Python Package Index*

```bash
# install a package
$ pip install some-package-name

# install a specific package version
$ pip install some-package-name==0.1.2

# install a list of packages
$ pip install -r requirements.txt

# list installed packages
$ pip list

# display info on a specific installed package;
# url, author, license, dependencies, etc.
$ pip show some-package-name
```

---

## Pip continued

```bash
# update a package
$ pip install some-package-name -U

# install from an index
$ pip install --extra-index-url <my-index-url> some-package-name

# install from git!
$ pip install git+git://github.com:pallets/flask.git

# uninstall
$ pip uninstall some-package-name
```

### `pip.conf`

`~/.pip/pip.conf` - contains global pip settings, for example specifying an
extra index url for packages.

https://pip.pypa.io/en/stable/user_guide/#config-file

---
name: venv

# 3. 👩‍🚀 Virtualenv / Venv
.center[![oof.jpg](https://imgs.xkcd.com/comics/python_environment.png)]

---
class: full-height
background-image: url(https://i.redd.it/dfulrui6r2521.jpg)

#

---
### Virtualenv

Virtualenv is just a folder containing a python interpreter (often a symlink)
and sandbox for site_packages (pip) and /bin console-scripts. It's an isolated
python install and package store that protects your system python.

Works for any version of python.

```bash
$ sudo apt install python-virtualenv
$ virtualenv ~/.virtualenvs/default
$ source ~/.virtualenvs/default/bin/activate
$ which python; which pip
$ deactivate

$ virtualenv --clear --python=python3 ~/.virtualenvs/python3/
```

---
### Venv

`venv` ships with python >= 3.3. Similar to virtualenv, but doesn't support
arbitrary python versions; you get a venv based on the source python:

```bash
$ python3 -m venv ~/.virtualenvs/python3
$ source ~/.virtualenvs/python3
$ which python; which pip
$ python --version
```

---
### (Ana)Conda

Multipurpose package manager. Installs to "environments", which are very similar
to python virtualenvs, except you can use `conda` to install packages other than
python packages.

Also supports installing `pip` packages (or you can use pip directly within a
conda env and affect only that environment).

```bash
# create from env file
$ conda env create -f environment.yml

# create raw env
$ conda create --name testenv -y "python==2.7.*" pip

$ conda install, uninstall, list
```

---
name: pylint

# 4. ✨ Pylint

---
name:fmt

# 5. 🚿 Formatting

---
name: py3

# 6. <img src="https://images-na.ssl-images-amazon.com/images/I/61SA0Wq1P1L._SY355_.png" alt="py3" height="42"> Python3

### print()

`print` keyword is 💀
```python
print()  # I'm a function!
```

### Strings are unicode

Slicing strings operates at the glyph level. It is O(n) (see 
[pypy optimization though]).
Encode to bytes/ascii for old-style slicing behavior.

[pypy optimization though]: https://news.ycombinator.com/item?id=19478177

---

### Python3: bytes

Byte types are now used in some io operations (eg file, requests)

```python
# python2
>>> f = open("README.md", "rb")
>>> type(f.read())
<type 'str'>

# python3
>>> f = open("README.md", "rb")
>>> type(f.read())
<class 'bytes'>  # 😮

>>> import requests
>>> type(requests.get("http://httpbin.org").content)
<class 'bytes'>
```

---

### New hotness

#### f-strings

```python
import math
print(f"doing maths {1 + math.sqrt(2)}")

# rip .format() .
```

#### asyncio!!!

https://docs.python.org/3/library/asyncio-task.html

```python
>>> import asyncio

>>> async def main():
...     print('hello')
...     await asyncio.sleep(1)
...     print('world')

>>> asyncio.run(main())
hello
world
```

---

#### Futurize

Don't use six.

```bash
$ pip install future
$ futurize -0 -w janky-module.py
```

---
name: pkg

# 7. 📦 Setuptools + Packaging

---
name: hashbang

# 8. #️⃣ Hashbang

Yes:
```python
#!/usr/bin/env python
```

No:
```bash
#!/usr/bin/python
```

Yes:
```bash
$ chmod +x foo.py
```
🌟🤗🌟

---
name: pytest

# 9. ✔️ Pytest / Tox

---
name: prof

# 10. ⏱ Profiling

### Cprofile
```bash
$ python -m cProfile -s cumtime foo.py <args>
```

### Profy
```bash
$ pip install profy
$ profy foo.py
```

---
name: cli

# 11. 🛠 Cli Scripts

---
name: cffi

# 12. 👽 cffi
