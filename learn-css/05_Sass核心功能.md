# 05_Sass核心功能

## 变量系统

Sass变量使用`$`符号定义，可以存储颜色、字体或任何CSS值。

```scss
// 变量定义
$primary-color: #3498db;
$secondary-color: #2ecc71;
$font-stack: 'Helvetica', Arial, sans-serif;
$base-padding: 15px;
$border-radius: 4px;

// 变量使用
.button {
  background-color: $primary-color;
  font-family: $font-stack;
  padding: $base-padding;
  border-radius: $border-radius;
}

// 变量作用域
.container {
  $width: 100%; // 局部变量
  width: $width;
}
// 这里不能访问$width

// 全局变量
$global-var: #000 !global;

// 默认变量（可被覆盖）
$brand-color: #3498db !default;
```

**与Python变量对比**:
```python
# Python变量
primary_color = "#3498db"
font_stack = ["Helvetica", "Arial", "sans-serif"]

# 局部作用域
def some_function():
    width = "100%"  # 局部变量
    print(width)

# 全局变量
global_var = "#000"
```

## 嵌套规则

Sass允许按HTML的层次结构嵌套CSS选择器。

```scss
// 基本嵌套
nav {
  background-color: #333;
  
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }
  
  li {
    display: inline-block;
    
    a {
      color: white;
      text-decoration: none;
      padding: 10px 15px;
      display: block;
    }
  }
}

// 等同于CSS
nav { background-color: #333; }
nav ul { margin: 0; padding: 0; list-style: none; }
nav li { display: inline-block; }
nav li a { color: white; text-decoration: none; padding: 10px 15px; display: block; }
```

### 父选择器引用(&)

使用`&`符号引用父选择器。

```scss
.button {
  background-color: blue;
  
  &:hover {
    background-color: darkblue;
  }
  
  &.large {
    padding: 20px;
  }
  
  &-primary {
    background-color: green;
  }
}

// 等同于CSS
.button { background-color: blue; }
.button:hover { background-color: darkblue; }
.button.large { padding: 20px; }
.button-primary { background-color: green; }
```

### 属性嵌套

相关属性可以嵌套在一起。

```scss
.element {
  font: {
    family: Arial;
    size: 16px;
    weight: bold;
  }
  
  margin: {
    top: 10px;
    right: 5px;
    bottom: 10px;
    left: 5px;
  }
}

// 等同于CSS
.element {
  font-family: Arial;
  font-size: 16px;
  font-weight: bold;
  margin-top: 10px;
  margin-right: 5px;
  margin-bottom: 10px;
  margin-left: 5px;
}
```

**与Python缩进块对比**:
```python
# Python中的嵌套结构
class Navigation:
    def __init__(self):
        self.background_color = "#333"
        
    class List:
        def __init__(self):
            self.margin = 0
            self.padding = 0
            self.style = "none"
            
    class Item:
        def __init__(self):
            self.display = "inline-block"
            
        class Link:
            def __init__(self):
                self.color = "white"
                self.text_decoration = "none"
```

## Mixins和函数

### Mixins

Mixins是可重用的CSS声明组，类似Python中的函数。

```scss
// 定义mixin
@mixin border-radius($radius) {
  -webkit-border-radius: $radius;
  -moz-border-radius: $radius;
  border-radius: $radius;
}

// 使用mixin
.box {
  @include border-radius(10px);
}

// 带默认值的mixin
@mixin box-shadow($x: 0, $y: 2px, $blur: 5px, $color: rgba(0,0,0,.4)) {
  -webkit-box-shadow: $x $y $blur $color;
  -moz-box-shadow: $x $y $blur $color;
  box-shadow: $x $y $blur $color;
}

// 使用默认值
.card {
  @include box-shadow();
}

// 覆盖默认值
.card-highlighted {
  @include box-shadow(0, 5px, 10px, rgba(0,0,0,.6));
}

// 可变参数mixin
@mixin transition($transitions...) {
  -webkit-transition: $transitions;
  -moz-transition: $transitions;
  transition: $transitions;
}

// 使用可变参数
.element {
  @include transition(color .3s ease, background-color .5s ease);
}
```

### 函数

Sass函数返回计算值，可在样式中使用。

```scss
// 自定义函数
@function calculate-width($col-span, $total-cols: 12) {
  @return percentage($col-span / $total-cols);
}

// 使用函数
.col-4 {
  width: calculate-width(4);  // 返回 33.33333%
}

// 复杂函数示例
@function text-contrast($background) {
  $brightness: lightness($background);
  
  @if $brightness > 60% {
    @return #000; // 深色文本用于浅色背景
  } @else {
    @return #fff; // 浅色文本用于深色背景
  }
}

// 使用对比函数
.button {
  $bg-color: #3498db;
  background-color: $bg-color;
  color: text-contrast($bg-color);
}
```

