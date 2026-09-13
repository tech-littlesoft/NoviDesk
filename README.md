# NoviDesk 官方文档工程

本目录是一个**标准 GitHub Pages（Jekyll）文档工程**，用于发布 NoviDesk 桌面工作站的官方文档。
结构参考同组织的 `NoviDesk-Privacy` 仓库（独立仓库、`main` 分支、仓库根目录即 Pages 源）。

## 站点结构（4 页 + 侧边栏导航）

```
Docs/Docs/
├── _config.yml              # Jekyll / GitHub Pages 配置（permalink: pretty）
├── _layouts/
│   └── default.html         # 页面骨架：顶部 logo 栏 + 侧边栏 + 内容 + 右侧目录 + 页脚
├── _includes/
│   └── sidebar.html         # 左侧文档导航（主页 / 指南 / 升级 Pro / 隐私）
├── assets/
│   ├── css/
│   │   └── style.css         # 顶部 logo + 侧边栏 + 目录 + 正文 + 响应式 + 暗色 + 打印
│   └── img/
│       ├── 0X-*.svg          # 各节插图（矢量，可直接替换为真实截图 .png/.jpg）
│       ├── tip*.gif
│       └── favicon.svg       # 品牌 3×3 标记
├── index.md                 # 1) 产品主页           （/）
├── guide.md                 # 2) 用户操作指南       （/guide/）
├── buy.md                   # 3) 升级 Pro           （/buy/）
├── privacy.md               # 4) 隐私说明           （/privacy/）
└── README.md               # 本说明
```

## 访问 URL

| 页面 | URL |
|---|---|
| 产品主页 | `https://tech-littlesoft.github.io/NoviDesk/` |
| 用户操作指南 | `https://tech-littlesoft.github.io/NoviDesk/guide/` |
| 升级 Pro | `https://tech-littlesoft.github.io/NoviDesk/buy/` |
| 隐私说明 | `https://tech-littlesoft.github.io/NoviDesk/privacy/` |

每页都可独立访问；左侧导航自动高亮当前页，右侧目录自动生成并跟随滚动高亮。

## 如何编辑（便于修改）

- **改文字**：分别编辑 `index.md` / `guide.md` / `buy.md` / `privacy.md`，按 Markdown 书写；中英文分别用 `.zh` / `.en` 样式类包裹。
- **换插图**：把 `assets/img/` 下对应 `.svg` 换成真实截图（同名 `.png`/`.jpg` 亦可）。
- **改导航**：编辑 `_includes/sidebar.html`。
- **开关右侧目录**：在页面 front matter 加 `toc: true` 即出现「本页目录」，不加则不显示。
- **调样式**：改 `assets/css/style.css`。
- **换 logo**：在 `_layouts/default.html` 中替换内联 `<svg class="brand-mark">`；或放一个 SVG 到 `assets/img/wordmark.svg` 后用 `<img>` 引用。

## 两条必须遵守的路径约定

1. **图片等静态资源用相对路径**，如 `assets/img/01-float.svg`。
   `_layouts/default.html` 里的 `<base href="{{ site.baseurl }}/">` 会让它自动解析到仓库根，任何页面、任何子目录都不需要改。
2. **站内链接必须用 Liquid 过滤器**：`<a href="{{ '/buy/' | relative_url }}">`。
   ⚠️ 不要写 `href="/buy/"` —— 因为 `<base>` 的存在，以 `/` 开头会被解析到**域名根**（`.../buy/` → 404）。

## 关于 permalink 命名

- 每页的 URL 后缀由 front matter 中的 `permalink:` 决定，例如 `permalink: /privacy/` 让 `privacy.md` 输出为 `/privacy/`。
- 注意：GitHub Pages 的**第一段路径是仓库名**，无法用 permalink 改写。要让 `buy` 成为顶级 URL `https://tech-littlesoft.github.io/NoviDesk-buy/`，需要新建独立仓库 `NoviDesk-buy`。
- 当前架构为**单仓库 4 页**（NoviDesk），便于共用侧边栏与样式。

## 绑定自有域名后要做的事

1. 仓库 Settings → Pages → Custom domain 填域名（如 `novidesk.com`），勾选 Enforce HTTPS
2. 仓库根目录新增 `CNAME` 文件，内容为该域名
3. `_config.yml` 改为：
   ```yaml
   url: https://novidesk.com
   baseurl: ""
   ```
   由于布局用的是 `relative_url` 与 `<base href>`，改这两行后全站链接、CSS、图片自动跟随，其余文件不用动。

## 如何发布到 GitHub Pages

```bash
# 1) 在本目录初始化仓库（若尚未初始化）
git init -b main
git add .
git commit -m "Add NoviDesk docs site (4 pages with sidebar)"

# 2) 关联远程仓库（github.com-techlittlesoft 为 ~/.ssh/config 中的主机别名）
git remote add origin git@github.com-techlittlesoft:tech-littlesoft/NoviDesk.git

# 3) 推送 main 分支，GitHub Pages 自动构建发布
git push -u origin main
```

> 仓库设置 → Pages → Source 选择 **main 分支 / root** 即可。
> 无需本地安装 Jekyll：GitHub 会在服务端自动用 `_config.yml` 构建。

## 本地预览（可选）

GitHub Pages 是服务端构建，**本地不需要编译**。若要本地实时预览，安装 Ruby + Jekyll 后在本目录执行：

```bash
bundle init        # 生成 Gemfile 后加入 gem "github-pages", group: :jekyll_plugins
bundle install
bundle exec jekyll serve --livereload
```

然后访问 **http://127.0.0.1:4000/NoviDesk/**（`/NoviDesk/` 前缀来自 `baseurl`，不能省）。
