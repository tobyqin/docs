# 01_CSS基础概念

## CSS 是什么及其作用

CSS（层叠样式表）是用于控制网页外观的语言。

**作用**：
- 分离内容(HTML)和表现(CSS)
- 统一管理网页样式
- 实现响应式设计

```css
/* 基本CSS语法 */
选择器 {
  属性: 值;
  属性2: 值2;
}
```

## CSS与HTML的关系

| HTML | CSS | Python类比 |
|------|-----|------------|
| 定义结构 | 定义样式 | 数据结构 vs 数据展示 |
| 内容 | 表现 | 类的属性 vs `__str__`方法 |

**三种引入CSS的方式**:

1. **内联样式**：直接在HTML元素中使用style属性
```html
<p style="color: blue; font-size: 16px;">这是蓝色文本</p>
```

2. **内部样式表**：在HTML文档头部使用`<style>`标签
```html
<head>
  <style>
    p {
      color: blue;
      font-size: 16px;
    }
  </style>
</head>
```

3. **外部样式表**：链接外部CSS文件（推荐）
```html
<head>
  <link rel="stylesheet" href="styles.css">
</head>
```

```css
/* styles.css */
p {
  color: blue;
  font-size: 16px;
}
```

## CSS选择器系统

| 选择器 | 示例 | 选择的元素 | Python类比 |
|--------|------|------------|------------|
| 元素选择器 | `p` | 所有`<p>`元素 | 按类型选择对象 |
| 类选择器 | `.intro` | 所有class="intro"的元素 | 按属性值筛选对象 |
| ID选择器 | `#firstname` | id="firstname"的元素 | 按唯一标识符查找对象 |
| 属性选择器 | `[type="text"]` | 所有type="text"的元素 | 按属性条件筛选 |

**组合选择器**:

```css
/* 后代选择器 */
div p {
  color: red;
}

/* 子元素选择器 */
div > p {
  color: blue;
}

/* 相邻兄弟选择器 */
h1 + p {
  margin-top: 0;
}

/* 通用兄弟选择器 */
h1 ~ p {
  color: green;
}
```

**伪类和伪元素**:

```css
/* 伪类 - 元素的特定状态 */
a:hover {
  color: red;
}

/* 伪元素 - 创建不存在于DOM的元素 */
p::first-line {
  font-weight: bold;
}
```

## CSS盒模型

每个HTML元素都被视为一个矩形盒子，由四个部分组成：

![CSS盒模型](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Box_Model/Introduction_to_the_CSS_box_model/boxmodel.png)

- **内容(Content)**: 元素的实际内容
- **内边距(Padding)**: 内容周围的空间
- **边框(Border)**: 包围内容和内边距
- **外边距(Margin)**: 元素与其他元素之间的空间

```css
div {
  /* 内容区域 */
  width: 300px;
  height: 200px;
  
  /* 内边距 */
  padding: 20px;
  /* 或分别设置 */
  padding-top: 10px;
  padding-right: 20px;
  padding-bottom: 10px;
  padding-left: 20px;
  
  /* 边框 */
  border: 1px solid black;
  /* 或分别设置 */
  border-width: 1px;
  border-style: solid;
  border-color: black;
  
  /* 外边距 */
  margin: 30px;
  /* 或分别设置 */
  margin-top: 10px;
  margin-right: 20px;
  margin-bottom: 10px;
  margin-left: 20px;
}
```

**盒模型计算**:

默认情况下(content-box)：
- 元素总宽度 = width + padding-left + padding-right + border-left + border-right
- 元素总高度 = height + padding-top + padding-bottom + border-top + border-bottom

使用`box-sizing: border-box`可以改变计算方式：
```css
div {
  box-sizing: border-box; /* width和height包含padding和border */
  width: 300px; /* 总宽度为300px */
  padding: 20px;
  border: 1px solid black;
}
```

## 练习示例

创建一个简单的卡片组件：

```html
<div class="card">
  <h2 class="card-title">卡片标题</h2>
  <p class="card-content">这是卡片内容，展示了CSS盒模型的应用。</p>
  <a href="#" class="card-button">了解更多</a>
</div>
```

```css
.card {
  width: 300px;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 8px;
  margin: 20px;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

.card-title {
  color: #333;
  margin-top: 0;
  border-bottom: 1px solid #eee;
  padding-bottom: 10px;
}

.card-content {
  color: #666;
  line-height: 1.5;
}

.card-button {
  display: inline-block;
  background-color: #0077cc;
  color: white;
  padding: 8px 16px;
  text-decoration: none;
  border-radius: 4px;
  margin-top: 10px;
}

.card-button:hover {
  background-color: #005fa3;
}
```

这个例子展示了选择器、盒模型和基本样式属性的应用。