**与Python函数对比**:
```python
# Python函数
def border_radius(radius):
    return {
        "webkit-border-radius": radius,
        "moz-border-radius": radius,
        "border-radius": radius
    }

# 带默认参数的函数
def box_shadow(x=0, y=2, blur=5, color="rgba(0,0,0,.4)"):
    return {
        "webkit-box-shadow": f"{x}px {y}px {blur}px {color}",
        "moz-box-shadow": f"{x}px {y}px {blur}px {color}",
        "box-shadow": f"{x}px {y}px {blur}px {color}"
    }

# 计算函数
def calculate_width(col_span, total_cols=12):
    return (col_span / total_cols) * 100
```

## 继承与扩展

Sass的`@extend`指令允许一个选择器继承另一个选择器的样式。

```scss
// 基本样式
.message {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}

// 继承基本样式
.success {
  @extend .message;
  border-color: green;
}

.error {
  @extend .message;
  border-color: red;
}

.warning {
  @extend .message;
  border-color: yellow;
}

// 编译为CSS
.message, .success, .error, .warning {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}

.success {
  border-color: green;
}

.error {
  border-color: red;
}

.warning {
  border-color: yellow;
}
```

### 占位符选择器(%)

占位符选择器只有被扩展时才会编译到CSS中。

```scss
// 定义占位符
%button-base {
  display: inline-block;
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

// 使用占位符
.primary-button {
  @extend %button-base;
  background-color: blue;
  color: white;
}

.secondary-button {
  @extend %button-base;
  background-color: gray;
  color: black;
}

// 编译为CSS（不包含%button-base）
.primary-button, .secondary-button {
  display: inline-block;
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.primary-button {
  background-color: blue;
  color: white;
}

.secondary-button {
  background-color: gray;
  color: black;
}
```

**与Python类继承对比**:
```python
# Python类继承
class Message:
    def __init__(self):
        self.border = "1px solid #ccc"
        self.padding = "10px"
        self.color = "#333"

class Success(Message):
    def __init__(self):
        super().__init__()
        self.border_color = "green"

class Error(Message):
    def __init__(self):
        super().__init__()
        self.border_color = "red"
```

## 实战示例：构建组件库

```scss
// _variables.scss
$primary-color: #3498db;
$secondary-color: #2ecc71;
$danger-color: #e74c3c;
$warning-color: #f39c12;
$light-gray: #f5f5f5;
$dark-gray: #333;
$border-radius: 4px;
$base-padding: 15px;
$transition-speed: 0.3s;

// _mixins.scss
@mixin flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}

@mixin button-variant($bg-color, $text-color: white) {
  background-color: $bg-color;
  color: $text-color;
  
  &:hover {
    background-color: darken($bg-color, 10%);
  }
  
  &:active {
    background-color: darken($bg-color, 15%);
  }
}

// _placeholders.scss
%card-base {
  border-radius: $border-radius;
  overflow: hidden;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

%button-base {
  display: inline-block;
  padding: $base-padding;
  border: none;
  border-radius: $border-radius;
  cursor: pointer;
  transition: all $transition-speed ease;
  text-align: center;
  text-decoration: none;
}

// _buttons.scss
.btn {
  @extend %button-base;
  
  &-primary {
    @include button-variant($primary-color);
  }
  
  &-secondary {
    @include button-variant($secondary-color);
  }
  
  &-danger {
    @include button-variant($danger-color);
  }
  
  &-warning {
    @include button-variant($warning-color);
  }
  
  &-outline {
    background-color: transparent;
    
    &-primary {
      color: $primary-color;
      border: 1px solid $primary-color;
      
      &:hover {
        background-color: $primary-color;
        color: white;
      }
    }
  }
  
  &-sm {
    padding: $base-padding / 2;
    font-size: 0.8em;
  }
  
  &-lg {
    padding: $base-padding * 1.5;
    font-size: 1.2em;
  }
}

// _cards.scss
.card {
  @extend %card-base;
  background-color: white;
  
  &__header {
    padding: $base-padding;
    border-bottom: 1px solid $light-gray;
  }
  
  &__body {
    padding: $base-padding;
  }
  
  &__footer {
    padding: $base-padding;
    border-top: 1px solid $light-gray;
    background-color: $light-gray;
  }
  
  &--primary {
    .card__header {
      background-color: $primary-color;
      color: white;
    }
  }
  
  &--flat {
    box-shadow: none;
    border: 1px solid $light-gray;
  }
}

// main.scss
@import 'variables';
@import 'mixins';
@import 'placeholders';
@import 'buttons';
@import 'cards';
```

**HTML使用示例**:

```html
<button class="btn btn-primary">主要按钮</button>
<button class="btn btn-secondary btn-lg">大号次要按钮</button>
<button class="btn btn-outline-primary btn-sm">小号轮廓按钮</button>

<div class="card">
  <div class="card__header">
    <h3>标准卡片</h3>
  </div>
  <div class="card__body">
    <p>这是卡片内容</p>
  </div>
  <div class="card__footer">
    <button class="btn btn-primary">确认</button>
  </div>
</div>

<div class="card card--primary card--flat">
  <div class="card__header">
    <h3>主要扁平卡片</h3>
  </div>
  <div class="card__body">
    <p>这是卡片内容</p>
  </div>
</div>
```

这个组件库示例展示了如何使用Sass的变量、混合、占位符和继承来创建可复用、可维护的组件系统。
