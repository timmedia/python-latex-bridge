# python-latex-bridge
Python package to include python variables in a LaTeX document, whereby the `pint` and `uncertainty` package are supported.

## Installation
As a user with sufficient permission run:

    pip install python-latex-bridge

or

    python3 -m pip install python-latex-bridge

Alternatively, one can clone / download this project and run the code locally.

## Usage
To create a file `dimensions/radius.dat` containing the value of a variable `r0`, run
```python
save(r0, 'radius', 'dimensions')
```
whereby the function signature with default arguments is given as:
```python
save(variable, name: str='variable', path: str=None) -> None
```

## Examples
Some use cases given a python file with
```python
from bridge import save
from uncertainties import ufloat
from pint import UnitRegistry
ureg = UnitRegistry()
Q = ureg.Quantity
```
to save...
- any python variable:
    ```python
    # creates `variables/variable.dat` containing "1024"
    x = 1024
    save(x)
    ```
- a value and its uncertainty:
    ```python
    # creates `variables/height.dat` containing "3.10 \pm 0.20"
    x = ufloat(3.1, 0.2)
    save(x, name='height')
    ```
- some variable with its unit using the [siunitex](https://ctan.org/pkg/siunitx) package:
    ```python
    # creates `values/variable.dat` containing "42\,\si[]{\milli\meter}"
    x = Q(42, 'mm')
    save(x, path='values')
    ```
- a combination of the aforementioned:
    ```python
    # creates `variables/variable.dat` containing "(420 \pm 7)\,\si[]{\ohm}"
    x = Q(ufloat(420, 7), 'ohm')
    save(x)
    ```
## Usage in LaTeX
To fetch the value in a LaTeX document, simply include the `.dat` file in math mode
```
$\input{variables/variable.dat}$
```
assuming the `.tex` file and the `variables/` folder share the same parent directory. 