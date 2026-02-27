# gamagama-core

`gamagama-core` (Game Master Game Manager — Core) provides the shared abstractions for building gamagama game system plugins.

It defines two abstract base classes — `GameSystem` and `DiceEngine` — that game system packages (such as `gamagama-rmu`) implement to integrate with `gamagama-cli`.

## Installation

`gamagama-core` is a library, not a standalone tool. Install it into a virtual environment with pip:

```bash
pip install .
```

In most cases you will not install it directly. It is pulled in automatically as a dependency of any game system plugin that requires it.

## Usage

### GameSystem

Subclass `GameSystem` to implement a game system plugin:

```python
from gamagama.core import GameSystem, DiceEngine

class MyDiceEngine(DiceEngine):
    def roll(self, sides: int, explode: bool) -> int:
        # custom dice logic
        ...

class MySystem(GameSystem):
    name = "mysystem"

    def __init__(self):
        self.dice = MyDiceEngine()
```

`GameSystem` also supports optional [Pydantic](https://docs.pydantic.dev/) schema definitions via the `schemas` class attribute, which maps schema names to Pydantic model classes.

### DiceEngine

`DiceEngine` provides a `roll(sides, explode)` method. The base implementation rolls a single die and, if `explode=True`, keeps rolling and adding whenever the maximum value is hit.

## Contributing

To run the tests:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -e ".[test]"
pytest
```
