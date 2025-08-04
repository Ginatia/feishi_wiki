## CPP包管理器: conan

## 编写 conanfile.txt

```
[requires]
poco/1.13.3

[generators]
CMakeDeps
CMakeToolchain

```

## 使用 conan 安装依赖

向 build 文件夹中安装依赖

```powershell
conan install . --output-folder=build --build=missing
```

## CMake 构建项目

```powershell
cmake --preset conan-default

or

cd build
cmake ..  -DCMAKE_TOOLCHAIN_FILE="conan_toolchain.cmake"



cmake --build . --config Release
```

