# Confident
[![PyPI - Python Version](https://img.shields.io/pypi/pyversions/confident?style=plastic)](https://github.com/limonyellow/confident-v0.2)
[![PyPI](https://img.shields.io/pypi/v/confident?style=plastic&color=%2334D058)](https://pypi.org/project/confident/)
[![GitHub Workflow Status (branch)](https://img.shields.io/github/workflow/status/limonyellow/confident-v0.2/Python%20package/main?style=plastic)](https://github.com/limonyellow/confident-v0.2/actions)
[![GitHub](https://img.shields.io/github/license/limonyellow/confident-v0.2?style=plastic)](https://github.com/limonyellow/confident-v0.2)
[![Coverage](https://codecov.io/gh/limonyellow/confident-v0.2/branch/main/graph/badge.svg?token=L161UYE2RM)](https://codecov.io/gh/limonyellow/confident-v0.2)
---

[**Documentation**](https://limonyellow.github.io/confident-v0.2/)

---

Confident helps you create configuration objects from multiple sources such as files, environment variables and maps.  
Confident BaseConfig class is a data model that enforce validation and type hints by using [pydantic](https://pydantic-docs.helpmanual.io/) library.

With Confident you can manage multiple configurations depends on the environment your code is deployed.
While having lots of flexibility how to describe your config objects, Confident will provide visibility of the process 
and help you expose misconfiguration as soon as possible.


## Example

```python
import os

from confident import BaseConfig


# Creating your own config class by inheriting from `BaseConfig`.
class MyAppConfig(BaseConfig):
    port: int = 5000
    host: str = 'localhost'
    labels: list


# Illustrates some environment variables.
os.environ['host'] = '127.0.0.1'
os.environ['labels'] = '["FOO", "BAR"]'  # JSON strings can be used for more types.

# Creating the config object. `BaseConfig` will load the values of the properties.
config = MyAppConfig()

print(config.host)
# > 127.0.0.1
print(config.json())
# > {"port": 5000, "host": "127.0.0.1", "labels": ["FOO", "BAR"]}
print(config)
# > port=5000 host='127.0.0.1' labels=['FOO', 'BAR']
print(config.full_fields())
# > {
# 'port': ConfigField(name='port', value=5000, origin_value=5000, source_name='MyAppConfig', source_type='class_default', source_location=PosixPath('~/confident/readme_example.py')),
# 'host': ConfigField(name='host', value='127.0.0.1', origin_value='127.0.0.1', source_name='host', source_type='env_var', source_location='host'),
# 'labels': ConfigField(name='labels', value=['FOO', 'BAR'], origin_value='["FOO", "BAR"]', source_name='labels', source_type='env_var', source_location='labels')
# }

```

## Installation
```shell
(.venv) $ pip install confident
```

## Capabilities
### Customized Fields Loaders
Built-in loaders:  
- Environment variables.  
- Config files such as 'json' and 'yaml'.  
- Config maps to load fields depends on the environment. (See documentation)  

It is possible to configure the loading priority and add your own loader classes.

### Full Support of Pydantic BaseSettings
Confident core functionality is based on [pydantic](https://pydantic-docs.helpmanual.io/) library. 
That means `BaseConfig` object has all the benefits of pydantic's [`BaseModel`](https://pydantic-docs.helpmanual.io/usage/models/) 
and [`BaseSettings`](https://pydantic-docs.helpmanual.io/usage/settings/)
including type validation, [object transformation](https://pydantic-docs.helpmanual.io/usage/exporting_models/) and many more features.

### Config Loading visibility
`BaseConfig` object stores details about the fields loading process and offers ways to understand the source of each loaded field.
Details about the origin value (before conversion), the location of the source and the type of loader, can all be accessed from the object. 

## Examples
More examples can be found in the project's [repository](https://github.com/limonyellow/confident-v0.2).

## Contributing
To contribute to Confident, please make sure any new features or changes to existing functionality include test coverage.
