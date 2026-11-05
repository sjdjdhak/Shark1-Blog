---
title: "Hugo + Blowfish 个人博客搭建指南（零基础版）"
date: 2026-11-02
draft: false
summary: "从零开始，用 Hugo + Blowfish 搭建个人博客并免费部署上线，每一条命令、每一个配置都有解释。"
tags: ["Hugo", "Blowfish", "博客", "教程"]
categories: ["Tutorials"]
series: ["Hugo 博客从零到一"]
series_order: 1
---

# Hugo + Blowfish 个人博客搭建指南

> 方案：Hugo（通过 npm 安装）+ Blowfish 主题（git submodule）+ GitHub + Cloudflare Pages
> 环境：Windows / PowerShell，需已安装 Node.js 和 Git
> 整理日期：2026-11-02

---

## 目录

1. [版本兼容性](#1-版本兼容性)
2. [创建项目并安装 Hugo](#2-创建项目并安装-hugo)
3. [生成站点骨架](#3-生成站点骨架)
4. [安装 Blowfish 主题](#4-安装-blowfish-主题)
5. [复制并修改配置](#5-复制并修改配置)
6. [npm 脚本与 .gitignore](#6-npm-脚本与-gitignore)
7. [写文章](#7-写文章)
8. [系列文章](#8-系列文章)
9. [本地预览](#9-本地预览)
10. [推送到 GitHub](#10-推送到-github)
11. [部署到 Cloudflare Pages](#11-部署到-cloudflare-pages)
12. [日常命令速查](#12-日常命令速查)
13. [升级 Hugo 与主题](#13-升级-hugo-与主题)
14. [常见问题](#14-常见问题)
15. [参考链接](#15-参考链接)

---

## 1. 版本兼容性

Blowfish 对 Hugo 版本有要求，写在主题仓库的 `config.toml` 里：

```toml
[module.hugoVersion]
extended = true      # 必须是 extended 版
min = "0.164.0"
max = "0.167.0"
```

- 记录时 Blowfish 最新为 v3.9.0，支持 Hugo **0.164.0 ~ 0.167.0**
- 本项目锁定 **Hugo extended 0.167.0**
- Hugo 最新版超出 `max` 时会报不兼容警告，可能出现页面异常，**不要盲目升级 Hugo**
- 查看当前要求：<https://github.com/nunocoracao/blowfish/blob/main/config.toml>

---

## 2. 创建项目并安装 Hugo

```bash
node -v
git --version
```

```bash
mkdir blog
cd blog
git init
npm init -y
```

安装指定版本的 Hugo，`--save-exact` 锁死版本号：

```bash
npm install hugo-extended@0.167.0 --save-dev --save-exact
```

### 如果出现 allow-scripts 警告

```
npm warn allow-scripts   hugo-extended@0.167.0 (postinstall: node postinstall.js)
```

**含义**：新版 npm 默认拦截包的安装脚本。`hugo-extended` 依靠 `postinstall` 脚本从 Hugo 官方 GitHub 下载 `hugo.exe`，被拦截就等于 Hugo 没装上。

**解决**：

```bash
npm approve-scripts hugo-extended
npm rebuild hugo-extended
```

授权记录会写进 `package.json`，记得提交到 Git。

### 验证

```bash
npx hugo version
```

输出中应包含 `v0.167.0` 和 `+extended`。

> 为什么不用 Git 装 Hugo：Git 只能拉源码，还要自己装 Go 和 GCC 编译。npm 和 winget 都是直接下载官方编译好的程序。
> 用 npm 装在项目里的好处：Hugo 版本记录在 `package.json` 中，换电脑或者在 Cloudflare 构建时都会用同一个版本。

---

## 3. 生成站点骨架

```bash
npx hugo new site . --force --format toml
```

| 部分 | 含义 |
|---|---|
| `npx` | 运行项目 `node_modules` 里的 Hugo |
| `hugo new site` | 创建新站点 |
| `.` | 建在当前目录 |
| `--force` | 目录非空也继续（命令行参数，不用写进任何文件） |
| `--format toml` | 配置文件用 TOML 格式 |

**为什么要 `--force`**：Hugo 只在空文件夹里建站，而此时目录里已经有 `package.json`、`node_modules`。`--force` 只是跳过这个检查，不会删除或覆盖已有文件。

生成的结构：

```
blog/
├── archetypes/      # 新建文章的模板
├── assets/          # 需处理的资源（头像、CSS）
├── content/         # 文章
├── data/
├── i18n/
├── layouts/         # 自定义模板，覆盖主题
├── static/          # 原样复制的静态文件（favicon 等）
├── themes/          # 主题
├── hugo.toml        # 下一步会删除
├── package.json
└── node_modules/
```

---

## 4. 安装 Blowfish 主题

```bash
git submodule add -b main https://github.com/nunocoracao/blowfish.git themes/blowfish
```

---

## 5. 复制并修改配置

Blowfish 使用 `config/_default/` 下的多个配置文件，替代根目录的 `hugo.toml`。

```bash
rm hugo.toml
mkdir config/_default
cp themes/blowfish/config/_default/* config/_default/
mv config/_default/languages.en.toml config/_default/languages.zh-cn.toml
mv config/_default/menus.en.toml config/_default/menus.zh-cn.toml
```

> **重要**：Hugo 通过**文件名**识别语言。`languages.zh-cn.toml` 对应语言 `zh-cn`，必须和 `defaultContentLanguage` 一致。

### 5.1 `config/_default/hugo.toml`

```toml
theme = "blowfish"                         # 取消注释
baseURL = "https://your-blog.pages.dev/"   # 部署后改成真实地址
defaultContentLanguage = "zh-cn"
hasCJKLanguage = true                      # 中文字数和阅读时间统计更准确

[outputs]
  home = ["HTML", "RSS", "JSON"]           # JSON 用于站内搜索，不能删

[taxonomies]
  tag = "tags"
  category = "categories"
  author = "authors"
  series = "series"                        # 系列文章
```

### 5.2 `config/_default/languages.zh-cn.toml`

```toml
languageCode = "zh-CN"
languageName = "简体中文"
weight = 1
title = "我的博客"

[params]
  displayName = "中文"
  isoCode = "zh-CN"
  rtl = false
  dateFormat = "2006年1月2日"
  description = "博客简介"

[params.author]
  name = "你的名字"
  image = "img/avatar.jpg"       # 文件放在 assets/img/avatar.jpg
  headline = "一句话头衔"
  bio = "个人简介"
  links = [
    { github = "https://github.com/你的用户名" },
  ]
```

### 5.3 `config/_default/menus.zh-cn.toml`

```toml
[[main]]
  name = "文章"
  pageRef = "posts"
  weight = 10

[[main]]
  name = "系列"
  pageRef = "series"
  weight = 15

[[main]]
  name = "标签"
  pageRef = "tags"
  weight = 20

[[main]]
  name = "关于"
  pageRef = "about"
  weight = 30
```

### 5.4 `config/_default/params.toml`（只改关键项）

```toml
colorScheme = "blowfish"         # blowfish / avocado / ocean / fire / forest / slate / congo 等
defaultAppearance = "light"
autoSwitchAppearance = true      # 跟随系统明暗
enableSearch = true
enableCodeCopy = true

[homepage]
  layout = "profile"             # page / profile / hero / card / background
  showRecent = true
  showRecentItems = 6
  cardView = true

[article]
  showTableOfContents = true
  showReadingTime = true
  showHero = true
  heroStyle = "basic"            # basic / big / background / thumbAndBackground
  seriesOpened = false           # 系列目录默认是否展开

[list]
  cardView = true
```

> 参数名以当前主题版本为准，对不上时查阅 <https://blowfish.page/docs/>。

---

## 6. npm 脚本与 .gitignore

`package.json` 中的 `scripts`：

```json
"scripts": {
  "dev": "hugo server -D",
  "build": "hugo --gc --minify",
  "new": "hugo new content"
}
```

`.gitignore`：

```
node_modules/
public/
resources/_gen/
.hugo_build.lock
```

---

## 7. 写文章

### 7.1 新建文章

```bash
npm run new -- posts/hello-world/index.md
```

```markdown
---
title: "你好，世界"
date: 2026-11-02
draft: false
summary: "我的第一篇博客"
tags: ["随笔"]
categories: ["生活"]
---

正文从这里开始。
```

- `draft: true` 的文章只在本地 `npm run dev`（带 `-D`）时显示，线上不会出现
- 页面标题来自 `title`；**文件夹名决定网址**：`posts/hello-world/` → `/posts/hello-world/`

### 7.2 两种文章写法

**单文件**（适合纯文字）：

```
content/posts/hello-world.md     →  /posts/hello-world/
```

**文件夹 / page bundle**（推荐，适合有图片的文章）：

```
content/posts/hello-world/
├── index.md          ← 正文
├── feature.jpg       ← 封面图，Blowfish 自动识别（文件名以 feature 开头）
└── screenshot.png    ← 正文插图，用 ![截图](screenshot.png) 引用
```

套一层文件夹的好处：封面自动生效、插图路径简单、一篇文章的所有文件放在一起、图片可以被 Hugo 压缩和生成多种尺寸。

### 7.3 多层嵌套

```
content/posts/2026/hello-world/index.md   →  /posts/2026/hello-world/
```

- 中间目录**没有** `_index.md`：只是整理文件用，文章仍属于 `posts`
- 中间目录**有** `_index.md`：成为独立子栏目，`/posts/2026/` 有专属列表页
- **限制**：带 `index.md` 的文章文件夹里不能再放文章，里面的东西都会被当作附件

分类建议用 `categories` / `tags`，不要靠文件夹层级。想让网址带年份，可以在 `hugo.toml` 中配置：

```toml
[permalinks]
  posts = "/posts/:year/:slug/"
```

### 7.4 「关于」页面

```bash
npm run new -- about/index.md
```

---

## 8. 系列文章

确认 `hugo.toml` 的 `[taxonomies]` 里有 `series = "series"`，然后在每篇文章头部标注：

```yaml
---
title: "Hugo 建站（一）：安装环境"
date: 2026-11-02
series: ["Hugo 建站指南"]
series_order: 1
---
```

```yaml
---
title: "Hugo 建站（二）：配置 Blowfish"
date: 2026-11-05
series: ["Hugo 建站指南"]
series_order: 2
---
```

- `series`：系列名，同一系列的每篇文章必须**完全一致**
- `series_order`：在系列中的顺序
- 效果：文章顶部出现系列目录；自动生成 `/series/` 汇总页

文件夹组织（可选，两种都能配合 `series` 使用）：

```
# 平铺
content/posts/hugo-setup-1/index.md
content/posts/hugo-setup-2/index.md

# 单独文件夹
content/posts/hugo-guide/
├── _index.md               ← 可选：系列介绍页
├── 01-install/index.md     ← 可加 slug: "install" 去掉网址中的 01-
├── 02-config/index.md
└── 03-deploy/index.md
```

---

## 9. 本地预览

```bash
npm run dev
```

打开 <http://localhost:1313>，修改后自动刷新，按 `Ctrl+C` 停止。

---

## 10. 推送到 GitHub

先在 GitHub 新建一个空仓库（不勾选 README），然后执行：

```bash
git add .
git commit -m "init blog"
git branch -M main
git remote add origin https://github.com/你的用户名/blog.git
git push -u origin main
```

---

## 11. 部署到 Cloudflare Pages

1. 打开 <https://dash.cloudflare.com>，依次点 **Workers & Pages**、**Create**、**Pages**、**Connect to Git**，选择仓库
2. 构建设置：
   - Framework preset：**None**
   - Build command：`npm run build`
   - Build output directory：`public`
3. 部署完成后，把 `xxx.pages.dev` 填回 `hugo.toml` 的 `baseURL`，提交并推送

> Cloudflare 检测到 `package.json` 会自动执行 `npm install`，用的就是锁定的 Hugo 版本，不需要再设置 `HUGO_VERSION`。主题 submodule 也会自动拉取。

### 可选：Giscus 评论

1. 在 GitHub 仓库的 Settings 中开启 **Discussions**
2. 在 <https://giscus.app/zh-CN> 生成 `<script>` 代码
3. 新建 `layouts/partials/comments.html`，粘贴这段代码
4. 在 `params.toml` 的 `[article]` 下设置 `showComments = true`

---

## 12. 日常命令速查

| 操作 | 命令 |
|---|---|
| 新建文章 | `npm run new -- posts/文章名/index.md` |
| 本地预览 | `npm run dev` |
| 本地构建 | `npm run build` |
| 发布 | `git add .` → `git commit -m "xxx"` → `git push` |
| 查看 Hugo 版本 | `npx hugo version` |
| 换电脑恢复环境 | `git clone --recurse-submodules 仓库地址` → `npm install` |

---

## 13. 升级 Hugo 与主题

**查看最新版本：**

```bash
npm view hugo-extended version
```

或者查看 <https://github.com/gohugoio/hugo/releases>。

**升级流程：**

1. 查看 Blowfish 最新 `config.toml` 中的 `max`，确认支持的 Hugo 版本
2. 阅读 Blowfish 的 Release 说明，留意不兼容的改动
3. 升级主题：

   ```bash
   git submodule update --remote --merge
   ```

4. 升级 Hugo 到支持范围内的版本：

   ```bash
   npm install hugo-extended@版本号 --save-dev --save-exact
   ```

5. 执行 `npm run dev` 检查页面是否正常，再提交

---

## 14. 常见问题

### `already exists and is not empty. See --force.`

目录非空，Hugo 拒绝建站。在命令末尾加上 `--force` 即可。如果加了还是报错，可能站点已经建过一次了（目录里已有 `hugo.toml`、`content/`），跳过这一步继续即可。

### `defaultContentLanguage "zh-cn" not found in languages configuration`

Hugo 找不到 `zh-cn` 语言，几乎都是文件名的问题。用 `ls config/_default` 检查：

- 是否还是 `languages.en.toml`，没有改名
- 是否写错，比如 `zh_cn`、`language.`（少了 s）、`zh`
- 是否实际是 `languages.zh-cn.toml.txt`（Windows 隐藏了扩展名）
- 是否放错目录，必须放在 `config/_default/` 下

### `npm warn allow-scripts ... hugo-extended`

见 [第 2 节](#如果出现-allow-scripts-警告)：执行 `npm approve-scripts hugo-extended`，再执行 `npm rebuild hugo-extended`。

### 文章本地能看到，线上看不到

检查文章头部的 `draft` 是否为 `false`，以及 `date` 是否晚于当前时间（未来日期的文章默认不发布）。

### 菜单不显示

检查 `menus.zh-cn.toml` 的文件名是否和语言名一致。

---

## 15. 参考链接

- Hugo 官网：<https://gohugo.io>
- Hugo 发布页：<https://github.com/gohugoio/hugo/releases>
- Blowfish 文档：<https://blowfish.page/docs/>
- Blowfish 仓库：<https://github.com/nunocoracao/blowfish>
- Giscus：<https://giscus.app/zh-CN>
- Cloudflare Pages：<https://dash.cloudflare.com>
