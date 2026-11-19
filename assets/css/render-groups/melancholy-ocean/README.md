# 渲染组：忧郁海蓝（melancholy-ocean）

这套视觉风格用于 Hugo + PaperMod 博客。**渲染与内容隔离**：本目录只定义外观，不存储任何文章，也不要求修改思源 Publisher 的发布路径。

## 风格

- 深夜海水蓝、墨蓝、轻微海雾和冷银色文字；不是晴朗蓝天
- 首页表现遥远海平线和缓慢变化的水纹（纯 CSS，不依赖图片或第三方服务）
- 半透明导航、文章卡片悬浮、阅读页排版优化
- 支持手机端、明暗切换与系统「减少动态效果」偏好

## 文件关系

```text
assets/css/render-groups/melancholy-ocean/
├── theme.css       ← 风格主体：颜色、玻璃质感、水纹及动效
└── README.md       ← 本说明

layouts/_partials/extend_head.html  ← 通用渲染组加载器
hugo.toml                           ← 通过 params.renderGroup 选择加载哪一组
content/posts/                       ← 文章内容，不受本组管理
```

PaperMod 会调用 `layouts/_partials/extend_head.html`；该文件读取 `hugo.toml` 中的 `renderGroup` 参数并加载相应主题样式。你可以在 `assets/css/render-groups/` 下增建其他风格目录，每个目录放一个 `theme.css`，然后仅切换：

```toml
[params]
  renderGroup = "melancholy-ocean"
```

> 注意：`renderGroup` 需填已有目录名。不存在时不会加载任何自定义 CSS，网站仍采用 PaperMod 原始样式。

## 修改提示

- **蓝色深浅**：编辑 `theme.css` 开头的 `--ocean-top`、`--ocean-middle`、`--ocean-bottom`
- **雾气强弱**：编辑 `--ocean-haze`
- **卡片透明度**：编辑 `--ocean-glass`
- **海面纹理速度**：编辑 `ocean-swell`（首页）和 `ocean-drift`（背景）的动画时长
- **首页文字**：位于 `hugo.toml` 的 `[params.homeInfoParams]`，不在文章目录里

保持 `content/posts/` 供思源 Publisher 写入，别把渲染组配置放到文章 Front Matter 中。变更样式后，push 到 `main` 即会触发原有的 GitHub Pages 构建。

## 性能与可访问性

全部用 CSS 绘制，无新字体、无远程图片、无运行时 JavaScript；`prefers-reduced-motion: reduce` 时停用相关动画。页面阅读区与导航层级保持独立，不覆盖文字交互。
