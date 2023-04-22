---
title: Configure the Path of a SQLite Database
tags:
- configuration
---

## Overview

We want to use different databases in the different environments:

* development
* tests
* production / live

We use `pydantic.Settings` and an environment variable to make the path configurable.

* For development: set the environment variable.
* For tests: Use a pytest fixture with `tmpdir` to define a custom path.

## Config

In a `config.py`:

```python
from pathlib import Path

from pydantic import BaseSettings, Field
from sqlite_utils import Database

from pycon_catalogue.initialize import init_db


class Settings(BaseSettings):
    db_path: Path = Field(
        Path.home() / "pycon-catalogue.db", env="APP_NAME_DB_PATH"
    )

    def db(self) -> Database:
        return Database(self.db_path)


settings: Settings = Settings()
```

Instead of `Path.home()`, you can also use a more sophisticated default value:

* [`typer.get_app_dir()`](https://typer.tiangolo.com/tutorial/app-dir/)
* [platformdirs library](https://pypi.org/project/platformdirs/)

## Setting a Development Database

```
export APP_NAME_DB_PATH=$HOME/dev-app-name.db
```

## Using a Temporary Database for Tests

Use a db in `tmpdir`.

```python
from pathlib import Path

import pytest

from app_name import config


@pytest.fixture(autouse=True)
def app_db_path(tmpdir):
    config.settings.db_path = Path(tmpdir / "test-app-name.db")
```

## More Info

* [Pydantic / Settings Management](https://docs.pydantic.dev/usage/settings/)
* [`tmpdir`](https://docs.pytest.org/en/7.1.x/reference/reference.html#std-fixture-tmpdir)