# Go 语法基础

## 基础框架

package 标识文件

import 引用依赖

import 也可以引用项目下的其他 go 代码文件

```go
package main

import "fmt"

func main() {
	fmt.Println("Hello World")
}
```

## 变量

使用 `var name type` 声明变量

使用 `:=` 简化定义

```go
package main

import "fmt"

func main() {
	var n int = 1
	m := 2
	fmt.Println(n)
	fmt.Println(m)
}
```

## 分支选择

在 go 中的分支选择中，可以在分号前赋值变量

在循环中也有相同的语法设计

```go
package main

import "fmt"

func main() {
	if x := 10; x%2 == 0 {
		fmt.Println(x)
	}
}

```

## 循环

```go
package main

import "fmt"

func main() {
	for x := 0; x <= 10; x++ {
		fmt.Println(x)
	}
}

```

也可以不带语句

通过这样来模仿while或do-while

```go
package main

import "fmt"

func main() {
	x := 0
	for {
		fmt.Println(x)
		x++
		if x > 20 {
			break
		}
	}
}
```

## 结构体

在go中没有类，只有结构体。

方法通过函数与结构体的绑定实现。

字母小写的结构体与函数不支持导出，反之支持导出

```go
package main

import "fmt"

type Node struct {
	x int
	y int
}

func (node Node) Out() {
	fmt.Println(node.x, node.y)
}

func main() {
	node := Node{1, 2}
	node.Out()
}

```