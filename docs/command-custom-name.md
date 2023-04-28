---
tags:
- command
- naming
---

## Overview

By default, Typer creates a command with the same name as the function.

* If the function name contains an `_` => It wil be replaced with `-`.
* You can define a custom command name in the 1st argument of the `@app.command()` decorator.

## Custom Command Name

Define a command named `list` using the function `list_items`.

```python
@app.command("list")
def list_items():
```

This pattern is useful if your command name is the name of a Python built-in function (like `list`) or a Python standard library module (like `os`).

## More Info

* [Typer / Custom Command Name](https://typer.tiangolo.com/tutorial/commands/name/)
* [CLIG Guidelines / Naming](https://clig.dev/#naming)