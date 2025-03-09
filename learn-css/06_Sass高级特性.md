# 06_Sass高级特性

## 条件语句与循环

### 条件语句

Sass提供`@if`、`@else if`和`@else`指令进行条件控制。

```scss
// 基本条件语句
$theme: 'dark';

body {
  @if $theme == 'light' {
    background-color: #fff;
    color: #000;
  } @else if $theme == 'dark' {
    background-color: #333;
    color: #fff;
  } @else {
    background-color: #f5f5f5;
    color: #333;
  }
}

// 使用条件表达式
@mixin text-color($bg-color) {
  @if lightness($bg-color) > 50% {
    color: #000; // 深色文本用于浅色背景
  } @else {
    color: #fff; // 浅色文本用于深色背景
  }
}

.button {
  background-color: #3498db;
  @include text-color(#3498db);
}
```

**与Python条件语句对比**:
```python
# Python条件语句
theme = 'dark'

if theme == 'light':
    background_color = '#fff'
    text_color = '#000'
elif theme == 'dark':
    background_color = '#333'
    text_color = '#fff'
else:
    background_color = '#f5f5f5'
    text_color = '#333'
```

### 循环语句

Sass提供三种循环方式：`@for`、`@each`和`@while`。

#### @for循环

```scss
// 从1到3的循环
@for $i from 1 through 3 {
  .col-#{$i} {
    width: 100% / 3 * $i;
  }
}

// 编译结果
.col-1 { width: 33.3333%; }
.col-2 { width: 66.6667%; }
.col-3 { width: 100%; }

// 从0到2的循环
@for $i from 0 to 3 {
  .m-#{$i} {
    margin: #{$i}rem;
  }
}

// 编译结果
.m-0 { margin: 0rem; }
.m-1 { margin: 1rem; }
.m-2 { margin: 2rem; }
```

#### @each循环

```scss
// 遍历列表
$colors: red, green, blue;

@each $color in $colors {
  .text-#{$color} {
    color: $color;
  }
}

// 编译结果
.text-red { color: red; }
.text-green { color: green; }
.text-blue { color: blue; }

// 遍历映射
$theme-colors: (
  "primary": #3498db,
  "success": #2ecc71,
  "danger": #e74c3c
);

@each $name, $color in $theme-colors {
  .btn-#{$name} {
    background-color: $color;
    
    &:hover {
      background-color: darken($color, 10%);
    }
  }
}

// 编译结果
.btn-primary { background-color: #3498db; }
.btn-primary:hover { background-color: #217dbb; }
.btn-success { background-color: #2ecc71; }
.btn-success:hover { background-color: #25a25a; }
.btn-danger { background-color: #e74c3c; }
.btn-danger:hover { background-color: #d62c1a; }
```

#### @while循环

```scss
$i: 1;

@while $i <= 3 {
  .item-#{$i} {
    width: 100% / $i;
  }
  $i: $i + 1;
}

// 编译结果
.item-1 { width: 100%; }
.item-2 { width: 50%; }
.item-3 { width: 33.3333%; }
```

**与Python循环对比**:
```python
# Python for循环
for i in range(1, 4):  # 1到3
    print(f".col-{i} {{ width: {100/3*i}%; }}")

# Python列表循环
colors = ["red", "green", "blue"]
for color in colors:
    print(f".text-{color} {{ color: {color}; }}")

# Python字典循环
theme_colors = {
    "primary": "#3498db",
    "success": "#2ecc71",
    "danger": "#e74c3c"
}
for name, color in theme_colors.items():
    print(f".btn-{name} {{ background-color: {color}; }}")

# Python while循环
i = 1
while i <= 3:
    print(f".item-{i} {{ width: {100/i}%; }}")
    i += 1
```

## 模块化与导入系统

### @import指令

Sass的`@import`允许将多个Sass文件组合成一个文件。

```scss
// 基本导入
@import 'variables';
@import 'mixins';

// 多文件导入
@import 'reset', 'typography', 'buttons';

// 嵌套导入
.sidebar {
  @import 'sidebar-styles';
}
```

### @use指令(Sass 3.6+)

`@use`是`@import`的现代替代品，提供了命名空间和模块化功能。

```scss
// _colors.scss
$primary: #3498db;
$secondary: #2ecc71;

@mixin theme($light-theme: true) {
  @if $light-theme {
    background-color: white;
    color: black;
  } @else {
    background-color: black;
    color: white;
  }
}

// main.scss
@use 'colors';

.button {
  background-color: colors.$primary;
  color: white;
}

.container {
  @include colors.theme(false);
}
```

