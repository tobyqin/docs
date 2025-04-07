---
type: docs
title: "Go语言独特特性"
---

## Go 中的指针

### 基本概念

```go
// Go指针
var x int = 42
var p *int = &x  // p是指向x的指针
fmt.Println(*p)  // 解引用，输出42
*p = 21          // 通过指针修改x的值
fmt.Println(x)   // 输出21

// 创建指针的另一种方式
y := new(int)    // 创建一个指向int零值的指针
*y = 100         // 设置值
```

对比其他语言：

```python
# Python没有直接的指针概念，但有对象引用
x = 42
y = x      # y引用与x相同的值
y = 21     # 修改y不影响x
print(x)   # 输出42

# 对于可变对象，行为类似指针
list1 = [1, 2, 3]
list2 = list1    # list2引用与list1相同的列表
list2.append(4)  # 修改list2也会影响list1
print(list1)     # 输出[1, 2, 3, 4]
```

```java
// Java没有直接的指针，但有对象引用
Integer x = 42;  // Integer是对象，存储在堆上
Integer y = x;   // y引用与x相同的对象
y = 21;          // 创建新对象，y指向新对象
System.out.println(x);  // 输出42

// 对于对象，行为类似指针
ArrayList<Integer> list1 = new ArrayList<>();
list1.add(1);
ArrayList<Integer> list2 = list1;  // list2引用与list1相同的对象
list2.add(2);
System.out.println(list1);  // 输出[1, 2]
```

```csharp
// C#有引用类型和值类型，以及ref参数
int x = 42;      // 值类型
int y = x;       // 复制值
y = 21;          // 修改y不影响x
Console.WriteLine(x);  // 输出42

// 使用ref参数可以实现类似指针的行为
void Modify(ref int a) {
    a = 100;
}
int z = 42;
Modify(ref z);
Console.WriteLine(z);  // 输出100

// 对于引用类型，行为类似指针
List<int> list1 = new List<int> { 1, 2, 3 };
List<int> list2 = list1;  // list2引用与list1相同的对象
list2.Add(4);
Console.WriteLine(string.Join(", ", list1));  // 输出"1, 2, 3, 4"
```

### 指针的用途

```go
// Go中指针的常见用途

// 1. 修改函数外部的变量
func increment(n *int) {
    *n++
}

count := 10
increment(&count)
fmt.Println(count)  // 输出11

// 2. 避免复制大型结构体
type LargeStruct struct {
    Data [1024]int
}

func process(ls *LargeStruct) {
    // 处理结构体但不复制它
    ls.Data[0] = 42
}

ls := LargeStruct{}
process(&ls)

// 3. 实现数据结构
type Node struct {
    Value int
    Next  *Node  // 指向下一个节点的指针
}
```

## 错误处理

### 错误值 vs. 异常

```go
// Go的错误处理 - 返回错误值
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, errors.New("除数不能为零")
    }
    return a / b, nil
}

// 调用并处理错误
result, err := divide(10, 0)
if err != nil {
    fmt.Println("错误:", err)
    return
}
fmt.Println("结果:", result)

// 自定义错误
type DivisionError struct {
    Dividend float64
    Divisor  float64
}

func (e *DivisionError) Error() string {
    return fmt.Sprintf("不能将%.2f除以%.2f", e.Dividend, e.Divisor)
}

func safeDivide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, &DivisionError{a, b}
    }
    return a / b, nil
}
```

对比其他语言：

```python
# Python的异常处理
def divide(a, b):
    if b == 0:
        raise ValueError("除数不能为零")
    return a / b

# 使用try-except捕获异常
try:
    result = divide(10, 0)
    print("结果:", result)
except ValueError as e:
    print("错误:", e)

# 自定义异常
class DivisionError(Exception):
    def __init__(self, dividend, divisor):
        self.dividend = dividend
        self.divisor = divisor
        super().__init__(f"不能将{dividend}除以{divisor}")

def safe_divide(a, b):
    if b == 0:
        raise DivisionError(a, b)
    return a / b
```

```java
// Java的异常处理
public static double divide(double a, double b) throws ArithmeticException {
    if (b == 0) {
        throw new ArithmeticException("除数不能为零");
    }
    return a / b;
}

// 使用try-catch捕获异常
try {
    double result = divide(10, 0);
    System.out.println("结果: " + result);
} catch (ArithmeticException e) {
    System.out.println("错误: " + e.getMessage());
}

// 自定义异常
class DivisionException extends Exception {
    private double dividend;
    private double divisor;

    public DivisionException(double dividend, double divisor) {
        super(String.format("不能将%.2f除以%.2f", dividend, divisor));
        this.dividend = dividend;
        this.divisor = divisor;
    }
}

public static double safeDivide(double a, double b) throws DivisionException {
    if (b == 0) {
        throw new DivisionException(a, b);
    }
    return a / b;
}
```

