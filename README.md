# NoviDesk 官方文档工程

本目录是一个**标准 GitHub Pages（Jekyll）文档工程**，用于发布 NoviDesk 桌面工作站的官方文档。
结构参考同组织的 `NoviDesk-Privacy` 仓库（独立仓库、`main` 分支、仓库根目录即 Pages 源）。

## 站点结构（三页 + 侧边栏导航）

```
Docs/Docs/
├── _config.yml              # Jekyll / GitHub Pages 配置（permalink: pretty）
├── _layouts/
│   └── default.html         # 页面骨架：顶部 logo 栏 + 侧边栏 + 内容 + 页脚
├── _includes/
│   └── sidebar.html         # 左侧文档导航（产品主页 / 用户指南 / 订购）
├── assets/
│   ├── css/
│   │   └── style.css         # 顶部 logo + 侧边栏 + 内容 + 响应式 + 暗色
│   └── img/
│       ├── 0X-*.svg          # 各节插图（矢量，可直接替换为真实截图 .png/.jpg）
│       ├── tip*.gif
│       └── favicon.svg       # 品牌 3×3 标记
├── index.md                 # 【主页】产品概览（/）
├── guide.md                 # 【用户操作指南】（/guide/）
├── buy.md                   # 【订购与购买】（/buy/）
└── README.md               # 本说明
```

## 访问 URL

| 页面 | URL |
|---|---|
| 主页 | `https://tech-littlesoft.github.io/NoviDesk-Docs/` |
| 用户操作指南 | `https://tech-littlesoft.github.io/NoviDesk-Docs/guide/` |
| 订购与购买 | `https://tech-littlesoft.github.io/NoviDesk-Docs/buy/` |

每个页面都可以独立访问；左侧导航自动高亮当前页。

## 如何编辑（便于修改）

- **改文字**：分别编辑 `index.md` / `guide.md` / `buy.md`，按 Markdown 书写；中英文分别用 `.zh` / `.en` 样式类包裹。
- **换插图**：把 `assets/img/` 下对应 `.svg` 换成真实截图（同名 `.png`/`.jpg` 亦可）。
- **改导航**：编辑 `_includes/sidebar.html`。
- **调样式**：改 `assets/css/style.css`。
- **换 logo**：在 `_layouts/default.html` 中替换内联 `<svg class="brand-mark">`；或放一个 SVG 到 `assets/img/wordmark.svg` 后用 `<img>` 引用。

### 关于 `<base href>` 与相对路径

`_layouts/default.html` 中 `<base href="{{ site.baseurl }}/">` 让所有页面的相对资源路径（如 `assets/img/01-float.svg`）自动指向仓库根目录。因此三个页面里的图片、CSS 引用都不需要写完整 URL，加新页面也无需额外处理。

## 关于 permalink 命名

- 每页的 URL 后缀由 front matter 中的 `permalink:` 决定，例如 `permalink: /guide/` 让 `guide.md` 输出为 `/guide/`。
- 注意：GitHub Pages 的**第一段路径是仓库名**，无法用 permalink 改写。要让 `buy` 成为顶级 URL `https://tech-littlesoft.github.io/NoviDesk-buy/`，需要新建独立仓库 `NoviDesk-buy`（与 `NoviDesk-Privacy` 同组织）。
- 当前选择**单仓库三页**（NoviDesk-Docs），便于共用侧边栏与样式；如以后要独立 `buy` 落地页，把 `buy.md` + `_layouts/_includes/assets` 拷到 `NoviDesk-buy` 仓库即可，URL 自动变成顶级 `/NoviDesk-buy/`。

## 如何发布到 GitHub Pages

```bash
# 1) 在本目录初始化仓库（若尚未初始化）
git init -b main
git add .
git commit -m "Add NoviDesk docs site (3 pages with sidebar)"

# 2) 关联远程仓库（建议命名 NoviDesk-Docs，与 NoviDesk-Privacy 同组织）
git remote add origin https://github.com/tech-littlesoft/NoviDesk-Docs.git

# 3) 推送 main 分支，GitHub Pages 自动构建发布
git push -u origin main
```

> 仓库设置 → Pages → Source 选择 **main 分支 / root** 即可。
> 无需本地安装 Jekyll：GitHub 会在服务端自动用 `_config.yml` 构建。

## 本地预览（可选）

如需本地预览，安装 Ruby + Jekyll 后在本目录执行：

```bash
bundle init        # 生成 Gemfile 后加入 gem "github-pages"
bundle exec jekyll serve
```