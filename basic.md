### 基础

```python
def convert_to_unicode(str_source):
  return str_source.decode('unicode-escape')

def convert_to_unicode2(str_source):
  return u''.join(str_source)
```

