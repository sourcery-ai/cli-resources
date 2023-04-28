---
tags: 
- testing
title: Using `typer.testing.CliRunner`
---

## Overview

`typer.testing.CliRunner` is a great tool for testing `typer` applications.

Checklist:

* [ ] Initialize with `mix_stderr=False`
* [ ] Ensure that each argument of `invoke` is a string.
* [ ] When asserting the value of `stdout` or `stderr`, add a trailing line break.

## Initialization

```python
runner = CliRunner(mix_stderr=False)
```

By default, `CliRunner` is created with `mix_stderr=True`. This means, that the content of `stdout` and `stderr` will both appear in `result.stdout`.

Recommended practice: Verify separately the content of `stdout` and `stderr`. => Create the `CliRunner` instance with `mix_stderr=False`

Verifying the content of `stdout` and `stderr` separately after invoking a command:

```python
from pycon_catalogue.cli.cli import app

runner = CliRunner(mix_stderr=False)

def test_success(cmd_parts):
    result = runner.invoke(app, "create", "--name", "something")

    assert result.exit_code == 0
    assert result.stdout
    # stderr
    assert "New talk created. 🪅" in result.stderr
```

[Typer docs](https://typer.tiangolo.com/tutorial/testing/#check-the-result)

## Invoke: Use only string Arguments

Example:

```python
def test_not_existing():
    result = runner.invoke(app, ["talk", "view", "42"])
```

Pitfall:

```python
def test_not_existing():
    result = runner.invoke(app, ["talk", "view", 42])
```

This will lead to an error during invoking the command:

<Result TypeError("object of type 'int' has no len()")>

This is especially easy to oversee when testing error scenarios.

## Asserting `stdout` and `stderr`: Add a Trailing Line Break

```python
assert result.stderr == "Talk with ID 42 not found.\n"
```

## More Info

* [Typer Testing docs](https://typer.tiangolo.com/tutorial/testing/)