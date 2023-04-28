---
tags:
- interface
---

## Overview

When code prints things, it actually writes to one of two virtual files: "standard output" or "standard error".

* For a human user, there's usually no difference: Both `stdout` and `stderr` are visible on the terminal.
* For machines, there's a difference: A script consuming the output of your script "sees" only `stdout`.

Distinguishing between `stdout` and `stderr`, is one of the [Basics recommended by the CLIG](https://clig.dev/#the-basics)

TLDR

* Send output that can be piped and consumed by a next script to `stdout`.
* Send everything else, especially errors and log messages to `stderr`.

For more explanation, on `stdout` and `stderr`, see also the [Typer Docs](https://typer.tiangolo.com/tutorial/printing/#standard-output-and-standard-error)

## Printing to `stdout` vs `stderr` with Rich

In Rich, you use a `rich.console.Console` object for printing.

* By default, it prints to `stdout`.
* You can initialize the `Console` with `stderr=True`, so that it prints to `stderr`.

After creating a talk, print:

* the talk ID to `stdout`
* a logging message to `stderr`

```python
from rich.console import Console

stdout_console = Console()
stderr_console = Console(stderr=True)

stdout_console.print(talk.talk_id)
stderr_console.print("New talk created. 🪅")
```

## Testing

For tests with `typer.testing.CliRunner`, it's recommended to verify the content of `stdout` and `stdin` separately.

In order to do that, you need to initialize the `typer.testing.CliRunner` instance with `mix_stderr=False`.

```python
runner = CliRunner(mix_stderr=False)
```

## Iteration vs Keeping the Interface Stable

> Changing output for humans is usually OK. The only way to make an interface easy to use is to iterate on it, and if the output is considered an interface, then you can’t iterate on it. Encourage your users to use --plain or --json in scripts to keep output stable (see Output).

[CLIG Guidelines / Future-proofing](https://clig.dev/#future-proofing)

## More Info

* [Typer Docs / Printing and Colors / "Standard Output" and "Standard Error"](https://typer.tiangolo.com/tutorial/printing/#standard-output-and-standard-error)
* [CLIG Guidelines / Basics](https://clig.dev/#the-basics)
* [CLIG Guidelines / Future-proofing](https://clig.dev/#future-proofing)