# AlphaDict

Recursively sorts dictionaries by key. 

Useful for determining if the hash of two dicts that contain identical data,
but are in slightly different orders, are the same.

Experienced this pain point when trying to quickly determine if a JSON object 
from the client had been previously cached by our search engine. 
The order of the JSON would vary slightly, but the desired data could often be the same.

```python
from collections import OrderedDict
from hashlib import md5

data_one = OrderedDict((
    ('a', [
        OrderedDict((
            ('c', 3), 
            ('a', 1), 
            ('b', 2), 
        )), 
    ]), 
))  

data_two = json.loads(request.body, object_pairs_hook=collections.OrderedDict)
"""
Somehow, the front-end sent us this for data_two:
OrderedDict((
    ('a', [
        OrderedDict((
            ('a', 1), 
            ('c', 3), # 'a' and 'c' swapped...
            ('b', 2),
        )), 
    ]), 
))  
"""

# fails, even though they're technically the same query
self.assertEqual(data_one, data_two)
```

Let's correct this with `AlphaDict`:

```python
a = AlphaDict(data_one)
b = AlphaDict(data_two)

# passes
self.assertEqual(a, b)

hash_a = md5(str(a)).hexdigest()
hash_b = md5(str(b)).hexdigest()

# passes
self.assertEqual(hash_a, hash_b)
```

Check out some of the examples in the unit tests:

```bash
python -m unittest discover
```
