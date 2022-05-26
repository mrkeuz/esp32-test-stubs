esp32-test-stubs
================

Sample project for test [PEP-561][1]

Notice: It is only "proof-of-concept" of [PEP-561][1] package so **not for production use**, 
package can be deleted in the future. 

## Installation

```shell 
pip3 install esp32-test-stubs
```

## Script

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
# see uuid-stubs and py.typed
uuid.uuid6()

import upip
upip.cleanup()
```

## Stubs possible locations

- project root `*.pyi` files
- `<package>-stubs` with `__init__.pyi` see [PEP-561](https://www.python.org/dev/peps/pep-0561)
- `<package>-stubs/<sub_package>.pyi` see [stub-only-packages](https://www.python.org/dev/peps/pep-0561/#stub-only-packages)
- custom folder like `src`, marked as `package = [{ include = "*.pyi" , from = "src"}]`,
  under hood all `*.pyi` will be moved into package root during package build.  
  CONS: **Not recommended** as custom folder does not recognize as stub source before stub package will pack properly. 
  Also in `*.tar.gz` stubs not moved properly, so it led to potential errors during stub recognition   

Note all of these variants should be explicitly marked in `pyproject.toml` in `Poetry` see `package` section

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

## Links

- [PEP-561](https://www.python.org/dev/peps/pep-0561)
- [Real world stubs example (Numpy)](https://github.com/numpy/numpy-stubs)
- [Poetry stub-only project examples](https://github.com/python-poetry/poetry/tree/master/tests/masonry/builders/fixtures/pep_561_stub_only)

#### Footnotes
[1]: https://www.python.org/dev/peps/pep-0561