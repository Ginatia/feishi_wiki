
## 创建控制台应用程序

```pwsh
dotnet new console -n MyApp
```

## 创建类库（Library）项目

```pwsh
dotnet new classlib -n MyLibrary
```

## 查看可用项目

根据dotnet列出的可用项目，进行搭建项目。

```pwsh
dotnet new --list
```

## 程序执行入口 Main

```cs
class TestClass { 
    static void Main(string[] args) {
        System.Console.WriteLine("Hello, World! FeiShi");
    }
}
```

## Class

```cs
public class Type
{
    public int x { get; set; }
    public int y { get; set; }

    public int f() {
        return x * y + x + y;
    }
}
```

## record

定义一个数据模型，满足：
1. 值相等
2. 对象不可变

```cs
public record Persion(string name,int age);
```

### 如何使用 with 表达式复制不可变对象并更改其中一个属性

```cs
Persion p = new Persion("FeiShi", 18);
System.Console.WriteLine($"Persion: {p.name}, {p.age}");

Persion p2 = p with { age = 22 };
System.Console.WriteLine($"Persion2: {p2.name}, {p2.age}");
```

## interface ：定义多种类型的行为

按照约定，接口名称以大写字母 `I` 开头。

```cs
interface IEquatable<T>
{
    bool Equals(T obj);
}

public class Type: IEquatable<Type>
{
    public int x { get; set; }
    public int y { get; set; }

    public int f() {
        return x * y + x + y;
    }

    public bool Equals(Type? type)
    {
        return (this.x, this.y) == (type?.x, type?.y);
    }
}
```

此处的 `Type? type` 表示一个可空的 `Type` 类型，既可以存储一个 `Type` 类型的值，也可以为 `null`

## 泛型

```cs
public class GType<T>
{
    public required T value { get; set; }
    public void Add (T item)
    {
        this.value = item;
    }
}
class TestMain 
{ 
    static void Main(string[] args) 
    {
        GType<int> g = new GType<int>() { value = 1 };
        System.Console.WriteLine($"GType<int> value: {g.value}");

        GType<string> g2 = new GType<string>() { value = "string 1" };
        System.Console.WriteLine($"GType<string> value: {g2.value}");
    }
}
```
