# Logging Utility Library

The role of a logging utility library should be to configure an existing logger (python standard logging, loguru, etc) with the following: 

- log structure
- where logs go

This proposed log-util solution uses [**loguru**](https://loguru.readthedocs.io/en/stable/overview.html) the primary logging library and then provides the following as utility: 

- Configuration helper method
- Example log structures (in the form of pydantic models that can be imported)
- Example handlers (in the form of python dictionary objects - could potentially define handlers as YML configs)

## Examples

Below are examples of using this utility library (these examples and more can be found in ``example.py``)

```python
from loguru import logger

import log_utils
import log_utils.handlers as log_handlers
import log_utils.models as log_models

model = log_models.DefaultLogFields()
log_utils.setup_logger(
    handlers=[log_handlers.FILE_HANDLER, log_handlers.CONSOLE_HANDLER],
    model=model,
)
logger.info("Test log")
```

Output: 
```
2025-12-16 10:49:39.814 | INFO     | __main__:main:17 - Test log | {'rig_id': 'MPE', 'comp_id': 'a', 'instance': 1}
```

## Contribution

Feel free to create a PR and contribute additional schemas and handlers to this utility library. 

- Handlers: these should be dictionaries where the keys are parameters for loguru [logger.add()](https://loguru.readthedocs.io/en/stable/api/logger.html#loguru._logger.Logger.add) function.
- Schemas: these should be pydantic models (representing log fields) and ensure that the model adheres to the ``LogStructure`` protocol defined in ``models/log_structure.py``. Basically it needs a ``patch`` function.