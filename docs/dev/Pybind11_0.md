
## 构建项目

```cmake
cmake_minimum_required(VERSION 3.10)

#set(CMAKE_TOOLCHAIN_FILE "D:/vcpkg/scripts/buildsystems/vcpkg.cmake")

project(demo_cpp)

find_package(Python REQUIRED COMPONENTS Interpreter Development)

add_subdirectory(extern/pybind11)

add_executable(test_calc main.cpp)

target_link_libraries(test_calc PRIVATE pybind11::embed)
```