```csharp
// C#的异常处理
public static double Divide(double a, double b)
{
    if (b == 0)
    {
        throw new DivideByZeroException("除数不能为零");
    }
    return a / b;
}

// 使用try-catch捕获异常
try
{
    double result = Divide(10, 0);
    Console.WriteLine($"结果: {result}");
}
catch (DivideByZeroException e)
{
    Console.WriteLine($"错误: {e.Message}");
}

// 自定义异常
public class DivisionException : Exception
{
    public double Dividend { get; }
    public double Divisor { get; }

    public DivisionException(double dividend, double divisor)
        : base($"不能将{dividend}除以{divisor}")
    {
        Dividend = dividend;
        Divisor = divisor;
    }
}

public static double SafeDivide(double a, double b)
{
    if (b == 0)
    {
        throw new DivisionException(a, b);
    }
    return a / b;
}
```

### 错误处理最佳实践

```go
// Go错误处理最佳实践

// 1. 尽早返回错误
func processFile(path string) error {
    file, err := os.Open(path)
    if err != nil {
        return fmt.Errorf("打开文件失败: %w", err)
    }
    defer file.Close()

    // 处理文件...
    return nil
}

// 2. 使用defer确保资源释放
func copyFile(src, dst string) error {
    srcFile, err := os.Open(src)
    if err != nil {
        return fmt.Errorf("打开源文件失败: %w", err)
    }
    defer srcFile.Close()

    dstFile, err := os.Create(dst)
    if err != nil {
        return fmt.Errorf("创建目标文件失败: %w", err)
    }
    defer dstFile.Close()

    _, err = io.Copy(dstFile, srcFile)
    if err != nil {
        return fmt.Errorf("复制文件失败: %w", err)
    }

    return nil
}

// 3. 错误包装 (Go 1.13+)
func readConfig(path string) ([]byte, error) {
    data, err := ioutil.ReadFile(path)
    if err != nil {
        return nil, fmt.Errorf("读取配置文件 %s 失败: %w", path, err)
    }
    return data, nil
}
```

## 并发模型

### Goroutines

```go
// Go的并发 - Goroutines
func sayHello() {
    fmt.Println("Hello, Gopher!")
}

// 启动一个goroutine
go sayHello()

// 带参数的goroutine
go func(name string) {
    fmt.Printf("Hello, %s!\n", name)
}("Gopher")

// 等待goroutine完成
time.Sleep(100 * time.Millisecond)

// 更好的方式 - 使用WaitGroup
var wg sync.WaitGroup

wg.Add(1)  // 添加一个等待的goroutine
go func() {
    defer wg.Done()  // 完成时通知WaitGroup
    fmt.Println("Working...")
}()

wg.Wait()  // 等待所有goroutine完成
```

对比其他语言：

```python
# Python的线程和异步
import threading
import asyncio

# 使用线程
def say_hello():
    print("Hello, Pythonista!")

thread = threading.Thread(target=say_hello)
thread.start()
thread.join()  # 等待线程完成

# 使用asyncio (Python 3.5+)
async def async_hello():
    print("Hello, async Python!")

async def main():
    await async_hello()

asyncio.run(main())  # Python 3.7+
```

```java
// Java的线程和并发

// 使用Thread
Thread thread = new Thread(() -> {
    System.out.println("Hello, Java!");
});
thread.start();
try {
    thread.join();  // 等待线程完成
} catch (InterruptedException e) {
    e.printStackTrace();
}

// 使用ExecutorService
ExecutorService executor = Executors.newSingleThreadExecutor();
executor.submit(() -> {
    System.out.println("Hello from ExecutorService!");
});
executor.shutdown();

// 使用CompletableFuture (Java 8+)
CompletableFuture.runAsync(() -> {
    System.out.println("Hello from CompletableFuture!");
}).join();
```

```csharp
// C#的线程和异步

// 使用Thread
Thread thread = new Thread(() => {
    Console.WriteLine("Hello, C#!");
});
thread.Start();
thread.Join();  // 等待线程完成

// 使用Task
Task task = Task.Run(() => {
    Console.WriteLine("Hello from Task!");
});
task.Wait();  // 等待任务完成

// 使用async/await
public async Task SayHelloAsync()
{
    await Task.Delay(100);  // 模拟异步操作
    Console.WriteLine("Hello, async C#!");
}

// 调用异步方法
await SayHelloAsync();
```

### 通道和 Select

```go
// Go的通道 (Channels)

// 创建通道
ch := make(chan string)      // 无缓冲通道
bufferedCh := make(chan int, 10)  // 缓冲通道

// 发送和接收
go func() {
    ch <- "hello"  // 发送到通道
}()
msg := <-ch  // 从通道接收

// 关闭通道
close(ch)

// 遍历通道
for msg := range ch {
    fmt.Println(msg)
}

// select语句 - 多通道操作
select {
case msg1 := <-ch1:
    fmt.Println("收到来自ch1的消息:", msg1)
case msg2 := <-ch2:
    fmt.Println("收到来自ch2的消息:", msg2)
case ch3 <- "hello
```
