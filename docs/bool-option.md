---
tags:
- option
---

## Overview

Typer creates per default 2 names for each bool option:

* one for the `True` value
* one for the `False` value

You can customize this by providing explicit option names.

## Default `bool` Option with Names for `True` and `False`

A bool option:

This creates 2 options:

## `bool` Option with a Name Only for `True`

For some `bool` options, it doesn't make sense to provide a separate name for the `False` value.

This snippet defines a `--version` option without creating a respective `--no-version`:

```python
@app.command()
def some_cmd(
    version: bool = typer.Option(False, "--version", help="Print the current version."),
) -> None:
```

## `bool` Option with a Custom Name for `False`

```python
@app.command()
def some_cmd(
    accept: bool = typer.Option(True, "--accept/--reject", help="Accept or reject. Default: accept"),
) -> None:
```

## `bool` Options with 3 Possible Values

**This is possible but not recommended.**

You can also define an option with the type `Optional[bool]`. These options have 3 possible values:

* `None`
* `True`
* `False`

```python
@app.command()
def trio(
    size_option: bool = typer.Option(
        None, "--small/--big", help="The size of something. Default: medium"
    ),
) -> None:
    if size_option is None:
        size = "medium"
    elif size_option:
        size = "small"
    else:
        size = "big"
    Console().print(size)
```

Note that this behavior might be surprising / confusing.

## More Info

* [Typer Docs / CLI Option Name](https://typer.tiangolo.com/tutorial/options/name/)
* [Typer Docs / Boolean CLI Options](https://typer.tiangolo.com/tutorial/parameter-types/bool/)
* [CLIG / Arguments and Flags](https://clig.dev/#arguments-and-flags)