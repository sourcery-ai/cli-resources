---
tags: 
- consistency
- option
- naming
title: Re-using Typer Options
---

## Overview

Options used for multiple commands can be defined at a common place. This makes the option name and docs consistent.

## Example

Define the option:

```python
short_name_option: str = typer.Option(
    None,
    help="The short name of the talk.",
)
```

Reference the option in the command argument:


```python
@app.command()
def create(
    short_name: str = short_name_option,
```

## More Info

* [CLI Options with Help](https://typer.tiangolo.com/tutorial/options/help/)
* [CLIG / Arguments and Flags](https://clig.dev/#arguments-and-flags)