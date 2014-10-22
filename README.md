# AlphaDict

Recursively sorts dictionaries by key. 

Useful for determining if the hash of two dicts that contain identical data,
but are in slightly different orders, are the same.

Experienced this pain point when trying to quickly determine if a JSON object 
from the client had been previously cached by our search engine. 
The order of the JSON would vary slightly, but the desired data could often be the same.

Check out some of the examples in the unit tests:

```bash
python -m unittest discover
```
