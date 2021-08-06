几种常用类型：
1. defaultdict
2. OrderedDict
3. deque
4. ChainMap
5. Counter

## defaultdict

在普通的dict(字典)之上添加了default\_factory, default\_factory参数可以指定成list, set, int等各种合法类型。（通俗点说，就是每个元素的值可以是 list, set, int）

list:
{'blue': \[2, 4, 4\], 'red': \[1, 3, 1\]}

set:
{'blue': {2, 4}, 'red': {1, 3}}

int:
{'o': 2, 'h': 1, 'w': 1, 'l': 3, ' ': 1, 'd': 1, 'e': 1, 'r': 1}

---
## OrderedDict

说一下和普通的字典的区别：
- OrderedDict是按照元素被添加的顺序进行排列的，而普通字典
是按哈希来存储的
- 如果OrderedDict的两个对象含有相同的元素，但内部排列顺序不同，则认为二者不相等。但对于普通的字典，它会认为二者是相等的。

[参考链接](https://www.cnblogs.com/notzy/p/9312049.html)

d=collections.OrderedDict() 初始化略

**popitem**

d.popitem(last=True): 删除最后一个key-value对，last=True也可以不写
d.popitem(last=False): 删除第一个key-value对

**move_to_end**

d.move_to_end(key, last=True): 将key的键值对移动到末尾，last=True也可以不写
d.move_to_end(key, last=False): 将key的键值对移动到开头

---
## deque

双向list，可以在开头和末尾 新增 / 添加 元素

d=collections.deque("xxx")

d.append('j')
d.dppendleft('j')

d.pop()
d.popleft()

---
## ChainMap

实际上就是对多个字典进行合并

**eg**
a = {"x":1, "z":3}
b = {"y":2, "z":4}

c = ChainMap(a, b)
c:  ChainMap( {"x":1, "z":3}, {"y":2, "z":4})
其它删除查找功能日后按需补充。

[可参考](https://www.cnblogs.com/mangmangbiluo/p/9882097.html)

---
## Counter

用来按照元素的出现此处进行计数，可参考[[169.多数元素]]

**most_common用来返回出现最多的数**

```python
c=Counter(nums)
#返回出现最多的n个值，此处n为1
res=c.most_common(1)
```

