---
type: docs
title: "Go语言类型系统"
---

## 基本类型

### 数值类型

```go
// Go的数值类型
var i int = 42       // 取决于平台的整数类型(32或64位)
var i8 int8 = 127    // -128到127
var i16 int16 = 32767 // -32768到32767
var i32 int32 = 2147483647 // -2147483648到2147483647
var i64 int64 = 9223372036854775807 // -9223372036854775808到9223372036854775807

var ui uint = 42     // 无符号整数
var ui8 uint8 = 255  // 0到255

var f32 float32 = 3.14 // 单精度浮点数
var f64 float64 = 3.141592653589793 // 双精度浮点数

var c64 complex64 = 1 + 2i  // 复数
var c128 complex128 = 1 + 2i // 复数

var b byte = 255  // uint8的别名
var r rune = '世'  // int32的别名，用于Unicode码点
```

对比其他语言：

```python
# Python
i = 42  # 整数，无大小限制
f = 3.14  # 浮点数
c = 1 + 2j  # 复数
```

```java
// Java
int i = 42;        // 32位整数
long l = 42L;      // 64位整数
float f = 3.14f;   // 32位浮点数
double d = 3.14;   // 64位浮点数
char c = 'A';      // 16位Unicode字符
```

```csharp
// C#
int i = 42;        // 32位整数
long l = 42L;      // 64位整数
float f = 3.14f;   // 32位浮点数
double d = 3.14;   // 64位浮点数
decimal m = 3.14m; // 128位高精度小数
char c = 'A';      // 16位Unicode字符
```

### 字符串和布尔值

```go
// Go
var s string = "Hello, 世界"  // UTF-8编码的字符串
var b bool = true           // 布尔值

// 字符串操作
len(s)                      // 字符串长度（字节数）
s + " Go!"                  // 字符串连接
s[0]                        // 获取第一个字节（注意：不是字符）

// 多行字符串
ml := `这是一个
多行字符串`
```

对比其他语言：

```python
# Python
s = "Hello, 世界"  # 字符串
b = True          # 布尔值

# 字符串操作
len(s)            # 字符串长度（字符数）
s + " Python!"    # 字符串连接
s[0]              # 获取第一个字符

# 多行字符串
ml = """这是一个
多行字符串"""
```

```java
// Java
String s = "Hello, 世界";  // 字符串
boolean b = true;        // 布尔值

// 字符串操作
s.length();              // 字符串长度
s + " Java!";            // 字符串连接
s.charAt(0);             // 获取第一个字符

// 多行字符串 (Java 15+)
String ml = """
        这是一个
        多行字符串
        """;
```

### 类型转换

```go
// Go - 显式类型转换
var i int = 42
var f float64 = float64(i)  // int到float64
var s string = strconv.Itoa(i)  // int到string
var i2, err = strconv.Atoi("42")  // string到int
var b bool = i != 0  // int到bool
```

对比其他语言：

```python
# Python - 隐式和显式转换
i = 42
f = float(i)  # int到float
s = str(i)    # int到string
i2 = int("42")  # string到int
b = bool(i)   # int到bool
```

```java
// Java
int i = 42;
double f = (double) i;  // int到double
String s = Integer.toString(i);  // int到String
int i2 = Integer.parseInt("42");  // String到int
boolean b = i != 0;  // int到boolean
```

## 复合类型

### 数组和切片

```go
// Go数组 - 固定长度
var arr [3]int = [3]int{1, 2, 3}
// 或简写为
arr := [3]int{1, 2, 3}
// 让编译器计算长度
arr := [...]int{1, 2, 3}

// Go切片 - 动态长度
var slice []int = []int{1, 2, 3}
// 或简写为
slice := []int{1, 2, 3}
// 从数组创建切片
slice = arr[:]
// 创建指定长度和容量的切片
slice = make([]int, 3, 5)  // 长度3，容量5

// 切片操作
len(slice)  // 长度
cap(slice)  // 容量
slice = append(slice, 4)  // 添加元素
slice2 := slice[1:3]  // 子切片
```

对比其他语言：

```python
# Python列表
list = [1, 2, 3]  # 创建列表
len(list)         # 长度
list.append(4)    # 添加元素
list2 = list[1:3] # 子列表
```

