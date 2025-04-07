---
type: docs
title: "实用技巧与模式"
---

## 常见布局实现

### 居中对齐

#### 水平居中

```css
/* 行内元素水平居中 */
.parent {
  text-align: center;
}

/* 块级元素水平居中 */
.child {
  margin: 0 auto;
  width: 50%; /* 必须设置宽度 */
}

/* Flexbox水平居中 */
.parent {
  display: flex;
  justify-content: center;
}

/* Grid水平居中 */
.parent {
  display: grid;
  justify-items: center;
}
```

#### 垂直居中

```css
/* 单行文本垂直居中 */
.text {
  height: 50px;
  line-height: 50px; /* 与height相同 */
}

/* Flexbox垂直居中 */
.parent {
  display: flex;
  align-items: center;
  height: 200px;
}

/* Grid垂直居中 */
.parent {
  display: grid;
  align-items: center;
  height: 200px;
}

/* 绝对定位垂直居中 */
.parent {
  position: relative;
  height: 200px;
}
.child {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
}
```

#### 水平垂直居中

```css
/* Flexbox方法 */
.parent {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 300px;
}

/* Grid方法 */
.parent {
  display: grid;
  place-items: center; /* align-items + justify-items 简写 */
  height: 300px;
}

/* 绝对定位方法 */
.parent {
  position: relative;
  height: 300px;
}
.child {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```

### 多列布局

#### 等宽多列

```css
/* Flexbox等宽多列 */
.container {
  display: flex;
}
.column {
  flex: 1; /* 所有列等宽 */
}

/* Grid等宽多列 */
.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr); /* 3列等宽 */
  gap: 20px;
}

/* CSS多列布局 */
.container {
  column-count: 3; /* 3列 */
  column-gap: 20px;
}
```

#### 不等宽多列

```css
/* Flexbox不等宽多列 */
.container {
  display: flex;
}
.sidebar {
  flex: 0 0 300px; /* 不增长不收缩，固定300px */
}
.content {
  flex: 1; /* 占用剩余空间 */
}

/* Grid不等宽多列 */
.container {
  display: grid;
  grid-template-columns: 300px 1fr; /* 左侧300px，右侧自适应 */
  gap: 20px;
}
```

### 粘性页脚

确保页脚始终在页面底部，即使内容不足以填充整个视口。

```css
/* Flexbox方法 */
body {
  display: flex;
  flex-direction: column;
  min-height: 100vh; /* 视口高度 */
  margin: 0;
}
.content {
  flex: 1; /* 占用所有可用空间 */
}
.footer {
  /* 页脚样式 */
}

/* Grid方法 */
body {
  display: grid;
  grid-template-rows: auto 1fr auto;
  min-height: 100vh;
  margin: 0;
}
.header {
  /* 页眉样式 */
}
.content {
  /* 内容样式 */
}
.footer {
  /* 页脚样式 */
}
```

## 组件设计思想

### 组件化 CSS 原则

1. **单一职责原则**：每个组件只负责一个功能
2. **封装性**：组件内部样式不影响外部
3. **可复用性**：组件可在不同场景重复使用
4. **可组合性**：小组件可组合成更复杂的组件

### 原子 CSS 方法

原子 CSS 是一种创建小型、单一用途类的方法。

```css
/* 间距类 */
.m-0 {
  margin: 0;
}
.m-1 {
  margin: 0.25rem;
}
.m-2 {
  margin: 0.5rem;
}
.m-3 {
  margin: 1rem;
}

.p-0 {
  padding: 0;
}
.p-1 {
  padding: 0.25rem;
}
.p-2 {
  padding: 0.5rem;
}
.p-3 {
  padding: 1rem;
}

/* 显示类 */
.d-block {
  display: block;
}
.d-flex {
  display: flex;
}
.d-grid {
  display: grid;
}

/* 对齐类 */
.text-center {
  text-align: center;
}
.text-left {
  text-align: left;
}
.text-right {
  text-align: right;
}

/* 颜色类 */
.text-primary {
  color: #3498db;
}
.bg-primary {
  background-color: #3498db;
}
```

使用原子类：

```html
<div class="d-flex p-3 bg-primary">
  <div class="m-2 p-2">项目1</div>
  <div class="m-2 p-2">项目2</div>
</div>
```

### 组件设计模式

#### 基础/修饰符模式

