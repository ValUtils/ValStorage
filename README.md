# ValStorage

[![PyPI - Version](https://img.shields.io/pypi/v/ValStorage?label=ValStorage)](https://pypi.org/project/ValStorage/)
![GitHub deployments](https://img.shields.io/github/deployments/ValUtils/ValStorage/deploy?label=deploy)
![GitHub](https://img.shields.io/github/license/ValUtils/ValStorage)

A helper module for managing paths and environments for the ValUtils project.

## Features

- Easy file system access
- Comprehensible API
- Default settings from dataclass_json

## Installation

The preferred method of installation is through `pip` but if you know better use the package manager that you want.

```sh
pip install ValStorage
```

## Reference

ValStorage contains the following methods:

- `list_dir` to list a directory
- `save_to_drive` to save a string to the filesystem
- `read_from_drive` to read a string to the filesystem
- `json_write` to write data as json to the filesystem
- `json_read` to read data as json to the filesystem
- `utils_path` to get the path for `ValUtils`

And the special `get_settings` method explained [down below](#settings-api).

### Usage

#### Basic usage

```python
from ValStorage import json_read, json_write, utils_path

data = json_read(utils_path / "data.json")
data["time"] = 100
json_write(data, utils_path / "data.json")
```

#### Magic usage

In this use case we export all the functionality as a local module and also our local `settingsPath` referencing the module directory.

```python
from ValStorage import *


def set_path():
    global settingsPath
    utilsPath = utils_path()
    settingsPath = utilsPath / "test"
    create_path(settingsPath)


set_path()
```

#### Settings API

The `get_settings` method takes a dataclass_json and a path as parameters and it retrieves or the settings in said path or the default settings.
For that our dataclass needs to have default values, here's a quick example:

```python
from ValStorage import get_settings
from dataclass_json import DataClassJsonMixin
from dataclass import dataclass

@dataclass
class Data(DataClassJsonMixin):
    time: float = 0
    argument: string = "default"

get_settings(Data, "data.json")
```

If `data.json` doesn't exist it gets filled with the default data provided by our dataclass.
