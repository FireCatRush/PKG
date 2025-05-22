# Python 中的 `@property`

`@property` 是 Python 提供的一个内置装饰器，通常用于定义**只读属性**，或者在不改变外部代码的前提下，将方法转换为属性访问的方式。

## **主要作用**

1. **使方法像属性一样被访问**  
    允许使用 `obj.attr` 而不是 `obj.method()`，从而提高代码的可读性。
2. **控制属性的访问和修改**  
    通过 `@property` 让属性可以被访问，同时结合 `@setter` 和 `@deleter` 控制修改和删除行为。
3. **提供封装性**  
    可以在不改变外部 API 的情况下，增加额外的逻辑，比如数据验证、懒加载等。

```python
class Circle:
    def __init__(self, radius):
        self._radius = radius  # 使用单下划线表示"私有"属性

    @property
    def radius(self):
        """定义 radius 作为只读属性"""
        return self._radius

    @radius.setter
    def radius(self, value):
        """设置属性时进行校验"""
        if value < 0:
            raise ValueError("半径不能为负数")
        self._radius = value

    @radius.deleter
    def radius(self):
        """删除属性时的行为"""
        print("半径属性已删除")
        del self._radius

# 测试
c = Circle(5)
print(c.radius)  # 5（像访问属性一样访问方法）

c.radius = 10    # 调用 @setter 方法
print(c.radius)  # 10

del c.radius     # 调用 @deleter 方法

```

---
# **`@property` 的核心逻辑**

1. **`@property`**
    
    - 把一个方法转换为**只读属性**，外部可以 `obj.attr` 访问，但不能 `obj.attr = value` 直接修改。
2. **`@property_name.setter`**
    
    - 允许修改该属性，并添加**数据校验**等逻辑。
3. **`@property_name.deleter`**
    
    - 允许删除该属性，并添加**删除逻辑**。

---

# **`@property` 的应用场景**

## **1. 延迟计算（懒加载）**

```python
class Rectangle:
    def __init__(self, width, height):
        self._width = width
        self._height = height

    @property
    def area(self):
        """计算面积，避免存储冗余数据"""
        return self._width * self._height

r = Rectangle(4, 5)
print(r.area)  # 20

```

> **这里 `area` 只是通过 `@property` 计算值，并不会存储实际的 `area` 变量**。

---

## **2. 保护内部变量**

```python
class Person:
    def __init__(self, name):
        self._name = name  # 约定为私有变量

    @property
    def name(self):
        return self._name

p = Person("Alice")
print(p.name)  # 访问 name，但不能修改
# p.name = "Bob"  # AttributeError: can't set attribute

```

> 由于 `@property` 只定义了 getter，但没有 `@name.setter`，所以 `name` 是**只读属性**。

---

## **3. 结合 `setter` 进行输入校验**

```python
class Student:
    def __init__(self, score):
        self._score = score

    @property
    def score(self):
        return self._score

    @score.setter
    def score(self, value):
        if not 0 <= value <= 100:
            raise ValueError("分数必须在 0 到 100 之间")
        self._score = value

s = Student(80)
s.score = 95  # 正确
print(s.score)  # 95
# s.score = 150  # ValueError: 分数必须在 0 到 100 之间

```

> `@setter` 让 `score` 变成可修改的属性，并且确保数值在合理范围内。

---

# **`@property` VS 传统 getter/setter**

在 Java、C++ 等语言中，通常使用 `get_xxx()` 和 `set_xxx(value)` 来访问和修改对象属性，而 Python 提供了 `@property` 语法，使代码更加**简洁、优雅、符合 Pythonic 风格**。

**对比**

```python
# Java/C++风格（传统方法）
class Person:
    def __init__(self, age):
        self._age = age

    def get_age(self):
        return self._age

    def set_age(self, value):
        if value < 0:
            raise ValueError("年龄不能为负数")
        self._age = value

p = Person(30)
p.set_age(25)
print(p.get_age())  # 25

```

```python
# Pythonic 风格（@property）
class Person:
    def __init__(self, age):
        self._age = age

    @property
    def age(self):
        return self._age

    @age.setter
    def age(self, value):
        if value < 0:
            raise ValueError("年龄不能为负数")
        self._age = value

p = Person(30)
p.age = 25  # 直接当作属性赋值
print(p.age)  # 25

```

> **区别：**
> 
> - `p.age = 25` 直接赋值，而不是 `p.set_age(25)`
> - `p.age` 直接访问，而不是 `p.get_age()`
> - **`@property` 使 API 更加直观，隐藏了内部实现细节**

---

# **`@property` 的底层原理**

`@property` 其实是 `property()` 函数的语法糖：

```python
class Person:
    def __init__(self, age):
        self._age = age

    def get_age(self):
        return self._age

    def set_age(self, value):
        if value < 0:
            raise ValueError("年龄不能为负数")
        self._age = value

    age = property(get_age, set_age)  # 用 property() 手动创建属性

p = Person(30)
p.age = 25
print(p.age)  # 25

```

等价于：

```python
class Person:
    def __init__(self, age):
        self._age = age

    @property
    def age(self):
        return self._age

    @age.setter
    def age(self, value):
        if value < 0:
            raise ValueError("年龄不能为负数")
        self._age = value

```

> **`property(fget, fset, fdel, doc)` 是一个内置函数，它和 `@property` 机制类似。**