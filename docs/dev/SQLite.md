# SQLite 数据库

## 使用命令行创建数据库

```
sqlite3 test.db
```

## CMake
```
find_package(SQLiteCpp CONFIG REQUIRED)
target_link_libraries(main PRIVATE SQLiteCpp)
```




