---
type: docs
title: "标准库和生态系统"
weight: 6
---

## 基本标准库包

### fmt包

```go
// Go的fmt包
fmt.Println("Hello, World!")  // 打印并换行
fmt.Printf("值: %v, 类型: %T\n", 42, 42)  // 格式化打印

var name = "Gopher"
var age = 10
fmt.Sprintf("名字: %s, 年龄: %d", name, age)  // 返回格式化字符串

// 从标准输入读取
var input string
fmt.Scanln(&input)  // 读取一行
```

对比其他语言：

```python
# Python的打印和格式化
print("Hello, World!")  # 打印并换行
print(f"值: {42}, 类型: {type(42)}")  # f-string格式化 (Python 3.6+)

name = "Pythonista"
age = 10
s = f"名字: {name}, 年龄: {age}"  # 格式化字符串

# 从标准输入读取
input_text = input()  # 读取一行
```

```java
// Java的打印和格式化
System.out.println("Hello, World!");  // 打印并换行
System.out.printf("值: %s, 类型: %s%n", 42, Integer.class.getSimpleName());  // 格式化打印

String name = "Javanese";
int age = 10;
String s = String.format("名字: %s, 年龄: %d", name, age);  // 格式化字符串

// 从标准输入读取
Scanner scanner = new Scanner(System.in);
String inputText = scanner.nextLine();  // 读取一行
```

```csharp
// C#的打印和格式化
Console.WriteLine("Hello, World!");  // 打印并换行
Console.WriteLine($"值: {42}, 类型: {42.GetType().Name}");  // 字符串插值

string name = "CSharpie";
int age = 10;
string s = $"名字: {name}, 年龄: {age}";  // 格式化字符串

// 从标准输入读取
string inputText = Console.ReadLine();  // 读取一行
```

### io和os包

```go
// Go的文件操作

// 读取文件
data, err := os.ReadFile("file.txt")
if err != nil {
    log.Fatal(err)
}
fmt.Println(string(data))

// 写入文件
err = os.WriteFile("output.txt", []byte("Hello, Go!"), 0644)
if err != nil {
    log.Fatal(err)
}

// 使用io.Reader和io.Writer接口
file, err := os.Open("file.txt")
if err != nil {
    log.Fatal(err)
}
defer file.Close()

buf := make([]byte, 1024)
n, err := file.Read(buf)
if err != nil && err != io.EOF {
    log.Fatal(err)
}
fmt.Println(string(buf[:n]))
```

对比其他语言：

```python
# Python的文件操作

# 读取文件
with open("file.txt", "r") as f:
    data = f.read()
print(data)

# 写入文件
with open("output.txt", "w") as f:
    f.write("Hello, Python!")

# 使用二进制模式
with open("file.bin", "rb") as f:
    binary_data = f.read(1024)
```

```java
// Java的文件操作

// 读取文件 (Java 11+)
String data = Files.readString(Path.of("file.txt"));
System.out.println(data);

// 写入文件
Files.writeString(Path.of("output.txt"), "Hello, Java!");

// 使用流
try (InputStream is = new FileInputStream("file.txt")) {
    byte[] buffer = new byte[1024];
    int n = is.read(buffer);
    System.out.println(new String(buffer, 0, n));
} catch (IOException e) {
    e.printStackTrace();
}
```

```csharp
// C#的文件操作

// 读取文件
string data = File.ReadAllText("file.txt");
Console.WriteLine(data);

// 写入文件
File.WriteAllText("output.txt", "Hello, C#!");

// 使用流
using (FileStream fs = File.OpenRead("file.txt"))
{
    byte[] buffer = new byte[1024];
    int n = fs.Read(buffer, 0, buffer.Length);
    Console.WriteLine(Encoding.UTF8.GetString(buffer, 0, n));
}
```

### net/http包

```go
// Go的HTTP客户端和服务器

// HTTP客户端
resp, err := http.Get("https://example.com")
if err != nil {
    log.Fatal(err)
}
defer resp.Body.Close()

body, err := io.ReadAll(resp.Body)
if err != nil {
    log.Fatal(err)
}
fmt.Println(string(body))

// HTTP服务器
http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Hello, %s!", r.URL.Path[1:])
})

log.Println("服务器启动在 http://localhost:8080")
log.Fatal(http.ListenAndServe(":8080", nil))
```

对比其他语言：

```python
# Python的HTTP客户端和服务器
import requests
from flask import Flask

# HTTP客户端 (使用requests库)
resp = requests.get("https://example.com")
print(resp.text)

# HTTP服务器 (使用Flask)
app = Flask(__name__)

@app.route('/<name>')
def hello(name):
    return f"Hello, {name}!"

if __name__ == '__main__':
    print("服务器启动在 http://localhost:5000")
    app.run(host='0.0.0.0', port=5000)
```

