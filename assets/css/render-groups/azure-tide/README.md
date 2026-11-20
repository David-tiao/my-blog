# 晴潮浅海 · Azure Tide

这是一套独立的 Hugo / PaperMod **视觉渲染组**，与博客内容分离，适用于轻快的浅蓝海洋风格。

## 效果设计

- 主色：深海蓝、暮蓝、浅天青；正文阅读区仍保持浅色高对比度
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

## 全站海边人像背景（2026-11-20）

当前样式由两层组成：

- `theme.css`：基础排版、按钮、卡片、动效及移动端基础样式。只保留一份基本 Hero 结构规则，不再包含过时的人物照片 Hero 覆盖层。
- `fullsite-background.css`：唯一负责深蓝海洋背景、人物照片、首页布局及文章阅读面板的扩展层。它由 `extend_head.html` 在 `theme.css` 后加载。

### 视觉布局

- 桌面端：人物照片仍使用 `static/images/site/hero-fixed.webp`，保持 `contain` 比例与原始清晰度；只用 `mask-image` 渐隐边缘，不使用模糊、拉伸或硬裁切。
- 首页介绍：透明 Hero 自然落在全站深蓝背景之上，没有独立的矩形深色面板。
- 「写作的三种波长」：宽屏为三列卡片，较窄的平板为两列，手机端为一列；不再被限制到左侧 440px。
- 手机端：首页先展示人像，再展示文字和分类。文章列表、正文页不会因首页照片预留空间而被推到下方。
- 文章正文使用浅色高对比度阅读面板；背景属于视觉装饰，不侵入正文内容。

### 调整入口

- 改人物图：替换 `static/images/site/hero-fixed.webp`，保持引用路径不变。
- 改人物位置或画幅：调整 `fullsite-background.css` 的 `body::before`。
- 改照片与海景的融合范围：调整 `body::before` 的 `mask-image` 径向渐变。
- 改海洋底色：调整 `body, body.list` 的渐变背景。
- 改首页内容宽度、卡片列数：调整 `.tide-hero`、`.tide-topics` 和相关媒体查询。

不要在渲染组里存储文章：思源笔记 Publisher 仍将文章写入 `content/posts/`，这次仅修改视觉样式和文档，不修改发布流程。
