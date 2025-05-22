# Python defaultdict 详解

> [!info] 简介 `defaultdict` 是 Python 标准库 `collections` 模块中的一个数据结构，它是内置 `dict` 类型的子类，但添加了处理缺失键的特殊功能。

## 基本概念

`defaultdict` 最大的特点是提供了一种自动处理访问不存在键的机制。当你试图访问字典中不存在的键时，`defaultdict` 会自动为该键创建一个值，这个值由你在创建 `defaultdict` 时提供的"默认工厂函数"(default_factory)生成。

> [!tip] 语法
> 
> ```python
> from collections import defaultdict
> 
> # 创建一个默认值为特定类型的defaultdict
> d = defaultdict(default_factory)
> ```

## 常用默认工厂函数

- `int`: 默认值为 0
- `list`: 默认值为空列表 `[]`
- `set`: 默认值为空集合 `set()`
- `str`: 默认值为空字符串 `''`
- `dict`: 默认值为空字典 `{}`
- 自定义函数: 返回任何你想要的默认值

## 与普通字典的对比

### 普通字典处理缺失键

```python
regular_dict = {}

# 方法1: 使用if语句检查
if 'key' in regular_dict:
    regular_dict['key'] += 1
else:
    regular_dict['key'] = 1

# 方法2: 使用get()方法
regular_dict['key'] = regular_dict.get('key', 0) + 1

# 方法3: 使用setdefault()方法
regular_dict.setdefault('key', 0)
regular_dict['key'] += 1
```

### defaultdict处理缺失键

```python
from collections import defaultdict

# 创建一个默认值为0的字典
count_dict = defaultdict(int)

# 直接增加计数，无需检查键是否存在
count_dict['key'] += 1
```

## 常见应用场景

### 1. 计数统计

```python
from collections import defaultdict

# 统计每个单词出现的次数
words = ['apple', 'banana', 'apple', 'orange', 'banana', 'apple']
word_counts = defaultdict(int)

for word in words:
    word_counts[word] += 1

print(dict(word_counts))  # {'apple': 3, 'banana': 2, 'orange': 1}
```

> [!note] 计数场景 使用 `defaultdict(int)` 是最常见的统计频率场景，默认值为0，每次出现就加1。

### 2. 分组数据

```python
from collections import defaultdict

# 按首字母分组单词
words = ['apple', 'banana', 'avocado', 'orange', 'blueberry']
grouped_words = defaultdict(list)

for word in words:
    grouped_words[word[0]].append(word)

print(dict(grouped_words))  
# {'a': ['apple', 'avocado'], 'b': ['banana', 'blueberry'], 'o': ['orange']}
```

### 3. 嵌套的defaultdict

```python
from collections import defaultdict

# 创建嵌套的defaultdict结构
nested = defaultdict(lambda: defaultdict(list))

# 添加数据
nested['fruit']['red'].append('apple')
nested['fruit']['yellow'].append('banana')
nested['vegetable']['green'].append('spinach')

print(dict(nested))  
# {'fruit': {'red': ['apple'], 'yellow': ['banana']}, 'vegetable': {'green': ['spinach']}}
```

### 4. 处理时间序列数据

```python
from collections import defaultdict
from datetime import datetime

# 示例数据
data = [
    {"value": 10, "timestamp": "2025-03-11 10:31:49"},
    {"value": 15, "timestamp": "2025-03-11 10:31:49"},
    {"value": 20, "timestamp": "2025-03-11 10:31:50"}
]

# 按秒分组
time_series = defaultdict(list)

for item in data:
    # 获取时间部分
    time_key = item["timestamp"].split(" ")[1]
    time_series[time_key].append(item["value"])

# 结果: {'10:31:49': [10, 15], '10:31:50': [20]}
```

## 高级用法

### 自定义默认工厂函数

```python
from collections import defaultdict

def default_value():
    return "Not Found"

d = defaultdict(default_value)
print(d["missing"])  # 输出: Not Found
```

### 使用lambda函数

```python
from collections import defaultdict

# 默认值为一个包含单个元素0的列表
d = defaultdict(lambda: [0])
d['key'].append(1)
print(d['key'])  # 输出: [0, 1]
```

### 使用class方法作为默认工厂

```python
from collections import defaultdict

class Counter:
    def __init__(self):
        self.count = 0
    
    def increment(self):
        self.count += 1
        return self.count

d = defaultdict(Counter)
print(d['a'].increment())  # 输出: 1
print(d['a'].increment())  # 输出: 2
print(d['b'].increment())  # 输出: 1
```

## 性能考虑

> [!warning] 性能说明
> 
> - `defaultdict` 在处理大量缺失键时比普通字典更高效
> - 减少了条件检查和异常处理的开销
> - 对于特定的场景（如计数、分组），代码更简洁且性能更好

## 注意事项

> [!caution] 注意
> 
> 1. `defaultdict` 会自动为任何不存在的键创建默认值，这可能会导致意外的内存使用
> 2. 当将 `defaultdict` 转换为普通字典时，所有生成的默认值都会被保留
> 3. 默认工厂函数不应有副作用，因为它可能会被多次调用
> 4. `defaultdict` 在序列化(如pickle)时需要特别注意

## 与其他数据结构对比

|数据结构|特点|适用场景|
|---|---|---|
|`dict`|基本的键值存储|一般用途|
|`defaultdict`|自动处理缺失键|频率统计、分组|
|`Counter`|专为计数设计|元素计数|
|`OrderedDict`|保持键的插入顺序|需要有序的字典|

## 总结

`defaultdict` 是处理缺失键的强大工具，尤其适合:

- 计数统计
- 数据分组
- 构建多层次数据结构
- 简化代码逻辑

它通过消除对缺失键的显式检查，使代码更简洁、更易读，并在某些情况下提供更好的性能。

---

## 相关链接

- [[Python Collections模块]]
- [[Python字典进阶]]
- [[数据结构选择指南]]