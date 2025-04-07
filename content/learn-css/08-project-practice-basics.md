---
type: docs
title: "项目实战_基础"
---

## 从零构建响应式网页

本章将通过一个实际项目，展示如何应用前面学习的 CSS 和 Sass 知识。我们将创建一个简单的响应式网页，包含导航栏、英雄区、特性展示和页脚等常见组件。

### 项目结构

```
project/
├── index.html
├── css/
│   └── main.css
├── scss/
│   ├── _variables.scss
│   ├── _mixins.scss
│   ├── _reset.scss
│   ├── _typography.scss
│   ├── _components.scss
│   ├── _layout.scss
│   └── main.scss
└── images/
    └── hero-bg.jpg
```

### HTML 结构

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>响应式网页示例</title>
    <link rel="stylesheet" href="css/main.css" />
  </head>
  <body>
    <!-- 导航栏 -->
    <header class="header">
      <div class="container">
        <div class="logo">Brand</div>
        <nav class="nav">
          <button class="nav-toggle" aria-label="Toggle Navigation">
            <span></span>
            <span></span>
            <span></span>
          </button>
          <ul class="nav-menu">
            <li><a href="#" class="nav-link">首页</a></li>
            <li><a href="#" class="nav-link">特性</a></li>
            <li><a href="#" class="nav-link">价格</a></li>
            <li><a href="#" class="nav-link">关于</a></li>
            <li><a href="#" class="nav-link btn btn-primary">注册</a></li>
          </ul>
        </nav>
      </div>
    </header>

    <!-- 英雄区 -->
    <section class="hero">
      <div class="container">
        <div class="hero-content">
          <h1>创建美观的响应式网站</h1>
          <p>使用现代CSS和Sass技术，轻松构建专业级网站</p>
          <div class="hero-buttons">
            <a href="#" class="btn btn-primary">开始使用</a>
            <a href="#" class="btn btn-outline">了解更多</a>
          </div>
        </div>
      </div>
    </section>

    <!-- 特性区 -->
    <section class="features">
      <div class="container">
        <h2 class="section-title">核心特性</h2>
        <div class="features-grid">
          <div class="feature-card">
            <div class="feature-icon">🎨</div>
            <h3>现代设计</h3>
            <p>采用最新的设计趋势，创造出色的用户体验</p>
          </div>
          <div class="feature-card">
            <div class="feature-icon">📱</div>
            <h3>完全响应式</h3>
            <p>在任何设备上都能完美展示</p>
          </div>
          <div class="feature-card">
            <div class="feature-icon">⚡</div>
            <h3>性能优化</h3>
            <p>快速加载，流畅交互</p>
          </div>
          <div class="feature-card">
            <div class="feature-icon">🔧</div>
            <h3>易于定制</h3>
            <p>简单调整以适应您的品牌</p>
          </div>
        </div>
      </div>
    </section>

    <!-- 页脚 -->
    <footer class="footer">
      <div class="container">
        <div class="footer-content">
          <div class="footer-logo">Brand</div>
          <div class="footer-links">
            <div class="footer-column">
              <h4>产品</h4>
              <ul>
                <li><a href="#">特性</a></li>
                <li><a href="#">价格</a></li>
                <li><a href="#">案例</a></li>
              </ul>
            </div>
            <div class="footer-column">
              <h4>资源</h4>
              <ul>
                <li><a href="#">文档</a></li>
                <li><a href="#">教程</a></li>
                <li><a href="#">博客</a></li>
              </ul>
            </div>
            <div class="footer-column">
              <h4>公司</h4>
              <ul>
                <li><a href="#">关于我们</a></li>
                <li><a href="#">联系我们</a></li>
                <li><a href="#">招聘</a></li>
              </ul>
            </div>
          </div>
        </div>
        <div class="footer-bottom">
          <p>&copy; 2025 Brand. 保留所有权利。</p>
        </div>
      </div>
    </footer>

    <script>
      // 简单的导航菜单切换
      document
        .querySelector(".nav-toggle")
        .addEventListener("click", function () {
          document.querySelector(".nav-menu").classList.toggle("active");
          this.classList.toggle("active");
        });
    </script>
  </body>
</html>
```

### Sass 文件

#### \_variables.scss

```scss
// 颜色
$primary-color: #3498db;
$secondary-color: #2ecc71;
$dark-color: #333333;
$light-color: #f4f4f4;
$gray-color: #888888;
$white-color: #ffffff;