```css
/* 基础按钮 */
.btn {
  display: inline-block;
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

/* 修饰符 */
.btn--primary {
  background-color: #3498db;
  color: white;
}

.btn--success {
  background-color: #2ecc71;
  color: white;
}

.btn--large {
  padding: 0.75rem 1.5rem;
  font-size: 1.2rem;
}
```

```html
<button class="btn btn--primary">主要按钮</button>
<button class="btn btn--success btn--large">大号成功按钮</button>
```

#### 容器/内容模式

```css
/* 容器 */
.card {
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  overflow: hidden;
}

/* 内容 */
.card__header {
  padding: 1rem;
  border-bottom: 1px solid #eee;
}

.card__body {
  padding: 1rem;
}

.card__footer {
  padding: 1rem;
  border-top: 1px solid #eee;
  background-color: #f9f9f9;
}
```

```html
<div class="card">
  <div class="card__header">
    <h3>卡片标题</h3>
  </div>
  <div class="card__body">
    <p>卡片内容</p>
  </div>
  <div class="card__footer">
    <button class="btn btn--primary">确认</button>
  </div>
</div>
```

## 主题系统实现

### 使用 CSS 变量的主题系统

```css
/* 定义根变量 */
:root {
  /* 亮色主题（默认） */
  --bg-primary: #ffffff;
  --bg-secondary: #f5f5f5;
  --text-primary: #333333;
  --text-secondary: #666666;
  --border-color: #dddddd;
  --primary-color: #3498db;
  --success-color: #2ecc71;
  --warning-color: #f39c12;
  --danger-color: #e74c3c;
}

/* 暗色主题 */
.theme-dark {
  --bg-primary: #222222;
  --bg-secondary: #333333;
  --text-primary: #ffffff;
  --text-secondary: #cccccc;
  --border-color: #444444;
  /* 主题色可以保持不变或稍微调整 */
}

/* 使用变量 */
body {
  background-color: var(--bg-primary);
  color: var(--text-primary);
}

.card {
  background-color: var(--bg-primary);
  border: 1px solid var(--border-color);
}

.button {
  background-color: var(--primary-color);
  color: white;
}

.button--success {
  background-color: var(--success-color);
}
```

### 主题切换

```html
<button id="theme-toggle">切换主题</button>

<script>
  const themeToggle = document.getElementById("theme-toggle");
  themeToggle.addEventListener("click", () => {
    document.body.classList.toggle("theme-dark");

    // 保存用户偏好
    const isDarkTheme = document.body.classList.contains("theme-dark");
    localStorage.setItem("theme", isDarkTheme ? "dark" : "light");
  });

  // 加载保存的主题
  document.addEventListener("DOMContentLoaded", () => {
    const savedTheme = localStorage.getItem("theme");
    if (savedTheme === "dark") {
      document.body.classList.add("theme-dark");
    }
  });
</script>
```

### 使用媒体查询自动切换主题

```css
/* 默认亮色主题 */
:root {
  --bg-primary: #ffffff;
  --text-primary: #333333;
  /* 其他变量... */
}

/* 根据系统偏好自动切换到暗色主题 */
@media (prefers-color-scheme: dark) {
  :root {
    --bg-primary: #222222;
    --text-primary: #ffffff;
    /* 其他变量... */
  }
}

/* 用户手动选择的主题优先级更高 */
.theme-light {
  --bg-primary: #ffffff;
  --text-primary: #333333;
  /* 其他变量... */
}

.theme-dark {
  --bg-primary: #222222;
  --text-primary: #ffffff;
  /* 其他变量... */
}
```

## 性能优化技巧

### 选择器优化

```css
/* 避免 */
body div ul li a {
  ...;
} /* 过长的选择器链 */

/* 推荐 */
.nav-link {
  ...;
} /* 直接类选择器 */
```

### 减少重绘和回流

```css
/* 避免 */
.box {
  width: 300px;
  height: 200px;
  /* 分别设置会导致多次回流 */
  margin-top: 10px;
  margin-right: 10px;
  margin-bottom: 10px;
  margin-left: 10px;
}

/* 推荐 */
.box {
  width: 300px;
  height: 200px;
  margin: 10px; /* 简写属性只触发一次回流 */
}
```

### 使用 transform 和 opacity 进行动画

```css
/* 避免 */
@keyframes move-bad {
  0% {
    left: 0;
    top: 0;
  }
  100% {
    left: 100px;
    top: 100px;
  }
}

/* 推荐 */
@keyframes move-good {
  0% {
    transform: translate(0, 0);
  }
  100% {
    transform: translate(100px, 100px);
  }
}
```

