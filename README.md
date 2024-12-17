<p align="center">
  <a href="https://github.com/stonefish-dev/code-shield"><img alt="stonefish-code-shield" src="https://raw.githubusercontent.com/stonefish-dev/code-shield/main/images/stonefish-code-shield-logo.svg" width="60%"></a>

Industry-grade Python code protection.

</p>

[![PyPi Version](https://img.shields.io/pypi/v/stonefish-code-shield.svg?style=flat-square)](https://pypi.org/project/stonefish-code-shield/)

For a comprehensive license management solution that is both developer- and
user-friendly, check out the [Stonefish License
Manager](https://github.com/stonefish-dev/license-manager).

### Quickstart

#### Protecting Python Packages

To protect a Python project using the Stonefish Code Shield, ensure that it
includes a minimal
[`pyproject.toml`](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/)
file. Then simple replace your build system (e.g., `setuptools`) with
`stonefish_code_shield` in the `pyproject.toml` file:

```toml
[build-system]
# requires = ["setuptools"]
# build-backend = "setuptools.build_meta"
requires = ["stonefish-code-shield"]
build-backend = "stonefish_code_shield.build_meta"

# ...
# additional project metadata as per PEP 621
# <https://peps.python.org/pep-0621/>
# (recommended)
# ...
```

Done! Your project is now protected with Stonefish Code Shield. Try it
out by running

```
pip install .
```

or

```
(pip install build)
python -m build . --wheel
```

> [!TIP]
>
> In some situations, turning off code protection can be beneficial,
> for example when testing your software. To turn off Stonefish Code Shield,
> build with
>
> ```
> python -m build . --wheel -Cstonefish=off
> ```
>
> When testing with [tox](https://github.com/tox-dev/tox), a possible
> config is
>
> ```ini
> [tox]
> envlist = py3
>
> [testenv:.pkg]
> passenv = *
> config_settings_build_wheel =
>   stonefish-code-shield = off
>
> [testenv]
> passenv = *
> package = wheel
> deps =
>     pytest
> commands =
>     pytest
> ```

#### Protecting Standalone Python Scripts

For individual Python files, you can use the `scs` command-line utility:

```
scs /path/to/file.py
```

### Guidelines

Here are some guidelines to keep in mind while working with Stonefish Code
Shield:

- Stonefish renames class and function names, so relying on the `__name__`
  attribute in your code won't be possible.

- Stonefish cannot yet handle relative `*` imports, e.g.,

  ```python
  from .utils import *
  ```

  In your code, make all imports explicit:

  ```python
  from .utils import div_to_mod, Extractor
  ```

  This is recommended practice anyway.

- Stonefish hides private (underscored) names from the API. If you want your
  users to use these variables or functions, you'll have to rename them.

- Users must add `*.dat` files to their package data, e.g.,

  ```toml
  [tool.setuptools.package-data]
  "*" = ["*.dat"]
  ```

  in `pyproject.toml`. This is because the encrypted code must be shipped with
  the package.

- Local imports should be relative (`from . import x`) instead of using
  absolute imports (`import x`) if `x/` or `x.py` is an internal folder or
  directory.

- Stonefish places all data files in a flat directory structure, so it cannot
  handle files that are read from two different Python paths.

  ```
  ./data.dat

  ./a.py
      Path(__file__).parent / "data.dat"

  ./b/b.py
      Path(__file__).parent / .. / "data.dat"
  ```

### More info

For details on on Stonefish Code Shield protects your code, see
[here](https://github.com/stonefish-dev/code-shield/wiki/How-Stonefish-Code-Shield-Protects-Your-Code).

Contact support@mondaytech.com for more info.
