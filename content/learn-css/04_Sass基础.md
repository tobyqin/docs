# 04_Sass基础

## Sass是什么及为什么使用它

Sass(Syntactically Awesome StyleSheets)是CSS的预处理器，扩展了CSS的功能。

**优势对比：**

| CSS | Sass | Python类比 |
|-----|------|------------|
| 不支持变量 | 支持变量 | Python变量 |
| 不支持嵌套 | 支持嵌套 | Python缩进块 |
| 代码重复 | 支持复用(Mixins) | Python函数 |
| 无逻辑控制 | 支持逻辑控制 | Python条件和循环 |

**为什么使用Sass：**
- 提高开发效率
- 代码更易维护
- 更好的组织大型样式表
- 减少重复代码

## Sass的安装与配置

### 安装方法

**使用npm安装：**
```bash
npm install -g sass
```

**使用Homebrew安装(Mac)：**
```bash
brew install sass/sass/sass
```

### 编译Sass文件

**命令行编译：**
```bash
# 单文件编译
sass input.scss output.css

# 监视文件变化
sass --watch input.scss:output.css

# 监视整个目录
sass --watch scss/:css/
```

**压缩输出：**
```bash
sass input.scss output.css --style compressed
```

### 在项目中配置

**使用package.json脚本：**
```json
{
  "scripts": {
    "sass": "sass --watch scss/:css/",
    "build:css": "sass scss/:css/ --style compressed"
  }
}
```

**与构建工具集成：**

webpack配置示例：
```javascript
// webpack.config.js
module.exports = {
  // ...
  module: {
    rules: [
      {
        test: /\.scss$/,
        use: [
          'style-loader', // 将CSS注入到DOM
          'css-loader',   // 解析CSS
          'sass-loader'   // 将Sass编译为CSS
        ]
      }
    ]
  }
};
```

## SCSS vs Sass语法对比

Sass有两种语法：SCSS(Sassy CSS)和缩进语法(Sass)。

### SCSS语法(推荐)

使用花括号和分号，类似CSS。

```scss
// SCSS语法
$primary-color: #3498db;

.container {
  width: 100%;
  max-width: 1200px;
  
  .header {
    background-color: $primary-color;
    color: white;
  }
}
```

### Sass语法(缩进语法)

使用缩进和换行，没有花括号和分号。

```sass
// Sass语法
$primary-color: #3498db

.container
  width: 100%
  max-width: 1200px
  
  .header
    background-color: $primary-color
    color: white
```

**选择建议：** 大多数项目使用SCSS语法，因为它与CSS兼容，学习曲线更平缓。

## 从CSS到Sass的过渡

### 将CSS转换为Sass

任何有效的CSS文件都是有效的SCSS文件，可以直接重命名为`.scss`并开始添加Sass功能。

**示例：从CSS到SCSS的渐进转换**

原始CSS：
```css
/* styles.css */
.button {
  background-color: #3498db;
  color: white;
  padding: 10px 20px;
  border-radius: 4px;
}

.button:hover {
  background-color: #2980b9;
}

.button.large {
  padding: 15px 30px;
  font-size: 18px;
}
```

转换为SCSS：
```scss
/* styles.scss */
$blue: #3498db;
$dark-blue: #2980b9;

.button {
  background-color: $blue;
  color: white;
  padding: 10px 20px;
  border-radius: 4px;
  
  &:hover {
    background-color: $dark-blue;
  }
  
  &.large {
    padding: 15px 30px;
    font-size: 18px;
  }
}
```

### 组织Sass文件

**推荐的文件结构：**

```
scss/
|-- abstracts/
|   |-- _variables.scss    // 变量
|   |-- _mixins.scss       // 混合
|   |-- _functions.scss    // 函数
|-- base/
|   |-- _reset.scss        // 重置样式
|   |-- _typography.scss   // 排版
|-- components/
|   |-- _buttons.scss      // 按钮
|   |-- _cards.scss        // 卡片
|-- layout/
|   |-- _header.scss       // 页眉
|   |-- _footer.scss       // 页脚
|-- pages/
|   |-- _home.scss         // 首页特定样式
|-- vendors/
|   |-- _bootstrap.scss    // 第三方库
|-- main.scss              // 主文件，导入所有部分
```

**主文件示例：**

```scss
// main.scss

// 抽象部分
@import 'abstracts/variables';
@import 'abstracts/mixins';
@import 'abstracts/functions';

// 基础部分
@import 'base/reset';
@import 'base/typography';

// 组件
@import 'components/buttons';
@import 'components/cards';

// 布局
@import 'layout/header';
@import 'layout/footer';

// 页面
@import 'pages/home';

// 第三方
@import 'vendors/bootstrap';
```

## 实战示例：创建一个主题系统

```scss
// _variables.scss
// 定义主题颜色
$themes: (
  light: (
    bg-color: #ffffff,
    text-color: #333333,
    primary-color: #3498db,
    secondary-color: #2ecc71,
    border-color: #dddddd
  ),
  dark: (
    bg-color: #222222,
    text-color: #f5f5f5,
    primary-color: #3498db,
    secondary-color: #2ecc71,
    border-color: #444444
  )
);

// _mixins.scss
// 主题混合
@mixin themed() {
  @each $theme, $map in $themes {
    .theme-#{$theme} & {
      $theme-map: () !global;
      @each $key, $value in $map {
        $theme-map: map-merge($theme-map, ($key: $value)) !global;
      }
      @content;
      $theme-map: null !global;
    }
  }
}

@function t($key) {
  @return map-get($theme-map, $key);
}

// _buttons.scss
.button {
  padding: 10px 20px;
  border-radius: 4px;
  border: none;
  cursor: pointer;
  
  @include themed() {
    background-color: t('primary-color');
    color: t('bg-color');
    border: 1px solid t('border-color');
  }
  
  &:hover {
    @include themed() {
      background-color: darken(t('primary-color'), 10%);
    }
  }
}

// _cards.scss
.card {
  border-radius: 8px;
  padding: 20px;
  margin-bottom: 20px;
  
  @include themed() {
    background-color: t('bg-color');
    color: t('text-color');
    border: 1px solid t('border-color');
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  }
  
  .card-title {
    margin-top: 0;
    
    @include themed() {
      color: t('primary-color');
    }
  }
}

// 使用示例
// HTML
// <div class="theme-light">
//   <div class="card">
//     <h2 class="card-title">Light Theme Card</h2>
//     <p>This is a card with light theme.</p>
//     <button class="button">Click Me</button>
//   </div>
// </div>
//
// <div class="theme-dark">
//   <div class="card">
//     <h2 class="card-title">Dark Theme Card</h2>
//     <p>This is a card with dark theme.</p>
//     <button class="button">Click Me</button>
//   </div>
// </div>
```

这个示例展示了如何使用Sass创建一个可切换的主题系统，通过简单地更改父元素的类名就可以应用不同的主题。
