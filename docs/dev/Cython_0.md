## Cython 接口

```cython
cdef double f(double x):
    return x ** 2 - x

cpdef double integrate_f(double a,double b,int N):
    cdef int i
    cdef double s
    cdef double dx
    s = 0
    dx = (b - a) / N
    for i in range(N):
        s += f(a + i * dx)
    return s * dx
```

## setup.py

创建 setup.py ，它类似于 Python 的 Makefile。

```python
from setuptools import setup
from Cython.Build import cythonize

setup(
    name='func',
    ext_modules=cythonize("func.pyx"),
)
```

### 构建Cython文件

```powershell
python setup.py build_ext --inplace
```

执行命令，即创建文件，可被python常规方法导入。

### pyi 文件

为 Python 代码提供类型信息，方便 类型检查工具（如 mypy、pyright、pylance） 和 IDE（如 VSCode、PyCharm） 在静态分析时使用。

```python
def integrate_f(a: float, b: float, N: int) -> float: ...
```

### 调用模块

```python
import func

print(func.integrate_f(1,2,10))
```

## 调用 C 函数

使用 `stdlib.h` 的 `atoi()` 函数
