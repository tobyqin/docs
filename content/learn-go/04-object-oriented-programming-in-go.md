---
type: docs
title: "Go 的面向对象编程"
weight: 5
---

## 结构体和方法 vs. 类

### 定义类型和行为

```go
// Go的结构体和方法
type Person struct {
    Name string
    Age  int
}

// 为Person类型定义方法
func (p Person) Greet() string {
    return fmt.Sprintf("你好，我是%s，今年%d岁", p.Name, p.Age)
}

// 指针接收者方法（可以修改接收者）
func (p *Person) Birthday() {
    p.Age++
}

// 创建和使用
p := Person{Name: "张三", Age: 30}
fmt.Println(p.Greet())  // 输出：你好，我是张三，今年30岁
p.Birthday()
fmt.Println(p.Age)      // 输出：31
```

对比其他语言：

```python
# Python的类
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def greet(self):
        return f"你好，我是{self.name}，今年{self.age}岁"

    def birthday(self):
        self.age += 1

# 创建和使用
p = Person("张三", 30)
print(p.greet())  # 输出：你好，我是张三，今年30岁
p.birthday()
print(p.age)      # 输出：31
```

```java
// Java的类
public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String greet() {
        return String.format("你好，我是%s，今年%d岁", name, age);
    }

    public void birthday() {
        age++;
    }

    public int getAge() {
        return age;
    }
}

// 创建和使用
Person p = new Person("张三", 30);
System.out.println(p.greet());  // 输出：你好，我是张三，今年30岁
p.birthday();
System.out.println(p.getAge());  // 输出：31
```

```csharp
// C#的类
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }

    public Person(string name, int age)
    {
        Name = name;
        Age = age;
    }

    public string Greet()
    {
        return $"你好，我是{Name}，今年{Age}岁";
    }

    public void Birthday()
    {
        Age++;
    }
}

// 创建和使用
var p = new Person("张三", 30);
Console.WriteLine(p.Greet());  // 输出：你好，我是张三，今年30岁
p.Birthday();
Console.WriteLine(p.Age);      // 输出：31
```

### 值接收者 vs. 指针接收者

```go
// Go中的值接收者和指针接收者
type Counter struct {
    count int
}

// 值接收者 - 不修改原始值
func (c Counter) Increment() int {
    c.count++  // 只修改副本
    return c.count
}

// 指针接收者 - 修改原始值
func (c *Counter) IncrementPtr() int {
    c.count++  // 修改原始值
    return c.count
}

// 使用示例
c := Counter{}
fmt.Println(c.Increment())  // 输出：1
fmt.Println(c.count)       // 输出：0（原值未变）

fmt.Println(c.IncrementPtr())  // 输出：1
fmt.Println(c.count)          // 输出：1（原值已变）
```

## 接口

### 隐式实现

```go
// Go的接口 - 隐式实现
type Speaker interface {
    Speak() string
}

// 实现Speaker接口的Person类型
type Person struct {
    Name string
}

func (p Person) Speak() string {
    return p.Name + "说：你好！"
}

// 实现Speaker接口的Dog类型
type Dog struct {
    Name string
}

func (d Dog) Speak() string {
    return d.Name + "说：汪汪！"
}

// 使用接口
func SaySomething(s Speaker) {
    fmt.Println(s.Speak())
}

// 调用
p := Person{Name: "张三"}
SaySomething(p)  // 输出：张三说：你好！

d := Dog{Name: "旺财"}
SaySomething(d)  // 输出：旺财说：汪汪！
```

对比其他语言：

```python
# Python的接口（通过抽象基类或鸭子类型）
from abc import ABC, abstractmethod

# 使用抽象基类
class Speaker(ABC):
    @abstractmethod
    def speak(self):
        pass

class Person(Speaker):
    def __init__(self, name):
        self.name = name

    def speak(self):
        return f"{self.name}说：你好！"

class Dog(Speaker):
    def __init__(self, name):
        self.name = name

    def speak(self):
        return f"{self.name}说：汪汪！"

# 使用接口
def say_something(speaker):
    print(speaker.speak())

# 调用
p = Person("张三")
say_something(p)  # 输出：张三说：你好！

d = Dog("旺财")
say_something(d)  # 输出：旺财说：汪汪！

# Python也支持鸭子类型（不需要显式继承）
class Cat:
    def __init__(self, name):
        self.name = name

    def speak(self):
        return f"{self.name}说：喵喵！"

c = Cat("咪咪")
say_something(c)  # 也能工作：咪咪说：喵喵！
```

