---
type: docs
title: "Go语言基础"
weight: 2
---

## 设置 Go 环境

### 安装 Go

```bash
# 下载并安装Go (以macOS为例)
brew install go

# 验证安装
go version
```

其他系统安装方式请参考[官方文档](https://golang.org/doc/install)

### 理解 GOPATH 和 Go 模块

```bash
# 查看当前GOPATH
go env GOPATH

# 创建新模块
mkdir myproject
cd myproject
go mod init example.com/myproject
```

**Go 模块 vs 其他语言的包管理：**

- **Python**: pip + requirements.txt/Pipenv/Poetry
- **Java**: Maven/Gradle
- **C#**: NuGet

### IDE 设置和基本工具

推荐 IDE：

- VSCode + Go 扩展
- GoLand
- Vim/Emacs + Go 插件

常用工具：

- `go fmt`: 代码格式化
- `go vet`: 代码静态分析
- `golint`: 代码风格检查
- `go test`: 运行测试

## Go 语法基础

### 变量声明和类型

```go
// Go
var name string = "Gopher"
age := 5  // 类型推断
const pi = 3.14159
```

```python
# Python
name = "Pythonista"
age = 5
PI = 3.14159  # 约定俗成的常量
```

```java
// Java
String name = "Javanese";
int age = 5;
final double PI = 3.14159;
```

```java
// C#
string name = "CSharpie";
var age = 5;  // 类型推断
const double PI = 3.14159;
```

### 控制结构

#### 条件语句

```go
// Go
if x > 10 {
    fmt.Println("x大于10")
} else if x > 5 {
    fmt.Println("x大于5且小于等于10")
} else {
    fmt.Println("x小于等于5")
}

// Go的switch不需要break
switch os := runtime.GOOS; os {
case "darwin":
    fmt.Println("macOS")
case "linux":
    fmt.Println("Linux")
default:
    fmt.Printf("%s\n", os)
}
```

#### 循环

```go
// Go只有for循环
for i := 0; i < 10; i++ {
    fmt.Println(i)
}

// 类似while的for
for count < 10 {
    count++
}

// 无限循环
for {
    // 做某事
    if shouldBreak {
        break
    }
}

// 遍历切片
fruits := []string{"apple", "banana", "cherry"}
for i, fruit := range fruits {
    fmt.Printf("%d: %s\n", i, fruit)
}

// 遍历映射
scores := map[string]int{"Alice": 98, "Bob": 85}
for name, score := range scores {
    fmt.Printf("%s: %d\n", name, score)
}
```

### 函数和方法

```go
// Go函数
func add(a, b int) int {
    return a + b
}

// 多返回值
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, errors.New("除数不能为零")
    }
    return a / b, nil
}

// 方法（绑定到结构体）
type Rectangle struct {
    Width, Height float64
}

func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}
```

对比其他语言：

```python
# Python
def add(a, b):
    return a + b

# Python方法
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height
```

```java
// Java
public int add(int a, int b) {
    return a + b;
}

// Java方法
public class Rectangle {
    private double width;
    private double height;

    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }

    public double area() {
        return width * height;
    }
}
```

## 练习

1. 创建一个简单的 Go 程序，打印"Hello, Go!"并编译运行它
2. 编写一个函数，接受一个整数切片并返回其中所有偶数的和
3. 创建一个结构体表示一本书，包含标题、作者和页数，并为其添加一个方法来打印书籍信息

## 常见陷阱

- 未使用的变量和导入会导致编译错误（Python、Java 和 C#只会警告）
- 大写字母开头的标识符表示公开可访问（其他语言使用 explicit 关键字如 public）
- Go 没有类和继承，而是使用结构体和组合
- 没有 try/catch 异常处理，而是使用错误返回值
- 短变量声明`:=`只能在函数内部使用，不能用于全局变量
