---
title: Raising an Error in Typer
tags:
- error-handling 
- consistency
---

## Overview

If an error happens, ensure that:

* [ ] The command returns a non-zero exit code.
* [ ] Nothing is printed to `stdout`.
* [ ] Some error message is printed to `stderr`.

The exit code of the last command is stored in the shell variable `$?`.

You can check that with:

```bash
echo $?
```

## Recommended Practice: Helper Function

Use a helper function for raising errors.

Benefits:

* ensuring that a non-zero exit code is returned
* ensuring that an error message is shown
* consistent error messages

Example:

```python
def exit_with_error(error_msg: str, code: int = 1) -> typer.Exit:
    stderr_console = Console(stderr=True, style="bold red")
    stderr_console.print(error_msg)
    return typer.Exit(code=code)
```

In the caller:

```python
if some_error_happened:
    raise exit_with_error(str(e)) from e
```

## Pitfalls

### Raising typer.Exit() without Providing a Code

```python
raise typer.Exit()
```

The command will return with exit code 0.

## Testing

Example test for an error case:

```python
def test_view_not_existing_talk():
    result = runner.invoke(app, ["talk", "view", "42"])

    assert result.exit_code == 1
    assert not result.stdout
    assert result.stderr == "Talk with ID 42 not found.\n"
```

## More Info

* [`typer.Exit` docs](https://typer.tiangolo.com/tutorial/terminating/#exit-with-an-error)
* [CLIG Guidelines / The Basics](https://clig.dev/#the-basics)