## execute

```powershell
lua .\test_1.lua
```

## function

lua 使用 `function` 标识函数
使用 `end` 标识结束

```lua
function f(n)
    if n == 0 then
        return 0
    else
        return n + f(n-1)
    end    
end
```



## Types And Values

注意到函数是一等对象。

| Type     | Value   |
| -------- | ------- |
| string   | "Hello" |
| number   | 10.0    |
| boolean  | true    |
| nil      | nil     |
| function | print   |
| thread   |         |
| table    |         |
| userdata |         |

## Tables

```lua
a = {}
k = "x"

a[k] = 10

print(a[k])

k = 20
a[k] = "hello"

print(a[k])

days = {"Sunday", "Monday", "Tuesday", "Wednesday",
            "Thursday", "Friday", "Saturday"}

print(days[1])
```

## Relational Operators

```lua
a = 1
b = 2

a == 1
b ~= 1
a < b
b > a
a <= 1
a >= 0
```

## if

```lua
x = 1

if x ~= 2 then
    print("x not equal 2")
elseif x == 2 then
    print("x equal 2")
else
    error("x error")
end
```

## while

```lua
local i = 1
while i < 10 do
    i = i + 1
end

print(i)
```

## repeat

```lua
local x = 1

repeat
    x = x + 1
until x >= 10

print(x)
```

