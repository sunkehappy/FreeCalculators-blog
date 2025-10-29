# 图片资源目录

本目录用于存放博客中使用的所有图片资源。

## 目录结构

```
assets/images/
├── posts/          # 文章配图
├── heroes/         # 首页和页面头图
├── icons/          # 图标文件
├── logos/          # Logo和品牌图片
└── README.md       # 本说明文件
```

## 使用规范

### 1. 文章配图 (`posts/`)
- 存放与具体文章相关的图片
- 命名格式：`YYYY-MM-DD-文章标题-描述.jpg`
- 例如：`2025-01-25-typescript-strict-mode-practices-code-example.png`

### 2. 头图 (`heroes/`)
- 存放首页轮播图、文章头图等
- 建议尺寸：1200x600px 或 1920x1080px
- 命名格式：`hero-描述.jpg`

### 3. 图标 (`icons/`)
- 存放各种小图标
- 支持格式：SVG, PNG, ICO
- 建议使用SVG格式以获得最佳效果

### 4. Logo (`logos/`)
- 存放品牌Logo和标识
- 建议提供多种尺寸和格式

## 在文章中使用图片

### 方法1：使用Jekyll的asset_url过滤器
```markdown
![图片描述]({{ '/assets/images/posts/example.jpg' | relative_url }})
```

### 方法2：使用HTML标签（推荐用于复杂布局）
```html
<img src="{{ '/assets/images/posts/example.jpg' | relative_url }}" 
     alt="图片描述" 
     class="post-image">
```

### 方法3：在CSS中引用
```scss
.hero-image {
  background-image: url('{{ "/assets/images/heroes/hero-bg.jpg" | relative_url }}');
}
```

## 图片优化建议

1. **格式选择**：
   - 照片：使用JPEG格式
   - 图标、Logo：使用PNG或SVG格式
   - 动画：使用GIF或WebP格式

2. **尺寸优化**：
   - 文章配图：宽度不超过800px
   - 头图：1200x600px或1920x1080px
   - 图标：根据使用场景选择合适的尺寸

3. **文件大小**：
   - 单张图片建议不超过500KB
   - 使用图片压缩工具优化文件大小

4. **命名规范**：
   - 使用小写字母和连字符
   - 避免空格和特殊字符
   - 文件名要有意义，便于管理
