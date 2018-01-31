### Python 2 转码问题

##### 问题：

运行以下程序：

```python
#!/usr/bin/env python

if __name__ == '__main__':
    chinese_character = u'中文'
    print chinese_character
```

报错：

> SyntaxError: Non-ASCII character '\xe4' in file /Users/xxx/demo.py on line 4, but no encoding declared; see http://python.org/dev/peps/pep-0263/ for details



##### 原因：

Python 2 默认用ASCII编码（encode），如果接受的赋值不是ASCII编码的就会报SyntaxError

以下程序同样报SyntaxError:

```Python
#!/usr/bin/env python

if __name__ == '__main__':
  	# \xe4 为"中文"的编码
    chinese_character = u'\xe4'
    print chinese_character
```



##### 解决方案：

在Python文件头指定编码格式为utf-8:

```Python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
if __name__ == '__main__':
    chinese_character = u'中文'
    print chinese_character
```

######运行下试试：

>/Users/xxx/demo.py
>中文
>
>Process finished with exit code 0

成功，很开心！但是由于程序里某些地方在传输资源的时候需要encode：

```Python
#!/usr/bin/env python
# -*- coding: utf-8 -*-


def test_non_ascii_unicode(src):
    print type(src)
    byte_accept = str(src)
    # method/func (byte_accept)
    return byte_accept


if __name__ == '__main__':
    chinese_character = u'中文'
    print test_non_ascii_unicode(chinese_character)

```



>Traceback (most recent call last):
>  File "/Users/xxx/demo.py", line 19, in <module>
>
>    print test_non_ascii_unicode(chinese_character)
>  File "/Users/xxx/demo.py", line 12, in test_non_ascii_unicode
>    byte_accept = str(src)
>UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)



很显然， 在encode是又使用ascci方式。只能处理8bit编码（128）超过了就报错。



##### 大招 - 改默认编码格式: sys.setdefaultencoding('utf8')

```Python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import sys

reload(sys)
sys.setdefaultencoding('utf8')


def test_non_ascii_unicode(src):
    print type(src)
    byte_accept = str(src)
    # method/func (byte_accept)
    return byte_accept


if __name__ == '__main__':
    chinese_character = u'中文'
    print test_non_ascii_unicode(chinese_character)
```



>/usr/bin/python/Users/xxx/demo.py
><type 'unicode'>
>中文
>
>Process finished with exit code 0

搞定！