```java
// Java的HTTP客户端和服务器

// HTTP客户端 (Java 11+)
HttpClient client = HttpClient.newHttpClient();
HttpRequest request = HttpRequest.newBuilder()
        .uri(URI.create("https://example.com"))
        .build();
HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
System.out.println(response.body());

// HTTP服务器 (使用Spring Boot)
@RestController
public class HelloController {
    @GetMapping("/{name}")
    public String hello(@PathVariable String name) {
        return "Hello, " + name + "!";
    }
}

// 主应用类
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
        System.out.println("服务器启动在 http://localhost:8080");
    }
}
```

```csharp
// C#的HTTP客户端和服务器

// HTTP客户端
using HttpClient client = new HttpClient();
string response = await client.GetStringAsync("https://example.com");
Console.WriteLine(response);

// HTTP服务器 (使用ASP.NET Core)
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.UseRouting();
        app.UseEndpoints(endpoints =>
        {
            endpoints.MapGet("/{name}", async context =>
            {
                var name = context.Request.RouteValues["name"];
                await context.Response.WriteAsync($"Hello, {name}!");
            });
        });
    }
}

// 主程序
public static void Main(string[] args)
{
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        })
        .Build()
        .Run();
    Console.WriteLine("服务器启动在 http://localhost:5000");
}
```

## Go中的测试

### testing包

```go
// Go的测试 - 保存为 math_test.go
package math

import "testing"

// 被测试的函数
func Add(a, b int) int {
    return a + b
}

// 单元测试
func TestAdd(t *testing.T) {
    got := Add(2, 3)
    want := 5
    if got != want {
        t.Errorf("Add(2, 3) = %d; want %d", got, want)
    }
}

// 表格驱动测试
func TestAddTable(t *testing.T) {
    tests := []struct {
        a, b, want int
    }{
        {2, 3, 5},
        {0, 0, 0},
        {-1, 1, 0},
    }
    
    for _, tt := range tests {
        got := Add(tt.a, tt.b)
        if got != tt.want {
            t.Errorf("Add(%d, %d) = %d; want %d", tt.a, tt.b, got, tt.want)
        }
    }
}

// 基准测试
func BenchmarkAdd(b *testing.B) {
    for i := 0; i < b.N; i++ {
        Add(2, 3)
    }
}
```

对比其他语言：

```python
# Python的测试 - 使用pytest
import pytest

# 被测试的函数
def add(a, b):
    return a + b

# 单元测试
def test_add():
    assert add(2, 3) == 5

# 参数化测试 (类似表格驱动测试)
@pytest.mark.parametrize("a,b,expected", [
    (2, 3, 5),
    (0, 0, 0),
    (-1, 1, 0),
])
def test_add_parametrized(a, b, expected):
    assert add(a, b) == expected

# 基准测试 (使用pytest-benchmark)
def test_add_benchmark(benchmark):
    result = benchmark(lambda: add(2, 3))
    assert result == 5
```

```java
// Java的测试 - 使用JUnit 5
import org.junit.jupiter.api.Test;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.CsvSource;

import static org.junit.jupiter.api.Assertions.assertEquals;

public class MathTest {
    // 被测试的函数
    public static int add(int a, int b) {
        return a + b;
    }
    
    // 单元测试
    @Test
    public void testAdd() {
        assertEquals(5, add(2, 3));
    }
    
    // 参数化测试 (类似表格驱动测试)
    @ParameterizedTest
    @CsvSource({
        "2, 3, 5",
        "0, 0, 0",
        "-1, 1, 0"
    })
    public void testAddParameterized(int a, int b, int expected) {
        assertEquals(expected, add(a, b));
    }
    
    // 基准测试 (使用JMH)
    // 需要单独设置JMH
}
```

```csharp
// C#的测试 - 使用xUnit
using Xunit;

public class MathTests
{
    // 被测试的函数
    public static int Add(int a, int b)
    {
        return a + b;
    }
    
    // 单元测试
    [Fact]
    public void TestAdd()
    {
        Assert.Equal(5, Add(2, 3));
    }
    
    // 参数化测试 (类似表格驱动测试)
    [Theory]
    [InlineData(2, 3, 5)]
    [InlineData(0, 0, 0)]
    [InlineData(-1, 1, 0)]
    public void TestAddParameterized(int a, int b, int expected)
    {
        Assert.Equal(expected, Add(a, b));
    }
    
    // 基准测试 (使用BenchmarkDotNet)
    // 需要单独设置BenchmarkDotNet
}
```

## 包管理

### Go模块

```go
// Go模块 - go.mod 文件
module example.com/myproject

go 1.16

require (
    github.com/gin-gonic/gin v1.7.4
    github.com/go-sql-driver/mysql v1.6.0
)
```

```bash
# 初始化新模块
go mod init example.com/myproject

# 添加依赖
go get github.com/gin-gonic/gin

# 更新依赖