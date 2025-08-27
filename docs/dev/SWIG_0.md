## 创建C++ API及SWIG接口文件

### demo.h

```cpp
#pragma once

int mul(int a,int b);
```

### demo.cpp

```cpp
#include "demo.h"

int mul(int a,int b){
    int ans = 0;
    while(b){
        if(b&1){
            ans += a;
        }
        a += a;
        b >>= 1;
    }
    return ans;
}
```

### demo.i

```swig
%module demo
%{
#include "demo.h"    
%}

%include "demo.h"
```


## 使用Python的setuptools 打包

```python
from setuptools import setup,Extension
from setuptools.command.build_ext import build_ext

import os
import sys

demo_module = Extension(
    name="_demo",
    sources=["demo.i","demo.cpp"],
    swig_opts=["-c++"],
    include_dirs=["."],
)

setup(
    name="demo",
    version="1.0",
    author="Feishi",
    description="SWIG+demo",
    ext_modules=[demo_module],
    py_modules=["demo"],
)
```

```powershell
python setup.py build_ext --inplace
```

### 存根文件

### demo.pyi

```pyi
def mul(a:int,b:int)->int:...
```


