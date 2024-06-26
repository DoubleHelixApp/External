# Description
[PyPi Page](https://pypi.org/project/doublehelix-external/)

This repository is used to build a wheel that install a module called `helix.third_party`. This module contains 3rd party dependencies for [DoubleHelix](https://github.com/DoubleHelixApp/DoubleHelix) for Windows.
The build works in this way:
- A release is made. The release contains inside its note a table with git tags, one for each of the dependencies
- A GitHub action is executed every time a new release is made
- The action executes `make_clone_script.py` providing the release TAG of the new release as argument
- `make_clone_script.py` fetch the details for the release associated with the TAG, parses the release note, and produce a set of `git clone` command that will clone the dependencies according to the tag specified
- The action build the binaries under a MSYS2 environment
- Once compiled, `import_scanner.py` is run to process the import table for each binary and copy each entry into `helix/third_party`
- Python setup is executed and the wheel is built
- The wheel is uploaded to PyPI

## Install (prod)
```batch
python -m pip install doublehelix-external
```

## Example usage
The module does not contain any python code and is used only to discover the binaries. Example usage:

```python
from helix import third_party
from pathlib import Path
from subprocess import Popen
folder = Path(third_party.__file__).parent
# Launch something, e.g., bcftools
Popen(folder.joinpath("bcftools.exe"))
```
