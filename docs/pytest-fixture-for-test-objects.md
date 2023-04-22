---
tags: 
- testing
title: Use pytest Fixtures for Creating Test Objects
---

## Overview

If a test needs one or more objects, provide them via pytest fixtures.

Do not run multiple `invoke`s of `typer.testing.CliRunner` in one test.

## Example

Fixture:

```python
@pytest.fixture()
def create_basic_talk():
    return talk_crud().create({"short_name": "error-messages"})
```

Test function:

```python
def test_existing_by_id(create_basic_talk):
    result = runner.invoke(app, ["talk", "view", str(create_basic_talk.id_)])

    assert result.exit_code == 0
    assert result.stdout
```

This provides a clear separation between the different phases of the test:

* arrange
* act
* assert

## Pitfall

Another possibility would be to invoke the `create` command in the test before invoking the "command under test" `view`.

```python
def test_existing_by_id(create_basic_talk):
    runner.invoke(app, ["talk", "create", "--short-name", "some-name"]

    result = runner.invoke(app, ["talk", "view", str(create_basic_talk.id_)])

    assert result.exit_code == 0
    assert result.stdout
```

Drawback: If the `create` command fails, the `invoke` is still successful and proceeds to the next step.

=>

The invoking of the `view` command will fail, which is misleading. (The `create` command has an error, but the test fails in the `view` step.)

## More Info

* [About pytest Fixtures](https://docs.pytest.org/en/7.1.x/explanation/fixtures.html#about-fixtures)
* [AAA Arrange-Act-Assert Pattern](https://automationpanda.com/2020/07/07/arrange-act-assert-a-pattern-for-writing-good-tests/)