// 字体
$font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
$font-sizes: (
  small: 0.875rem,
  normal: 1rem,
  medium: 1.25rem,
  large: 1.5rem,
  xl: 2rem,
  xxl: 3rem,
);

// 布局
$container-width: 1200px;
$gutter: 20px;

// 断点
$breakpoints: (
  sm: 576px,
  md: 768px,
  lg: 992px,
  xl: 1200px,
);

// 其他
$border-radius: 4px;
$box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
$transition: all 0.3s ease;
```

#### \_mixins.scss

```scss
// 媒体查询
@mixin respond-to($breakpoint) {
  $value: map-get($breakpoints, $breakpoint);

  @if $value != null {
    @media (max-width: $value) {
      @content;
    }
  } @else {
    @error "Unknown breakpoint: #{$breakpoint}.";
  }
}

// Flexbox
@mixin flex($direction: row, $justify: flex-start, $align: stretch) {
  display: flex;
  flex-direction: $direction;
  justify-content: $justify;
  align-items: $align;
}

// 按钮样式
@mixin button-style($bg-color, $text-color, $hover-darken: 10%) {
  background-color: $bg-color;
  color: $text-color;

  &:hover {
    background-color: darken($bg-color, $hover-darken);
  }
}

// 卡片样式
@mixin card {
  background-color: $white-color;
  border-radius: $border-radius;
  box-shadow: $box-shadow;
  padding: $gutter;
}
```

#### \_reset.scss

```scss
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  font-size: 16px;
  scroll-behavior: smooth;
}

body {
  font-family: $font-family;
  line-height: 1.6;
  color: $dark-color;
  background-color: $white-color;
}

a {
  text-decoration: none;
  color: $primary-color;
}

ul {
  list-style: none;
}

img {
  max-width: 100%;
  height: auto;
}
```

#### \_typography.scss

```scss
h1,
h2,
h3,
h4,
h5,
h6 {
  font-weight: 700;
  line-height: 1.2;
  margin-bottom: 1rem;
}

h1 {
  font-size: map-get($font-sizes, xxl);

  @include respond-to(md) {
    font-size: map-get($font-sizes, xl);
  }
}

h2 {
  font-size: map-get($font-sizes, xl);

  @include respond-to(md) {
    font-size: map-get($font-sizes, large);
  }
}

h3 {
  font-size: map-get($font-sizes, large);
}

h4 {
  font-size: map-get($font-sizes, medium);
}

p {
  margin-bottom: 1rem;
}

.section-title {
  text-align: center;
  margin-bottom: 2rem;

  &::after {
    content: "";
    display: block;
    width: 50px;
    height: 3px;
    background-color: $primary-color;
    margin: 1rem auto 0;
  }
}
```

#### \_components.scss

```scss
// 按钮
.btn {
  display: inline-block;
  padding: 0.75rem 1.5rem;
  border-radius: $border-radius;
  font-weight: 600;
  text-align: center;
  cursor: pointer;
  transition: $transition;
  border: none;

  &-primary {
    @include button-style($primary-color, $white-color);
  }

  &-secondary {
    @include button-style($secondary-color, $white-color);
  }

  &-outline {
    background-color: transparent;
    border: 2px solid $primary-color;
    color: $primary-color;

    &:hover {
      background-color: $primary-color;
      color: $white-color;
    }
  }
}

// 卡片
.feature-card {
  @include card;
  text-align: center;
  transition: $transition;

  &:hover {
    transform: translateY(-5px);
  }

  .feature-icon {
    font-size: 3rem;
    margin-bottom: 1rem;
  }
}

// 导航菜单
.nav-toggle {
  display: none;
  background: none;
  border: none;
  cursor: pointer;

  span {
    display: block;
    width: 25px;
    height: 3px;
    background-color: $dark-color;
    margin: 5px 0;
    transition: $transition;
  }

  &.active {
    span:nth-child(1) {
      transform: rotate(45deg) translate(5px, 5px);
    }

    span:nth-child(2) {
      opacity: 0;
    }

    span:nth-child(3) {
      transform: rotate(-45deg) translate(7px, -6px);
    }
  }

  @include respond-to(md) {
    display: block;
  }
}
```

#### \_layout.scss

```scss
// 容器
.container {
  max-width: $container-width;
  margin: 0 auto;
  padding: 0 $gutter;
}

