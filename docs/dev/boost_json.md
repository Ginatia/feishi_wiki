
## vcpkg 提供的cmake链接方式

```
The package boost-json is compatible with built-in CMake targets of FindBoost.cmake:

    find_package(Boost REQUIRED COMPONENTS json)
    target_link_libraries(main PRIVATE Boost::json)

or the generated cmake configs via:

    find_package(boost_json REQUIRED CONFIG)
    target_link_libraries(main PRIVATE Boost::json)
```

## 存放json数据的相关类型

| 类型 | 描述 |
| --- | --- |
| array | JSON 值的序列容器，支持动态大小和快速随机访问。其接口和性能特征与 std :: vector 类似。 |
| object | 一个包含具有唯一键的键值对的关联容器，其中键为字符串，映射类型为 JSON 值。搜索、插入和删除操作具有平均时间复杂度。此外，元素在内存中连续存储，从而允许缓存友好的迭代。|
| string | 连续的字符范围。该库假定字符串内容仅包含有效的 UTF-8 编码。|
| value | 一种特殊变体，可以保存六种标准 JSON 数据类型中的任意一种。|



