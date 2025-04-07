---
type: docs
title: "CSS样式与效果"
---

## 文本与字体样式

### 字体属性

```css
body {
  font-family: "Arial", sans-serif; /* 字体系列 */
  font-size: 16px; /* 字体大小 */
  font-weight: 400; /* 字体粗细: 100-900 或 normal, bold */
  font-style: normal; /* 字体样式: normal, italic, oblique */
  line-height: 1.5; /* 行高 */
  letter-spacing: 0.5px; /* 字母间距 */
  word-spacing: 2px; /* 单词间距 */
}
```

### 文本属性

```css
p {
  text-align: left; /* 文本对齐: left, right, center, justify */
  text-decoration: none; /* 文本装饰: none, underline, line-through, overline */
  text-transform: capitalize; /* 文本转换: none, uppercase, lowercase, capitalize */
  text-indent: 2em; /* 首行缩进 */
  white-space: nowrap; /* 空白处理: normal, nowrap, pre, pre-line, pre-wrap */
  overflow: hidden; /* 溢出处理: visible, hidden, scroll, auto */
  text-overflow: ellipsis; /* 文本溢出: clip, ellipsis */
}
```

### Web 字体

```css
/* 使用Google Fonts */
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;700&display=swap");

/* 或使用@font-face */
@font-face {
  font-family: "MyCustomFont";
  src: url("fonts/mycustomfont.woff2") format("woff2"), url("fonts/mycustomfont.woff")
      format("woff");
  font-weight: normal;
  font-style: normal;
}

body {
  font-family: "Roboto", sans-serif;
}

h1 {
  font-family: "MyCustomFont", serif;
}
```

示例：创建吸引人的标题和段落

```html
<h1 class="headline">精美排版示例</h1>
<p class="subheading">探索CSS中的文本与字体样式</p>
<p class="content">
  这是一段示例文本，展示了如何使用CSS控制文本的外观。通过调整字体、大小、间距和其他属性，可以创建出既美观又易读的排版效果。
</p>
```

```css
.headline {
  font-family: "Georgia", serif;
  font-size: 2.5rem;
  font-weight: 700;
  color: #2c3e50;
  margin-bottom: 0.5rem;
  letter-spacing: -0.5px;
}

.subheading {
  font-family: "Arial", sans-serif;
  font-size: 1.2rem;
  font-weight: 300;
  color: #7f8c8d;
  margin-top: 0;
  margin-bottom: 2rem;
}

.content {
  font-family: "Helvetica", sans-serif;
  font-size: 1rem;
  line-height: 1.6;
  color: #34495e;
  max-width: 600px;
  text-align: justify;
}
```

## 颜色与背景

### 颜色值表示法

```css
.color-examples {
  color: red; /* 颜色名称 */
  color: #ff0000; /* 十六进制 */
  color: #f00; /* 十六进制简写 */
  color: rgb(255, 0, 0); /* RGB */
  color: rgba(255, 0, 0, 0.5); /* RGBA (带透明度) */
  color: hsl(0, 100%, 50%); /* HSL (色相, 饱和度, 亮度) */
  color: hsla(0, 100%, 50%, 0.5); /* HSLA (带透明度) */
}
```

### 背景属性

```css
.background-examples {
  background-color: #f0f0f0; /* 背景颜色 */
  background-image: url("bg.jpg"); /* 背景图片 */
  background-repeat: no-repeat; /* 背景重复: repeat, no-repeat, repeat-x, repeat-y */
  background-position: center; /* 背景位置 */
  background-size: cover; /* 背景尺寸: auto, cover, contain, 具体尺寸 */
  background-attachment: fixed; /* 背景附着: scroll, fixed, local */

  /* 简写形式 */
  background: #f0f0f0 url("bg.jpg") no-repeat center/cover fixed;
}
```

### 渐变背景

```css
/* 线性渐变 */
.linear-gradient {
  background: linear-gradient(to right, #3498db, #2ecc71);
  /* 或 */
  background: linear-gradient(45deg, #3498db, #2ecc71);
}

/* 径向渐变 */
.radial-gradient {
  background: radial-gradient(circle, #3498db, #2ecc71);
}

/* 多色渐变 */
.multi-color-gradient {
  background: linear-gradient(to right, #f1c40f, #e74c3c, #9b59b6);
}

/* 重复渐变 */
.repeating-gradient {
  background: repeating-linear-gradient(
    45deg,
    #3498db,
    #3498db 10px,
    #2ecc71 10px,
    #2ecc71 20px
  );
}
```

示例：创建渐变按钮

```html
<button class="gradient-button">点击我</button>
```