### @forward指令(Sass 3.6+)

`@forward`允许从一个模块加载并重新导出样式，便于创建统一的入口点。

```scss
// _variables.scss
$primary-color: #3498db;
$secondary-color: #2ecc71;

// _functions.scss
@function calculate-width($col, $total: 12) {
  @return percentage($col / $total);
}

// _index.scss
@forward 'variables';
@forward 'functions';

// main.scss
@use 'index' as *;

.element {
  color: $primary-color;
  width: calculate-width(6);
}
```

### 模块配置

使用`with`关键字配置模块默认值。

```scss
// _theme.scss
$primary: #3498db !default;
$secondary: #2ecc71 !default;

.button {
  background-color: $primary;
  color: white;
}

// main.scss
@use 'theme' with (
  $primary: #9b59b6,
  $secondary: #f1c40f
);
```

**与Python模块系统对比**:
```python
# Python模块导入
import colors
from colors import primary, secondary
from utils import *

# Python模块配置
import config
config.DEBUG = True
```

## 最佳实践与架构模式

### 7-1模式

将Sass文件组织为7个文件夹和1个主文件。

```
sass/
|-- abstracts/        # 工具和辅助函数
|   |-- _variables.scss
|   |-- _functions.scss
|   |-- _mixins.scss
|-- base/             # 基础样式
|   |-- _reset.scss
|   |-- _typography.scss
|-- components/       # 组件样式
|   |-- _buttons.scss
|   |-- _cards.scss
|-- layout/           # 布局样式
|   |-- _header.scss
|   |-- _footer.scss
|-- pages/            # 页面特定样式
|   |-- _home.scss
|   |-- _about.scss
|-- themes/           # 主题样式
|   |-- _default.scss
|   |-- _dark.scss
|-- vendors/          # 第三方样式
|   |-- _bootstrap.scss
|-- main.scss         # 主文件
```

### BEM命名约定

BEM(Block Element Modifier)是一种CSS命名约定。

```scss
// Block
.card {
  // ...
  
  // Element
  &__header {
    // ...
  }
  
  &__body {
    // ...
  }
  
  // Modifier
  &--featured {
    // ...
  }
}

// 编译结果
.card { ... }
.card__header { ... }
.card__body { ... }
.card--featured { ... }
```

### ITCSS架构

Inverted Triangle CSS是一种从通用到特定的CSS组织方法。

```
1. Settings    - 变量和配置
2. Tools       - 混合和函数
3. Generic     - 重置和标准化
4. Elements    - 无类元素样式
5. Objects     - 布局和结构
6. Components  - UI组件
7. Utilities   - 辅助类
```

## Sass与现代前端框架的集成

### Vue与Sass

```vue
<template>
  <div class="component">
    <button class="component__button">点击我</button>
  </div>
</template>

<style lang="scss">
// 全局样式
</style>

<style lang="scss" scoped>
// 组件局部样式
$primary-color: #3498db;

.component {
  padding: 20px;
  
  &__button {
    background-color: $primary-color;
    color: white;
    padding: 10px 15px;
    border: none;
    border-radius: 4px;
    
    &:hover {
      background-color: darken($primary-color, 10%);
    }
  }
}
</style>
```

### React与Sass

**使用CSS模块**:

```jsx
// Button.jsx
import React from 'react';
import styles from './Button.module.scss';

const Button = ({ children, variant = 'primary' }) => {
  return (
    <button className={`${styles.button} ${styles[`button--${variant}`]}`}>
      {children}
    </button>
  );
};

export default Button;
```

```scss
// Button.module.scss
$primary-color: #3498db;
$success-color: #2ecc71;
$danger-color: #e74c3c;

.button {
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  
  &--primary {
    background-color: $primary-color;
    color: white;
    
    &:hover {
      background-color: darken($primary-color, 10%);
    }
  }
  
  &--success {
    background-color: $success-color;
    color: white;
    
    &:hover {
      background-color: darken($success-color, 10%);
    }
  }
  
  &--danger {
    background-color: $danger-color;
    color: white;
    
    &:hover {
      background-color: darken($danger-color, 10%);
    }
  }
}
```

### Angular与Sass