// 头部
.header {
  padding: 1rem 0;
  box-shadow: $box-shadow;
  position: sticky;
  top: 0;
  background-color: $white-color;
  z-index: 100;

  .container {
    @include flex(row, space-between, center);
  }

  .logo {
    font-size: map-get($font-sizes, large);
    font-weight: 700;
    color: $primary-color;
  }

  .nav {
    @include flex(row, flex-end, center);

    .nav-menu {
      @include flex(row, flex-end, center);

      li {
        margin-left: 1.5rem;
      }

      .nav-link {
        color: $dark-color;
        font-weight: 500;
        transition: $transition;

        &:hover {
          color: $primary-color;
        }
      }
    }

    @include respond-to(md) {
      .nav-menu {
        position: fixed;
        top: 0;
        right: -100%;
        width: 80%;
        max-width: 300px;
        height: 100vh;
        background-color: $white-color;
        @include flex(column, flex-start, flex-start);
        padding: 5rem 2rem;
        box-shadow: -2px 0 10px rgba(0, 0, 0, 0.1);
        transition: $transition;

        &.active {
          right: 0;
        }

        li {
          margin: 1rem 0;
          width: 100%;
        }

        .nav-link {
          display: block;
          padding: 0.5rem 0;
        }
      }
    }
  }
}

// 英雄区
.hero {
  background-image: linear-gradient(rgba(0, 0, 0, 0.7), rgba(0, 0, 0, 0.7)),
    url("../images/hero-bg.jpg");
  background-size: cover;
  background-position: center;
  color: $white-color;
  padding: 5rem 0;

  .hero-content {
    max-width: 600px;

    h1 {
      margin-bottom: 1rem;
    }

    p {
      font-size: map-get($font-sizes, medium);
      margin-bottom: 2rem;
    }

    .hero-buttons {
      .btn {
        margin-right: 1rem;
        margin-bottom: 1rem;
      }
    }
  }

  @include respond-to(md) {
    text-align: center;

    .hero-content {
      margin: 0 auto;
    }
  }
}

// 特性区
.features {
  padding: 5rem 0;

  .features-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: $gutter;
  }
}

// 页脚
.footer {
  background-color: $dark-color;
  color: $light-color;
  padding: 3rem 0 1rem;

  .footer-content {
    @include flex(row, space-between, flex-start);
    margin-bottom: 2rem;

    .footer-logo {
      font-size: map-get($font-sizes, large);
      font-weight: 700;
      color: $white-color;
    }

    .footer-links {
      @include flex(row, flex-end, flex-start);
      flex-wrap: wrap;

      .footer-column {
        margin-left: 3rem;

        h4 {
          color: $white-color;
          margin-bottom: 1rem;
        }

        ul li {
          margin-bottom: 0.5rem;

          a {
            color: $light-color;
            transition: $transition;

            &:hover {
              color: $primary-color;
            }
          }
        }
      }
    }
  }

  .footer-bottom {
    border-top: 1px solid rgba(255, 255, 255, 0.1);
    padding-top: 1rem;
    text-align: center;
    font-size: map-get($font-sizes, small);
  }

  @include respond-to(md) {
    .footer-content {
      flex-direction: column;

      .footer-logo {
        margin-bottom: 2rem;
      }

      .footer-links {
        justify-content: flex-start;

        .footer-column {
          margin-left: 0;
          margin-right: 2rem;
          margin-bottom: 2rem;
        }
      }
    }
  }
}
```

#### main.scss

```scss
// 导入变量和混合
@import "variables";
@import "mixins";

// 导入基础样式
@import "reset";
@import "typography";

// 导入组件和布局
@import "components";
@import "layout";
```

### 编译 Sass

使用命令行编译 Sass：

```bash
# 安装Sass
npm install -g sass

# 编译
sass scss/main.scss css/main.css

# 监视文件变化
sass --watch scss/main.scss:css/main.css
```

### 项目要点

1. **模块化结构**：将样式分成多个文件，便于维护
2. **响应式设计**：使用媒体查询适配不同设备
3. **组件化思想**：创建可复用的 UI 组件
4. **Sass 功能应用**：变量、混合、嵌套等
5. **性能考虑**：优化选择器和样式

这个项目展示了如何将前面学习的 CSS 和 Sass 知识应用到实际开发中，创建一个完整的响应式网页。
