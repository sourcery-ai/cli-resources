---
tags:
- command
- naming
---

## Overview

Typer doesn't support multiple aliases for the same command.

You can define an alias in Python by:

* Calling one command function from the other.
* Calling the same private function from both commands.

## Two Commands Calling the Same Function

Create 2 commands `pycon` and `display-pycon` executing the same function:

```python
@app.command()
def display_pycon():
    _print_pycon()


@app.command()
def pycon():
    _print_pycon()


def _print_pycon():
    Console().print("PyCon!")
```

If the aliased command also has options, consider using the [re-use options](re-using-options.md) pattern.

## CLIG Recommendation on Command Aliases

> Don’t allow arbitrary abbreviations of subcommands. For example, say your command has an install subcommand. When you added it, you wanted to save users some typing, so you allowed them to type any non-ambiguous prefix, like mycmd ins, or even just mycmd i, and have it be an alias for mycmd install. Now you’re stuck: you can’t add any more commands beginning with i, because there are scripts out there that assume i means install.

> There’s nothing wrong with aliases—saving on typing is good—but they should be explicit and remain stable.

[CLIG Guidelines / Future-proofing](https://clig.dev/#future-proofing)

## More Info

* [CLIG Guidelines / Naming](https://clig.dev/#naming)
* [CLIG Guidelines / Subcommands](https://clig.dev/#subcommands)
* [CLIG Guidelines / Future-proofing](https://clig.dev/#future-proofing)