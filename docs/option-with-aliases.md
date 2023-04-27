---
tags:
- option
---

## Overview

You can provide multiple aliases for an option. This is helpful when multiple expressions are common for the same concept.

## Simple Option with Multiple Aliases

An option that accepts both `--directory` and `--dir`:

```python
@app.command()
def create(
    directory: Optional[Path] = typer.Option(
        None, "--directory", "--dir", help="The directory of the page."
    ),
):
```

## Option with Full-Length and Short Name

An option that accepts both `--all` and `-a`:

```python
@app.command()
def ls(
    all: bool = typer.Option(False, "--all", "-a", help="Show all items."),
):
```

CLIG's recommendation regarding short names:

> Only use one-letter flags for commonly used flags, particularly at the top-level when using subcommands. That way you don’t “pollute” your namespace of short flags, forcing you to use convoluted letters and cases for flags you add in the future.

[CLIG / Arguments and Flags](https://clig.dev/#arguments-and-flags)

## `bool` Option with Multiple Aliases

A `bool` option with 2 names both for the `True` and the `False` value:

```python
@app.command()
def create(
    interactive_option: bool = typer.Option(
        True,
        "--interactive/--no-interactive",
        "--input/--no-input",
        help="Switch whether interactive prompts are shown. Use `--no-input` when you call this command from scripts.",
    )
):
```

## More Info

* [Typer Docs / CLI Option Name](https://typer.tiangolo.com/tutorial/options/name/)
* [Typer Docs / Boolean CLI Options](https://typer.tiangolo.com/tutorial/parameter-types/bool/)
* [CLIG / Arguments and Flags](https://clig.dev/#arguments-and-flags)