```java
// Java的接口 - 显式实现
public interface Speaker {
    String speak();
}

public class Person implements Speaker {
    private String name;

    public Person(String name) {
        this.name = name;
    }

    @Override
    public String speak() {
        return name + "说：你好！";
    }
}

public class Dog implements Speaker {
    private String name;

    public Dog(String name) {
        this.name = name;
    }

    @Override
    public String speak() {
        return name + "说：汪汪！";
    }
}

// 使用接口
public static void saySomething(Speaker speaker) {
    System.out.println(speaker.speak());
}

// 调用
Person p = new Person("张三");
saySomething(p);  // 输出：张三说：你好！

Dog d = new Dog("旺财");
saySomething(d);  // 输出：旺财说：汪汪！
```

```csharp
// C#的接口 - 显式实现
public interface ISpeaker
{
    string Speak();
}

public class Person : ISpeaker
{
    public string Name { get; set; }

    public Person(string name)
    {
        Name = name;
    }

    public string Speak()
    {
        return $"{Name}说：你好！";
    }
}

public class Dog : ISpeaker
{
    public string Name { get; set; }

    public Dog(string name)
    {
        Name = name;
    }

    public string Speak()
    {
        return $"{Name}说：汪汪！";
    }
}

// 使用接口
public static void SaySomething(ISpeaker speaker)
{
    Console.WriteLine(speaker.Speak());
}

// 调用
var p = new Person("张三");
SaySomething(p);  // 输出：张三说：你好！

var d = new Dog("旺财");
SaySomething(d);  // 输出：旺财说：汪汪！
```

### 空接口和类型断言

```go
// Go的空接口和类型断言

// 空接口可以存储任何类型的值
var i interface{}
i = 42
fmt.Println(i)  // 输出：42
i = "hello"
fmt.Println(i)  // 输出：hello

// 类型断言
s, ok := i.(string)  // 检查i是否为string类型
if ok {
    fmt.Println("i是字符串:", s)
} else {
    fmt.Println("i不是字符串")
}

// 类型选择
switch v := i.(type) {
case int:
    fmt.Printf("整数: %d\n", v)
case string:
    fmt.Printf("字符串: %s\n", v)
default:
    fmt.Printf("未知类型: %T\n", v)
}
```

对比其他语言：

```python
# Python的动态类型和类型检查

# Python变量可以存储任何类型
i = 42
print(i)  # 输出：42
i = "hello"
print(i)  # 输出：hello

# 类型检查
if isinstance(i, str):
    print("i是字符串:", i)
else:
    print("i不是字符串")

# 类型选择
def check_type(obj):
    if isinstance(obj, int):
        print(f"整数: {obj}")
    elif isinstance(obj, str):
        print(f"字符串: {obj}")
    else:
        print(f"未知类型: {type(obj).__name__}")

check_type(i)  # 输出：字符串: hello
```

```java
// Java的Object类和instanceof

// Object可以引用任何对象
Object obj = 42;  // 自动装箱为Integer
System.out.println(obj);  // 输出：42
obj = "hello";
System.out.println(obj);  // 输出：hello

// 类型检查和转换
if (obj instanceof String) {
    String s = (String) obj;
    System.out.println("obj是字符串: " + s);
} else {
    System.out.println("obj不是字符串");
}

// 类型选择
public static void checkType(Object obj) {
    if (obj instanceof Integer) {
        System.out.printf("整数: %d%n", (Integer) obj);
    } else if (obj instanceof String) {
        System.out.printf("字符串: %s%n", (String) obj);
    } else {
        System.out.printf("未知类型: %s%n", obj.getClass().getSimpleName());
    }
}

checkType(obj);  // 输出：字符串: hello
```

```csharp
// C#的object类型和模式匹配

// object可以存储任何类型
object obj = 42;
Console.WriteLine(obj);  // 输出：42
obj = "hello";
Console.WriteLine(obj);  // 输出：hello

// 类型检查和转换
if (obj is string s)
{
    Console.WriteLine($"obj是字符串: {s}");
}
else
{
    Console.WriteLine("obj不是字符串");
}

// 类型选择（C# 7.0+的模式匹配）
public static void CheckType(object obj)
{
    switch (obj)
    {
        case int i:
            Console.WriteLine($"整数: {i}");
            break;
        case string s:
            Console.WriteLine($"字符串: {s}");
            break;
        default:
            Console.WriteLine($"未知类型: {obj.GetType().Name}");
            break;
    }
}

CheckType(obj);  // 输出：字符串: hello
```

## 组合 vs. 继承

```go
// Go使用组合而非继承

// 基础结构体
type Animal struct {
    Name string
}

func (a Animal) Breathe() string {
    return a.Name + "正在呼吸"
}

// 通过组合复用Animal的功能
type Dog struct {
    Animal  // 嵌入Animal，实现组合
    Breed string
}

// Dog可以有自己的方法
func (d Dog) Bark() string {
    return d.Name + "汪汪叫"
}

// 使用
dog := Dog{
    Animal: Animal{Name: "旺财"},
```
