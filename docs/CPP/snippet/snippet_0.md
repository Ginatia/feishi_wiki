#### 可变参数模板+折叠表达式

**读入**

```cpp
template<typename ...Args>
void read(Args& ...args){
    (std::cin>>...>>args);
}
```

**输出(空格隔开)**

```cpp
template<typename ...Args>
void print(Args& ...args){
    ((std::cout<<args<<" "),...);
    std::cout<<"\n";
}
```