class: left, middle

🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍
# Python Tools Crash Course

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

🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍🐍

<a class="github-fork-ribbon" href="https://github.com/noahp/python-tools-crash-course" data-ribbon="Fork me on GitHub" title="Fork me on GitHub">Fork me on GitHub</a>

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

## Pip for local packages

```bash
# cwd is an installable python package
$ pip install .

# for development, you can install in "editable" mode
$ pip install -e .

# this places a <package-name>.egg.link into site-packages with
# a path to the package location on disk. freely edit the package
# source to test and avoid installing repeatedly!
```

---
name: venv

# 3. 👩‍🚀 Virtualenv / Venv
.center[![oof.jpg](https://imgs.xkcd.com/comics/python_environment.png)]

---
class: full-height
background-image: url(https://i.redd.it/dfulrui6r2521.jpg)


---
### Virtualenv

Virtualenv is just a folder containing a copy of the python interpreter and
sandbox for site_packages (pip) and /bin console-scripts. It's an isolated
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
### etc.

As usual in Python, buffet style options:

- Pyenv https://github.com/pyenv/pyenv
- Poetry https://github.com/sdispater/poetry
- pipenv https://github.com/pypa/pipenv
- virtualenvwrapper https://virtualenvwrapper.readthedocs.io/en/latest/
- pyenv-virtualenvwrapper https://github.com/pyenv/pyenv-virtualenv

And probably many others.

#### direnv

Direnv can be used to automatically select a particular virtual environment
when entering a directory. Useful if you have a set of projects that use a
particular python / set of packages.
https://github.com/direnv/direnv/wiki/Python

---
name: pylint

# 4. ✨ Pylint

Pep 8 outlines the general style rules:<br/>
https://www.python.org/dev/peps/pep-0008/

Pylint checks for those... but also checks for errors (crashes that would occur
at runtime) and emits warnings for conditions that might cause unexpected
behavior (misuse).

100% pylint compliance will make your python better!

Many other options for linting of course:
- mccabe (complexity) https://github.com/PyCQA/mccabe
- bandit (security) https://github.com/PyCQA/bandit
- safety (vulnerable modules) https://github.com/pyupio/safety
- vulture (dead code) https://github.com/jendrikseipp/vulture
- [many more](https://github.com/vintasoftware/python-linters-and-code-analysis)
of _especial_ interest are the meta-linters pylama, prospector, coala

Flake8 is very popular, and useful to run. Pylint typically catches everything
flake8 does though.
---
name:fmt

# 5. 🚿 Formatting

Use `black`:<br/>
https://github.com/python/black

`black` doesn't have configuration options; only one style is supported.
pep8 compliant, so linters that check for that are happy.

I'm not going to talk about other tools 🙂

```bash
pip3 install black
black <some python files>

# or for ci, returns non-zero for non-compliance
black --check <some python files>
```

`black` requires python3 to execute, but you can run it on python2 files:
```bash
python3 -m black <some python files>
```

*Note: sadly (?) `black` only operates on entire files

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

### New stuff

#### Assignment Expressions

Walrus operator `:=`
https://www.python.org/dev/peps/pep-0572/

```python
# old idiom
match = re.search(r"(\d+\.\d+\.\d+)", "1.2.3")
if match:
    print(match)

# assignment expression version
if match := re.search(r"(\d+\.\d+\.\d+)", "1.2.3"):
    print(match)
```

#### f-strings

```python
import math
print(f"doing maths {1 + math.sqrt(2)}")

# rip .format() .
```

---
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

#### asyncio is _hard_ in python 😔

Mostly due to the way the syntax operates. These are popular workarounds:

- https://github.com/python-trio/trio (well known and used)
- https://github.com/dabeaz/curio (more experimental)


---
#### Futurize

Don't use `six`. It's pretty limited in the compatibility problems it can deal
with.

Use `future`!

```bash
$ pip install future
$ futurize -0 -w janky-module.py
```

Usually, you'll have to test the code some to iron out the remaining issues.
Also of note, `pylint` in py3k compatibility mode:
```bash
pylint --py3k <python file>

# or python3 pylint
python3 -m pylint <python file>
```

---
name: pkg

# 7. 📦 Packaging

Standard package format is `.whl`, which is a Zip archive containing a pre-built
package.

Historically `sdist` (source dist) releases were used, but required valid
compiler for native extensions, and required running the setup script on
destination.

More info at https://pythonwheels.com/ .

---
## Option 1: setuptools

Anatomy:

```bash
# typical package
.
├── foo
│   ├── foo.py
│   └── __init__.py
├── README.md
├── setup.cfg
└── setup.py
```

Using declarative form (`setup.cfg`), `setup.py` is only:
```python
import setuptools
setuptools.setup()
```

---
### `setup.cfg`

```ini
[metadata]
name = foobar
author = Dave Null
author-email = foobar@example.org
license = MIT
long_description = file: README.md
long-description-content-type = text/markdown
url = http://pypi.python.org/pypi/foobar
requires-python = >=2.6
# Add here all kinds of additional classifiers as defined under
# https://pypi.python.org/pypi?%3Aaction=list_classifiers
classifiers =
    Development Status :: 4 - Beta
    Environment :: Console
    License :: OSI Approved :: Apache Software License
    Operating System :: OS Independent
```

---
### `setup.cfg` continued

```ini
[options.entry_points]
# Pip will install a script available on PATH for the current python environment
console_scripts =
    foobar = foobar:main

[bdist_wheel]
universal = 1
```

Populating classifiers enables badges from pypi.org, eg

![PyPI](https://img.shields.io/pypi/v/poetry.svg)
![PyPI - Python Version](https://img.shields.io/pypi/pyversions/poetry.svg)
![PyPI - License](https://img.shields.io/pypi/l/poetry.svg)

```bash
# building
$ python setup.py bdist_wheel

# installing
$ pip install .
# or
$ pip install <path to .whl>
```

---
## Option 2: _poetry_

Packaging tool:
https://github.com/sdispater/poetry

Uses the new toml format for package configuration:
https://www.python.org/dev/peps/pep-0518/

```bash
$ pip install poetry

$ poetry new foobar

$ tree foobar
.
├── foobar
│   └── __init__.py
├── pyproject.toml
├── README.rst  # rename to .md if you like! (I do)
└── tests
    ├── __init__.py
    └── test_foobar.py
```

Supports lock files for dependencies.

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

Pytest at its simplest:
```python
def foo():
    return 4

def test_foo():
    assert foo() is 4, "Whoops foo() fails"
```
```bash
# run it
pytest foo.py -vvv  # extra info when running
```

Coverage:
```bash
pip install pytest-cov

pytest foo.py -vvv --cov=foo --cov-type=html

firefox htmlcov/index.html
```

---
## tox

Tox manages virtualenvs and runs commands, and can run against multiple python versions.

```ini
[tox]
envlist = py27, py36, black

[testenv]
deps=pylint
whitelist_externals=/bin/bash
commands=
    bash -c "pylint ./**/*.py"
    pytest

[testenv:black]
deps=
    black==18.9b0
basepython=python3
commands=
    black --check --verbose .
```
---
## tox continued

```bash
# run it!
$ tox -p auto
GLOB sdist-make: /home/dev/example/setup.py
✔ OK py27 in 3.465 seconds
✔ OK py36 in 7.601 seconds
✔ OK black in 8.622 seconds
____________________ summary ____________________
  py27: commands succeeded
  py36: commands succeeded
  black: commands succeeded
  congratulations :)
```

Info at https://tox.readthedocs.io/ !

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
