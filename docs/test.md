# Unit testing
* **Reading assertion** failures and stack traces to locate the source of an error
* **Writing assert** statements and unit test functions to ensure correct behavior

```Python
#!python

from set import Set
import unittest

class SetTest(unittest.TestCase):
    def test_remove(self):
        s = Set(['Ab', 'Bc', 'C'])
        assert s.length() == 3
        assert s.size == 3
        s.remove('Ab')
        assert s.length() == 2  # should have removed 'A'
        assert s.size == 2
        with self.assertRaises(KeyError):
            s.remove('Ab') # not in set

if __name__ == '__main__':
    unittest.main()
```
