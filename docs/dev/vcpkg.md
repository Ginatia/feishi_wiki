## 通过清单文件添加依赖项

添加 vcpkg.json 清单文件:

`vcpkg new --application`

添加依赖库:

`vcpkg add port libname`

执行cmake时添加选项:

`cmake -S . -B build -DCMAKE_TOOLCHAIN_FILE="<path to vcpkg>/vcpkg.cmake"`

或者在 项目顶层cmake的project之前添加变量设定:

`set(CMAKE_TOOLCHAIN_FILE "<path to vcpkg>")`


## 通过全局vcpkg添加依赖项

下载所需包

`vcpkg install libname`

在项目CMakeLists.txt 添加vcpkg路径

`set(CMAKE_TOOLCHAIN_FILE "<path to vcpkg>")`

注意若是Qt项目,若先使用Qt创建工程,
则在修改CmakeLists.txt 之后,需要重新构建项目,因为Qt识别此命令失败
且注意在Windows平台下,vcpkg都使用的是VC++ 编译器,注意不要与Qt库编译器对不上