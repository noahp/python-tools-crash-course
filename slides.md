class: left, middle

# Python Meta Crash Course

### *Python tooling*

1. background + vocab
2. pip
3. virtualenv / venv
4. pylint
5. formatting
6. python 3
7. setuptools + packaging
8. hashbang
9. pytest / tox
10. profiling
11. cli scripts

---

# 1. 🐍 Background + Vocab

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
import sys  # import from site_packages
import foo  # import from adjacent modules
import a_package.yolo  # import module `yolo` from `a_package`
a_package.yolo.some_function()  # use it

from a_package import yolo  # alternate form
yolo()  # use it

import foo as foobish  # import module under alias
```

---

# 2. 🚚 Pip

Python package manager: *"Pip Installs Packages"*.
Python 2.7+ ships with pip (use `pip3` for Python3; not required in virtual
environments).

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

# 3. 👩‍🚀 Virtualenv / Venv

---

# 4. ✨ Pylint

---

# 5. 🚿 Formatting

---

# 6. Python 3

---

# 7. 📦 Setuptools + Packaging

---

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

# 9. ✔️ Pytest / Tox

---

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

# 11. Cli Scripts