```java
// Java数组
int[] arr = {1, 2, 3};  // 创建数组
arr.length;             // 长度

// Java动态数组 (ArrayList)
ArrayList<Integer> list = new ArrayList<>();
list.add(1);
list.add(2);
list.add(3);
list.size();            // 长度
list.add(4);            // 添加元素
List<Integer> list2 = list.subList(1, 3);  // 子列表
```

### 映射

```go
// Go映射
var m map[string]int = map[string]int{
    "apple": 1,
    "banana": 2,
}
// 或简写为
m := map[string]int{
    "apple": 1,
    "banana": 2,
}

// 创建空映射
m = make(map[string]int)

// 映射操作
m["cherry"] = 3  // 添加或更新
val, exists := m["apple"]  // 检查键是否存在
delete(m, "banana")  // 删除
len(m)  // 长度

// 遍历映射
for k, v := range m {
    fmt.Printf("%s: %d\n", k, v)
}
```

对比其他语言：

```python
# Python字典
d = {"apple": 1, "banana": 2}  # 创建字典
d["cherry"] = 3  # 添加或更新
val = d.get("apple")  # 获取值，键不存在返回None
val = d["apple"]  # 获取值，键不存在抛出异常
"apple" in d  # 检查键是否存在
del d["banana"]  # 删除
len(d)  # 长度

# 遍历字典
for k, v in d.items():
    print(f"{k}: {v}")
```

```java
// Java Map
Map<String, Integer> map = new HashMap<>();
map.put("apple", 1);
map.put("banana", 2);
map.put("cherry", 3);  // 添加或更新
Integer val = map.get("apple");  // 获取值
boolean exists = map.containsKey("apple");  // 检查键是否存在
map.remove("banana");  // 删除
map.size();  // 长度

// 遍历Map
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    System.out.printf("%s: %d%n", entry.getKey(), entry.getValue());
}
```

### 结构体

```go
// Go结构体
type Person struct {
    Name string
    Age  int
}

// 创建结构体
p := Person{Name: "Alice", Age: 30}
// 或
var p Person
p.Name = "Alice"
p.Age = 30

// 结构体指针
p2 := &Person{Name: "Bob", Age: 25}
p2.Age = 26  // 自动解引用

// 嵌套结构体
type Address struct {
    City  string
    State string
}

type Employee struct {
    Person  // 匿名字段，实现组合
    Address // 匿名字段
    Salary  float64
}

e := Employee{
    Person: Person{Name: "Charlie", Age: 35},
    Address: Address{City: "New York", State: "NY"},
    Salary: 50000,
}

// 访问嵌套字段
fmt.Println(e.Name)  // 直接访问Person的Name
fmt.Println(e.City)  // 直接访问Address的City
```

对比其他语言：

```python
# Python类
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

# 创建对象
p = Person("Alice", 30)

# 继承
class Employee(Person):
    def __init__(self, name, age, salary):
        super().__init__(name, age)
        self.salary = salary
        self.address = {"city": "New York", "state": "NY"}

e = Employee("Charlie", 35, 50000)
print(e.name)  # 访问父类属性
print(e.address["city"])  # 访问字典属性
```

```java
// Java类
public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // Getters and setters
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public int getAge() { return age; }
    public void setAge(int age) { this.age = age; }
}

// 继承
public class Employee extends Person {
    private Address address;
    private double salary;

    public Employee(String name, int age, double salary) {
        super(name, age);
        this.salary = salary;
        this.address = new Address("New York", "NY");
    }

    // 内部类
    public class Address {
        private String city;
        private String state;

        public Address(String city, String state) {
            this.city = city;
            this.state = state;
        }

        // Getters and setters
    }
}
```

## 练习

1. 创建一个学生结构体，包含姓名、年龄和成绩切片，并实现一个计算平均成绩的方法
2. 编写一个函数，接受一个字符串映射，返回值最大的键
3. 实现一个简单的购物车，使用结构体表示商品，使用切片存储多个商品

## 常见陷阱

- 数组是值类型，作为参数传递时会复制整个数组
- 切片是引用类型，修改切片会影响原始数据
- 映射也是引用类型，不能对 nil 映射赋值
- 结构体是值类型，但可以使用指针来避免复制
- Go 没有类和继承，而是使用组合和接口
- 结构体字段首字母大写表示公开，小写表示私有
