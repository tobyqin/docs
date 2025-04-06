# 02_CSS布局技术

## 传统布局方法

### 定位(Position)

控制元素在页面中的确切位置。

```css
/* 定位类型 */
.element {
  position: static;    /* 默认值，按正常文档流排列 */
  position: relative;  /* 相对于原位置偏移 */
  position: absolute;  /* 相对于最近的定位祖先元素偏移 */
  position: fixed;     /* 相对于视口固定位置 */
  position: sticky;    /* 根据滚动位置在relative和fixed之间切换 */
}
```

示例：创建一个固定在页面右下角的按钮
```css
.floating-button {
  position: fixed;
  bottom: 20px;
  right: 20px;
  width: 50px;
  height: 50px;
  background-color: #0077cc;
  border-radius: 50%;
  color: white;
  text-align: center;
  line-height: 50px;
  box-shadow: 0 2px 5px rgba(0,0,0,0.3);
}
```

### 浮动(Float)

使元素脱离正常文档流，向左或向右浮动。

```css
.left {
  float: left;   /* 元素浮动到左侧 */
}

.right {
  float: right;  /* 元素浮动到右侧 */
}

.clear {
  clear: both;   /* 清除两侧浮动 */
}
```

示例：创建简单的两列布局
```html
<div class="container">
  <div class="sidebar">侧边栏</div>
  <div class="content">主要内容区域</div>
  <div class="clear"></div>
</div>
```

```css
.container {
  width: 100%;
}

.sidebar {
  float: left;
  width: 25%;
  background-color: #f0f0f0;
  padding: 20px;
  box-sizing: border-box;
}

.content {
  float: left;
  width: 75%;
  padding: 20px;
  box-sizing: border-box;
}

.clear {
  clear: both;
}
```

## Flexbox布局

Flexbox是一维布局系统，适用于一行或一列的布局。

### 基本概念

```css
.container {
  display: flex;           /* 启用Flexbox */
  flex-direction: row;     /* 主轴方向: row(默认), column, row-reverse, column-reverse */
  justify-content: center; /* 主轴对齐: flex-start, flex-end, center, space-between, space-around */
  align-items: center;     /* 交叉轴对齐: flex-start, flex-end, center, stretch, baseline */
  flex-wrap: wrap;         /* 换行: nowrap(默认), wrap, wrap-reverse */
}

.item {
  flex: 1;                 /* 简写: flex-grow, flex-shrink, flex-basis */
  /* 等同于: */
  flex-grow: 1;            /* 增长比例 */
  flex-shrink: 1;          /* 收缩比例 */
  flex-basis: auto;        /* 初始大小 */
}
```

示例：响应式导航栏
```html
<nav class="navbar">
  <div class="logo">Logo</div>
  <ul class="nav-links">
    <li><a href="#">首页</a></li>
    <li><a href="#">关于</a></li>
    <li><a href="#">服务</a></li>
    <li><a href="#">联系</a></li>
  </ul>
</nav>
```

```css
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 2rem;
  background-color: #333;
  color: white;
}

.nav-links {
  display: flex;
  list-style: none;
  margin: 0;
  padding: 0;
}

.nav-links li {
  margin-left: 1rem;
}

.nav-links a {
  color: white;
  text-decoration: none;
}

/* 响应式设计 */
@media (max-width: 768px) {
  .navbar {
    flex-direction: column;
    align-items: flex-start;
  }
  
  .nav-links {
    margin-top: 1rem;
    flex-direction: column;
    width: 100%;
  }
  
  .nav-links li {
    margin: 0.5rem 0;
  }
}
```

## Grid布局

Grid是二维布局系统，适用于行和列的布局。

### 基本语法

```css
.container {
  display: grid;
  grid-template-columns: 1fr 2fr 1fr;  /* 三列，比例为1:2:1 */
  grid-template-rows: 100px auto 100px; /* 三行，第一行和第三行高100px，中间行自适应 */
  gap: 10px;                           /* 行列间距 */
}

.item {
  grid-column: 1 / 3;  /* 从第1列线到第3列线 (跨2列) */
  grid-row: 2 / 3;     /* 从第2行线到第3行线 (跨1行) */
}
```

示例：创建一个典型的网站布局
```html
<div class="grid-layout">
  <header class="header">页眉</header>
  <nav class="sidebar">侧边栏</nav>
  <main class="content">主要内容</main>
  <footer class="footer">页脚</footer>
</div>
```

```css
.grid-layout {
  display: grid;
  grid-template-areas: 
    "header header header"
    "sidebar content content"
    "footer footer footer";
  grid-template-columns: 1fr 3fr;
  grid-template-rows: auto 1fr auto;
  min-height: 100vh;
  gap: 10px;
}

.header {
  grid-area: header;
  background-color: #333;
  color: white;
  padding: 1rem;
}

.sidebar {
  grid-area: sidebar;
  background-color: #f0f0f0;
  padding: 1rem;
}

.content {
  grid-area: content;
  padding: 1rem;
}

.footer {
  grid-area: footer;
  background-color: #333;
  color: white;
  padding: 1rem;
}

/* 响应式设计 */
@media (max-width: 768px) {
  .grid-layout {
    grid-template-areas: 
      "header"
      "sidebar"
      "content"
      "footer";
    grid-template-columns: 1fr;
  }
}
```

## 响应式设计原则

### 媒体查询

根据设备特性应用不同的样式。

```css
/* 基本语法 */
@media screen and (max-width: 768px) {
  /* 当屏幕宽度小于等于768px时应用的样式 */
  body {
    font-size: 14px;
  }
}

/* 常用断点 */
/* 手机 */
@media (max-width: 576px) { ... }

/* 平板 */
@media (min-width: 577px) and (max-width: 992px) { ... }

/* 桌面 */
@media (min-width: 993px) { ... }
```

### 视口单位

相对于视口大小的单位。

```css
.responsive-element {
  width: 50vw;  /* 视口宽度的50% */
  height: 50vh; /* 视口高度的50% */
  font-size: 5vmin; /* 视口宽度或高度中较小值的5% */
}
```

### 流体布局与弹性图片

```css
.container {
  max-width: 1200px;
  width: 100%;
  margin: 0 auto;
}

img {
  max-width: 100%;
  height: auto;
}
```

## 实战示例：响应式卡片网格

```html
<div class="card-grid">
  <div class="card">卡片1</div>
  <div class="card">卡片2</div>
  <div class="card">卡片3</div>
  <div class="card">卡片4</div>
  <div class="card">卡片5</div>
  <div class="card">卡片6</div>
</div>
```

```css
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 20px;
  padding: 20px;
}

.card {
  background-color: white;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
  transition: transform 0.3s ease;
}

.card:hover {
  transform: translateY(-5px);
}

/* 在小屏幕上增加间距 */
@media (max-width: 576px) {
  .card-grid {
    gap: 15px;
    padding: 15px;
  }
}
```

这个示例自动适应不同屏幕尺寸，卡片数量会根据可用空间自动调整，确保每个卡片至少250px宽，但可以更宽以填充可用空间。
