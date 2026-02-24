# v2.4: 
random.randint(_low_, _high=None_, _size=None_, _dtype=int_)

Return random integers from _low_ (inclusive) to _high_ (exclusive).
	Return random integers from the “discrete uniform” distribution of the specified dtype in the “half-open” interval \[_low_, _high_). If _high_ is None (the default), then results are from \[0, _low_).

Note:
	New code should use the [`integers`](https://numpy.org/doc/stable/reference/random/generated/numpy.random.Generator.integers.html#numpy.random.Generator.integers "numpy.random.Generator.integers") method of a [`Generator`](https://numpy.org/doc/stable/reference/random/generator.html#numpy.random.Generator "numpy.random.Generator") instance instead; please see the [Quick start](https://numpy.org/doc/stable/reference/random/index.html#random-quick-start).


Parameters:
	**low**: int or array-like of ints
	**high**: int or array-like of ints, optional
	**size**: int or tuple of ints, optional
	**dtype**: dtype, optional
	
This function defaults to the C-long dtype, which is 32bit on windows and otherwise 64bit on 64bit platforms (and 32bit on 32bit ones). Since NumPy 2.0, NumPy’s default integer is 32bit on 32bit platforms and 64bit on 64bit platforms. Which corresponds to _np.intp_. (_dtype=int_ is not the same as in most NumPy functions.)

这段话指的是NumPy 2.0 之前，在 Windows 上，`dtype=int` 和许多默认整数使用的是 C `long`（32 位）；NumPy 2.0 起，NumPy 的“默认整数语义”改为 `np.intp`，因此在 64 位 Windows 上默认整数变为 64 位，但 NumPy 从始至终都支持 64 位整数类型，可以通过显示指定来使用64位整数类型。


```python
import numpy as np  
import platform, sys  
  
print("platform:", platform.platform())  
print("python:", sys.version.split()[0])  
print("np.version:", np.__version__)  
  
print("np.dtype(int):", np.dtype(int), "itemsize:", np.dtype(int).itemsize)  
print("np.int_     :", np.dtype(np.int_), "itemsize:", np.dtype(np.int_).itemsize)  
print("np.intp     :", np.dtype(np.intp), "itemsize:", np.dtype(np.intp).itemsize)  
print("np.int64    :", np.dtype(np.int64), "itemsize:", np.dtype(np.int64).itemsize)
```
下面的运行结果分别为
```shell
platform: Windows-10-10.0.26200-SP0
python: 3.9.7
np.version: 1.20.3
np.dtype(int): int32 itemsize: 4
np.int_     : int32 itemsize: 4
np.intp     : int64 itemsize: 8
np.int64    : int64 itemsize: 8
```
和
```shell
python: 3.12.10
np.version: 2.4.2
np.dtype(int): int64 itemsize: 8
np.int_     : int64 itemsize: 8
np.intp     : int64 itemsize: 8
np.int64    : int64 itemsize: 8
```

---
该方法也支持广播机制：
```python
np.random.randint([1, 3, 5, 7], [[10], [20]], dtype=np.uint8)
```

```text
array([[ 8,  6,  9,  7], # random
       [ 1, 16,  9, 12]], dtype=uint8)
```