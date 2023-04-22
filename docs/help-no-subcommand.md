---
tags: 
- robustness
title: Show Help for Missing Subcommand
---

## Overview

If no subcommand has been provided => Show the help of the command.

(instead of raising an error)

## Example

```python
@app.callback(invoke_without_command=True)
def callback(ctx: typer.Context),
) -> None:
    if ctx.invoked_subcommand is None:
        Console().print(ctx.get_help())
```

## More Info

* [Typer Callback docs](https://typer.tiangolo.com/tutorial/commands/callback/)
* [Typer Context docs](https://typer.tiangolo.com/tutorial/commands/context/)