```css
.gradient-button {
  padding: 12px 24px;
  border: none;
  border-radius: 4px;
  background: linear-gradient(to right, #3498db, #2980b9);
  color: white;
  font-size: 16px;
  cursor: pointer;
  transition: transform 0.3s ease;
}

.gradient-button:hover {
  background: linear-gradient(to right, #2980b9, #3498db);
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}
```

## 变换与动画

### 变换(Transform)

```css
.transform-examples {
  /* 平移 */
  transform: translateX(20px);
  transform: translateY(20px);
  transform: translate(20px, 20px);

  /* 缩放 */
  transform: scaleX(1.5);
  transform: scaleY(1.5);
  transform: scale(1.5);

  /* 旋转 */
  transform: rotate(45deg);

  /* 倾斜 */
  transform: skewX(10deg);
  transform: skewY(10deg);
  transform: skew(10deg, 10deg);

  /* 组合变换 (从右到左应用) */
  transform: rotate(45deg) scale(1.5) translate(20px, 20px);

  /* 3D变换 */
  transform: rotateX(45deg);
  transform: rotateY(45deg);
  transform: rotateZ(45deg);
  transform: perspective(500px) rotateY(45deg);
}
```

### 过渡(Transition)

```css
.transition-examples {
  /* 单个属性过渡 */
  transition-property: background-color;
  transition-duration: 0.3s;
  transition-timing-function: ease;
  transition-delay: 0s;

  /* 简写形式 */
  transition: background-color 0.3s ease 0s;

  /* 多属性过渡 */
  transition: background-color 0.3s ease, transform 0.5s ease-out;

  /* 所有属性过渡 */
  transition: all 0.3s ease;
}
```

### 动画(Animation)

```css
/* 定义关键帧 */
@keyframes slide-in {
  0% {
    transform: translateX(-100%);
    opacity: 0;
  }
  100% {
    transform: translateX(0);
    opacity: 1;
  }
}

.animation-examples {
  /* 单独属性 */
  animation-name: slide-in;
  animation-duration: 1s;
  animation-timing-function: ease-out;
  animation-delay: 0s;
  animation-iteration-count: 1;
  animation-direction: normal;
  animation-fill-mode: forwards;
  animation-play-state: running;

  /* 简写形式 */
  animation: slide-in 1s ease-out 0s 1 normal forwards running;
}
```

示例：创建一个加载动画

```html
<div class="loader"></div>
```

```css
.loader {
  width: 50px;
  height: 50px;
  border: 5px solid #f3f3f3;
  border-top: 5px solid #3498db;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
```

## 过渡效果实战

### 卡片悬停效果

```html
<div class="card">
  <img src="image.jpg" alt="卡片图片" />
  <div class="card-content">
    <h3>卡片标题</h3>
    <p>卡片描述文本，展示悬停效果。</p>
  </div>
</div>
```

```css
.card {
  width: 300px;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
}

.card img {
  width: 100%;
  height: 200px;
  object-fit: cover;
  transition: transform 0.5s ease;
}

.card-content {
  padding: 20px;
}

.card:hover {
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
  transform: translateY(-5px);
}

.card:hover img {
  transform: scale(1.1);
}
```

### 按钮动画效果

```html
<button class="animated-button">点击我</button>
```

```css
.animated-button {
  position: relative;
  padding: 12px 24px;
  background-color: #3498db;
  color: white;
  border: none;
  border-radius: 4px;
  overflow: hidden;
  cursor: pointer;
  z-index: 1;
}

.animated-button::before {
  content: "";
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    90deg,
    transparent,
    rgba(255, 255, 255, 0.2),
    transparent
  );
  transition: left 0.7s ease;
  z-index: -1;
}

.animated-button:hover::before {
  left: 100%;
}

.animated-button:active {
  transform: scale(0.95);
}
```

### 页面过渡动画

```html
<div class="page-container">
  <div class="page page1">页面1</div>
  <div class="page page2">页面2</div>
</div>
```

```css
.page-container {
  position: relative;
  width: 100%;
  height: 100vh;
  overflow: hidden;
}

.page {
  position: absolute;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 2rem;
  transition: transform 0.5s ease-in-out;
}

.page1 {
  background-color: #3498db;
  color: white;
  transform: translateX(0);
}

.page2 {
  background-color: #2ecc71;
  color: white;
  transform: translateX(100%);
}

/* 当添加active类时切换页面 */
.page-container.active .page1 {
  transform: translateX(-100%);
}

.page-container.active .page2 {
  transform: translateX(0);
}
```

```javascript
// 切换页面的JavaScript
document
  .querySelector(".page-container")
  .addEventListener("click", function () {
    this.classList.toggle("active");
  });
```

这些示例展示了如何使用 CSS 创建各种视觉效果，从简单的颜色变化到复杂的动画和过渡。通过组合这些技术，可以创建出既美观又具有交互性的用户界面。
