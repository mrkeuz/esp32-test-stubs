esp32-test-stubs
================

Sample PoC of PyPi packaging [PEP-561][1] stubs for Micropython Esp32. 

As it is only "proof-of-concept" it NOT assumed to for production use and package can be deleted in the future. 
Another purspose of this project is collect info about stubbing

## Installation

```shell 
pip3 install esp32-test-stubs
```

## Script

Next script correct working in PyCharm for autocompete and analyzing with only installed stub package via standart `pip` command. All types does not show any warnings and recognized properly:

```python
import machine
machine.freq()

import esp32
esp32.NVS()
esp32.wake_on_ext0("Test")

from esp.sub_pkg import sub_pkg_fun
sub_pkg_fun()

from esp32.sub_pkg import sub_pkg_function
sub_pkg_function()

import uuid

# Points to stdlib
uuid.uuid4()

# Points to stub (mean partial stub, so extent stdlib)
# (see uuid-stubs and py.typed)
uuid.uuid6()

import upip
upip.cleanup()
```

## Stubs possible locations

- Project root `*.pyi` files
- `<package>-stubs` with `__init__.pyi` (see [stub-only packages][2])
- `<package>-stubs/<sub_package>.pyi` (see [stub-only-packages][2])
- "custom folders" (**Not recommended**) - like `src`, marked as `package = [{ include = "*.pyi" , from = "src"}]`, under hood all `*.pyi`
  will be moved into package root during package build.  
  **CONS**: 
    - "custom folders" does not recognize as stub source before stub package will pack properly. 
    -  stubs not moved properly in in target `*.tar.gz` build, so it led to potential errors during stub recognition from pure sources.

Note: all of these variants should be explicitly marked in `pyproject.toml` in `Poetry` (see `package` section `pyproject.toml`)

## Poetry commands

- Prepare
  ```shell 
  poetry config repositories.testpypi https://test.pypi.org/legacy/
  poetry config pypi-token.testpypi <TOKEN>

  poetry config repositories.pypi https://upload.pypi.org/legacy/
  poetry config pypi-token.pypi <TOKEN>
  ```

- Publish
  ```shell 
  poetry publish --build -r testpypi
  poetry publish --build -r pypi
  ```
## Stub lib examples

- [PyCharm Micropython plugin stubs](https://github.com/vlasovskikh/intellij-micropython/tree/master/typehints)
- [Brython stubs](https://github.com/Real-Gecko/brython-stubs)

## Links
- [PEP-484 - Type Hints, Stub Files ](https://peps.python.org/pep-0484/#stub-files)
- [PEP-561 - Distributing and Packaging Type](https://www.python.org/dev/peps/pep-0561)
- [Real world stubs example (Numpy)](https://github.com/numpy/numpy-stubs)
- [Poetry stub-only project examples](https://github.com/python-poetry/poetry/tree/master/tests/masonry/builders/fixtures/pep_561_stub_only)
- [stubgen](https://mypy.readthedocs.io/en/stable/stubgen.html) - generate stubs from code
- [stubtest](https://mypy.readthedocs.io/en/latest/stubtest.html) - tool for validate stubs files against implementaion ([info](https://stackoverflow.com/questions/51716200/how-do-you-check-if-a-typeshed-stub-pyi-file-matches-the-implementation)) 
- [merge-pyi](https://github.com/google/pytype/tree/main/pytype/tools/merge_pyi) - merge `pyi` types to `py` files
- [tools for stubbing](https://github.com/typeddjango/awesome-python-typing#working-with-types) - list of tools for working with stubs

#### Footnotes
[1]: https://www.python.org/dev/peps/pep-0561
[2]: https://peps.python.org/pep-0561/#stub-only-packages
