# 晴潮浅海 · Azure Tide

这是一套独立的 Hugo / PaperMod **视觉渲染组**，与博客内容分离，适用于轻快的浅蓝海洋风格。

## 效果设计

- 主色：奶白、海盐蓝、浅天青、薄荷青与少量暖阳黄
- 首页：自绘 SVG 海面与太阳，分层海浪轻微浮动
- 设计组件：英雄区、胶囊导航按钮、三个不同色彩的写作方向卡片
- 文章列表：卡片悬浮、阅读排版、聚焦态
- 空文章页：温柔的「故事，正在酝酿」提示，避免空白页面
- 无远程图片、字体和第三方 JS 依赖；支持移动端及减少动画偏好

## 目录隔离

```text
assets/css/render-groups/
├── azure-tide/
│   ├── theme.css            # 所有颜色、动效与组件样式
│   └── README.md
└── melancholy-ocean/        # 保留的深蓝色旧风格

layouts/_partials/render-groups/azure-tide/home.html
                             # 晴潮浅海首页结构与 SVG
layouts/_partials/home_info.html
                             # 风格分派入口
layouts/_partials/extend_head.html
                             # 加载当前渲染组 CSS
layouts/posts/list.html      # 文章列表 / 空状态
content/posts/_index.md     # Posts 栏目入口；真正文章由 Publisher 创建
```

更改整体风格，只需要修改 `hugo.toml`：

```toml
[params]
  renderGroup = "azure-tide"
  defaultTheme = "light"
```

要切回旧深海版，将 `renderGroup` 改为 `melancholy-ocean`。注意首页和文章空状态属于 Azure Tide 扩展布局，若完全切换设计语言，还可以对应调整布局。

## 常用定制

- **主色/深浅模式**：`theme.css` 开头的 PaperMod 变量和 `--tide-*`
- **首页文本、按钮、海景 SVG**：`layouts/_partials/render-groups/azure-tide/home.html`
- **波浪与浮动速度**：`theme.css` 中的 `tide-wave-slow`、`tide-float`
- **文章空状态**：`layouts/posts/list.html`
- **站点名称、导航**：`hugo.toml`

**不要在渲染组里存储文章。** 请继续通过思源笔记 Publisher 将文章发布到 `main` 分支的 `content/posts/` 目录。保留 `content/posts/_index.md`，它是栏目页，不是博客文章。

## 关于自动部署

GitHub Actions 每次运行前执行 `rm -rf public`，再执行 `hugo --minify`，保证旧网页不会因为构建缓存而残留。仓库的 `.gitignore` 已忽略 `public/`。

旧文章已从当前分支移除；如果你以后想找回，可以通过 Git 历史恢复。思源笔记里的原始文档不受影响。

## 首页人像海景背景（2026-11）

首页现在采用用户提供的海边蓝调照片，已在上传前做了轻度柔焦与 WebP 压缩：

- `static/images/site/hero-bg.webp`：首页背景图（轻微柔焦，约 8.6 KB）
- `theme.css` 的最后一节 **Sea Portrait Edition / 海边暮蓝肖像**：专门管理照片背景、蓝色遮罩、文字配色与移动端裁切
- 桌面端照片集中在首页右侧，左边使用深蓝半透明渐变保证可读性；手机端图片上方展示、文字在下方
- 首页旧 SVG 海浪组件仍保留在 Hugo 布局文件中，但当前被 CSS 隐藏；未来切换回纯插画风格时可恢复

照片只用于首页，不覆盖文章阅读区域。调整虚化程度，修改 `theme.css` 中的 `filter: blur(2px)`；调整人物位置，修改 `background-position`。如果将来替换照片，请替换相同路径文件，不需要修改思源笔记文章。