### 使用 will-change 提示浏览器

```css
.animated-element {
  will-change: transform, opacity;
}
```

### 避免@import

```css
/* 避免 */
@import url("typography.css");
@import url("buttons.css");

/* 推荐 */
/* 使用<link>标签在HTML中引入多个CSS文件 */
/* 或使用构建工具合并CSS文件 */
```

## 实战示例：响应式导航栏

```html
<header class="header">
  <div class="container">
    <div class="logo">Brand</div>
    <button class="nav-toggle" aria-label="Toggle Navigation">
      <span class="nav-toggle__bar"></span>
      <span class="nav-toggle__bar"></span>
      <span class="nav-toggle__bar"></span>
    </button>
    <nav class="nav">
      <ul class="nav__list">
        <li class="nav__item"><a href="#" class="nav__link">首页</a></li>
        <li class="nav__item"><a href="#" class="nav__link">关于</a></li>
        <li class="nav__item"><a href="#" class="nav__link">服务</a></li>
        <li class="nav__item"><a href="#" class="nav__link">博客</a></li>
        <li class="nav__item"><a href="#" class="nav__link">联系</a></li>
      </ul>
    </nav>
  </div>
</header>
```

```css
:root {
  --primary-color: #3498db;
  --text-color: #333;
  --bg-color: #fff;
  --nav-height: 70px;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  color: var(--text-color);
  background-color: var(--bg-color);
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

.header {
  height: var(--nav-height);
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  position: sticky;
  top: 0;
  background-color: var(--bg-color);
  z-index: 100;
}

.header .container {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 100%;
}

.logo {
  font-size: 1.5rem;
  font-weight: bold;
  color: var(--primary-color);
}

.nav__list {
  display: flex;
  list-style: none;
}

.nav__item {
  margin-left: 30px;
}

.nav__link {
  color: var(--text-color);
  text-decoration: none;
  font-weight: 500;
  transition: color 0.3s ease;
  position: relative;
}

.nav__link::after {
  content: "";
  position: absolute;
  bottom: -5px;
  left: 0;
  width: 0;
  height: 2px;
  background-color: var(--primary-color);
  transition: width 0.3s ease;
}

.nav__link:hover {
  color: var(--primary-color);
}

.nav__link:hover::after {
  width: 100%;
}

.nav-toggle {
  display: none;
  background: none;
  border: none;
  cursor: pointer;
  padding: 5px;
}

.nav-toggle__bar {
  display: block;
  width: 25px;
  height: 3px;
  background-color: var(--text-color);
  margin: 5px 0;
  border-radius: 3px;
  transition: transform 0.3s ease, opacity 0.3s ease;
}

@media (max-width: 768px) {
  .nav-toggle {
    display: block;
    z-index: 101;
  }

  .nav {
    position: fixed;
    top: 0;
    right: 0;
    width: 80%;
    max-width: 300px;
    height: 100vh;
    background-color: var(--bg-color);
    box-shadow: -2px 0 10px rgba(0, 0, 0, 0.1);
    transform: translateX(100%);
    transition: transform 0.3s ease;
    z-index: 100;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .nav.active {
    transform: translateX(0);
  }

  .nav__list {
    flex-direction: column;
    align-items: center;
  }

  .nav__item {
    margin: 15px 0;
  }

  .nav-toggle.active .nav-toggle__bar:nth-child(1) {
    transform: translateY(8px) rotate(45deg);
  }

  .nav-toggle.active .nav-toggle__bar:nth-child(2) {
    opacity: 0;
  }

  .nav-toggle.active .nav-toggle__bar:nth-child(3) {
    transform: translateY(-8px) rotate(-45deg);
  }
}
```

```javascript
document.addEventListener("DOMContentLoaded", () => {
  const navToggle = document.querySelector(".nav-toggle");
  const nav = document.querySelector(".nav");

  navToggle.addEventListener("click", () => {
    navToggle.classList.toggle("active");
    nav.classList.toggle("active");
  });

  // 点击导航链接后关闭菜单
  const navLinks = document.querySelectorAll(".nav__link");
  navLinks.forEach((link) => {
    link.addEventListener("click", () => {
      navToggle.classList.remove("active");
      nav.classList.remove("active");
    });
  });
});
```

这个响应式导航栏示例展示了如何结合 CSS 和 JavaScript 创建在桌面和移动设备上都能良好工作的导航组件。它使用了 CSS 变量、Flexbox 布局、过渡动画和媒体查询等技术。