```typescript
// button.component.ts
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-button',
  templateUrl: './button.component.html',
  styleUrls: ['./button.component.scss']
})
export class ButtonComponent {
  @Input() variant: 'primary' | 'success' | 'danger' = 'primary';
}
```

```html
<!-- button.component.html -->
<button class="button" [ngClass]="'button--' + variant">
  <ng-content></ng-content>
</button>
```

```scss
// button.component.scss
$primary-color: #3498db;
$success-color: #2ecc71;
$danger-color: #e74c3c;

@mixin button-variant($color) {
  background-color: $color;
  color: white;
  
  &:hover {
    background-color: darken($color, 10%);
  }
}

.button {
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  
  &--primary {
    @include button-variant($primary-color);
  }
  
  &--success {
    @include button-variant($success-color);
  }
  
  &--danger {
    @include button-variant($danger-color);
  }
}
```

## 实战示例：高级主题系统

```scss
// _theme-config.scss
$themes: (
  light: (
    bg-primary: #ffffff,
    bg-secondary: #f5f5f5,
    text-primary: #333333,
    text-secondary: #666666,
    border: #dddddd,
    primary: #3498db,
    success: #2ecc71,
    warning: #f39c12,
    danger: #e74c3c
  ),
  dark: (
    bg-primary: #222222,
    bg-secondary: #333333,
    text-primary: #ffffff,
    text-secondary: #cccccc,
    border: #444444,
    primary: #3498db,
    success: #2ecc71,
    warning: #f39c12,
    danger: #e74c3c
  )
);

// _theme-functions.scss
@function theme-get($key, $theme-name: 'light') {
  $theme: map-get($themes, $theme-name);
  @return map-get($theme, $key);
}

// _theme-mixins.scss
@mixin themed() {
  @each $theme-name, $theme in $themes {
    .theme-#{$theme-name} & {
      @content($theme-name, $theme);
    }
  }
}

@mixin theme-aware($property, $key) {
  @include themed {
    #{$property}: theme-get($key, $theme-name);
  }
}

// _buttons.scss
.button {
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.3s ease;
  
  @include themed using ($theme-name, $theme) {
    background-color: map-get($theme, primary);
    color: map-get($theme, bg-primary);
    
    &:hover {
      background-color: darken(map-get($theme, primary), 10%);
    }
  }
  
  &--success {
    @include themed using ($theme-name, $theme) {
      background-color: map-get($theme, success);
      
      &:hover {
        background-color: darken(map-get($theme, success), 10%);
      }
    }
  }
  
  &--danger {
    @include themed using ($theme-name, $theme) {
      background-color: map-get($theme, danger);
      
      &:hover {
        background-color: darken(map-get($theme, danger), 10%);
      }
    }
  }
}

// _cards.scss
.card {
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  
  @include theme-aware('background-color', 'bg-primary');
  @include theme-aware('color', 'text-primary');
  @include theme-aware('border', 'border');
  
  &__header {
    padding: 15px;
    @include theme-aware('border-bottom', 'border');
    
    h3 {
      margin: 0;
      @include theme-aware('color', 'primary');
    }
  }
  
  &__body {
    padding: 15px;
    
    p {
      @include theme-aware('color', 'text-secondary');
    }
  }
  
  &__footer {
    padding: 15px;
    @include theme-aware('border-top', 'border');
    @include theme-aware('background-color', 'bg-secondary');
  }
}

// main.scss
@import 'theme-config';
@import 'theme-functions';
@import 'theme-mixins';
@import 'buttons';
@import 'cards';

// 默认使用亮色主题
body {
  @extend .theme-light;
}

// 主题切换
.theme-toggle {
  position: fixed;
  top: 20px;
  right: 20px;
  z-index: 1000;
}
```

**HTML使用示例**:

```html
<div class="theme-light">
  <!-- 或 class="theme-dark" -->
  <div class="card">
    <div class="card__header">
      <h3>卡片标题</h3>
    </div>
    <div class="card__body">
      <p>这是卡片内容，展示了主题系统的应用。</p>
    </div>
    <div class="card__footer">
      <button class="button">默认按钮</button>
      <button class="button button--success">成功按钮</button>
      <button class="button button--danger">危险按钮</button>
    </div>
  </div>
</div>

<button class="theme-toggle" onclick="document.body.classList.toggle('theme-dark')">
  切换主题
</button>
```

这个高级主题系统示例展示了如何使用Sass的条件语句、循环、函数和混合来创建一个灵活、可扩展的主题切换系统。
