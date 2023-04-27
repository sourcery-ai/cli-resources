---
tags: 
- robustness
- option
title: Interactive Input
---

## Overview

* Prompt for missing values instead of raising an error immediately.
* Do not prompt if `stdin` isn't an interactive terminal. 💻
* Do not prompt if the `--no-input` or `--no-interactive` option has been provided

## Examples

### Basic Example

Prompt for 1 option (`--description`) if missing.

```python
import sys

import typer
from rich.prompt import Prompt

@app.command()
def create(
    description_option: str = typer.Option(
        None,
        "--description",
        help="Ca. 1 sentence description.",
    ),
    interactive_flag: bool = typer.Option(
        True,
        "--interactive/--no-interactive",
        "--input/--no-input",
        help="Switch whether interactive prompts are shown. Use `--no-input` when you call this command from scripts.",
    ),
):
    interactive = sys.stdin.isatty() and interactive_flag

    description = description_option or interactive and Prompt.ask("Description")
```

### More Complex Example

* If no fields were provided at all via options => Prompt for all fields.
* If the object can't be created => Prompt for the fields that have raised a validation error.

```python
@app.command()
def create(
    ctx: typer.Context,
    short_name: str = short_name_option,
    duration_total_seconds: int = duration_total_seconds_option,
    speaker_name: str = speaker_name_option,
    title: str = title_option,
    youtube_video_id: str = youtube_video_id_option,
    interactive_flag: bool = INTERACTIVE_OPTION,
    plain: bool = FormatOptions.PLAIN,
    show_json: bool = FormatOptions.JSON,
):
    """Create a new talk"""
    interactive = sys.stdin.isatty() and interactive_flag

    stdout_console = Console()
    stderr_console = Console(stderr=True)

    provided_further_fields = _get_field_values(ctx, interactive, stderr_console)
    try:
        result = talk_crud().create(provided_further_fields)
    except ValidationError as e:
        if not interactive or not Talk.all_simple_field_errors(e):
            raise exit_with_error(f"Can't create a talk. {e}") from e

        for error_data in e.errors():
            field_name = error_data["loc"][0]
            msg = error_data["msg"]
            stderr_console.print(f"Validation error. {field_name} {msg}")
            provided_further_fields[field_name] = Prompt.ask(field_name)
        try:
            result = talk_crud().create(provided_further_fields)
        except ValidationError as e_next:
            raise exit_with_error(f"Talk still invalid. {e_next}") from e_next
    stderr_console.print("New talk created. 🪅")
    _print_talk(result, show_json, plain)

```

## Remark

Typer also provides [CLI option with Prompt](https://typer.tiangolo.com/tutorial/options/prompt/)

This allows for less fine-grained control. (It doesn't take into account the `--interactive/--no-interactive` flag.)

## More Info

* [CLI Guidelines / Interactivity](https://clig.dev/#interactivity)
* [Rich: Prompt docs](https://rich.readthedocs.io/en/latest/prompt.html)