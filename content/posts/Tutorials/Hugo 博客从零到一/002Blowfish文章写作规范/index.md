---
title: "Blowfish 主题文章写作规范（零基础版）"
date: 2026-11-02
draft: false
slug: "blowfish-writing-guide"
summary: "一篇 Blowfish 文章该怎么写：文章信息、自动模板、Markdown 正文规范、扩展组件和发布前检查，附可直接复制的模板。"
tags: ["Hugo", "Blowfish", "Markdown", "教程"]
categories: ["Tutorials"]
showTableOfContents: true
series: ["Hugo 博客从零到一"]
series_order: 2
---

> **适合谁看**：已经用 Hugo + Blowfish 搭好博客，准备开始写文章的人。
> **你会得到**：一份可以直接复制的文章模板、一个让新文章自动套用模板的设置，以及写正文时需要注意的规范。
> **整理日期**：2026-11-02（Blowfish v3.9.0，Hugo 0.167.0）

---

## 一、一篇文章由哪几部分组成

一篇 Blowfish 文章就是一个 Markdown 文件（`.md`），分为两部分：

```
---
这里是 front matter（文章信息）
标题、日期、标签、是否草稿……
---

这里是正文
用 Markdown 写，可以加入 Blowfish 的扩展组件
```

- **Front matter**：夹在两行 `---` 之间，写的是「关于这篇文章的信息」，不会直接显示成正文。
- **正文**：第二个 `---` 之后的全部内容，就是读者看到的文章。

---

## 二、文章放在哪里

推荐每篇文章一个文件夹（Hugo 里叫 page bundle，页面包）：

```
content/posts/my-first-post/
├── index.md       ← 文章本身，文件名固定叫 index.md
├── feature.jpg    ← 封面图，文件名以 feature 开头
└── img-1.png      ← 正文里用到的图片
```

| 文件 | 作用 |
|---|---|
| 文件夹名 `my-first-post` | **决定文章网址**：`https://你的域名/posts/my-first-post/`。建议用英文或拼音，单词之间用 `-` 连接。用中文的话网址里会变成一串乱码 |
| `index.md` | 文章正文。注意没有下划线（`_index.md` 是栏目首页，不是文章） |
| `feature.jpg` | 封面图。Blowfish 会自动识别，显示在文章列表卡片和文章顶部。`.png`、`.webp` 格式也可以 |
| 其他图片 | 正文插图，和 `index.md` 放在一起，引用时直接写文件名 |

新建文章的命令：

```bash
npm run new -- posts/my-first-post/index.md
```

> **作用**：在 `content/posts/my-first-post/` 下新建 `index.md`。如果按第四节设置了模板，生成的文件会自动带上完整的 front matter。

---

## 三、文章标准模板

直接复制下面的模板，**用不到的字段整行删掉即可**：

```markdown
---
title: "文章标题"
date: 2026-11-02
lastmod: 2026-11-02
draft: false
slug: "article-url"
summary: "一两句话的摘要，显示在文章列表卡片上"
description: "给搜索引擎看的描述，不填就用 summary"
tags: ["标签1", "标签2"]
categories: ["分类"]
series: ["系列名"]
series_order: 1
keywords: ["关键词1", "关键词2"]

showTableOfContents: true
showReadingTime: true
showWordCount: true
showComments: true
showHero: true
heroStyle: "basic"
---

开头一段引言，概括这篇文章讲什么。

## 第一部分

正文……

## 第二部分

正文……
```

### Front matter 的书写规则

这里用的是 YAML 格式，规则很简单：

| 规则 | 正确 | 错误 |
|---|---|---|
| 冒号后面必须有一个空格 | `title: "标题"` | `title:"标题"` |
| 文字用英文双引号括起来 | `title: "标题"` | `title: “标题”`（中文引号） |
| 多个值用方括号，英文逗号分隔 | `tags: ["a", "b"]` | `tags: ["a"，"b"]`（中文逗号） |
| 开关用 true / false，不加引号 | `draft: false` | `draft: "false"` |
| 日期格式是 年-月-日 | `date: 2026-11-02` | `date: 2026/11/2` |
| 开头和结尾各有一行 `---` | — | 缺了任意一行，整个 front matter 都会失效 |

> 💡 中文输入法下很容易打出中文的引号和逗号，这是最常见的错误。写 front matter 时切换到英文输入。

### 基础字段（每篇都要写）

| 字段 | 作用 | 说明 |
|---|---|---|
| `title` | 文章标题 | 显示在页面顶部、文章列表、浏览器标签页。可以用中文 |
| `date` | 发布日期 | 文章按这个日期排序。**晚于今天的文章不会发布** |
| `draft` | 是否为草稿 | `true`：草稿，只在本地预览（`npm run dev`）时能看到<br>`false`：正式发布 |
| `summary` | 文章摘要 | 显示在文章列表卡片上。不填的话会自动截取正文开头，效果往往不好 |
| `tags` | 标签 | 可以填多个。Hugo 会自动生成标签页，比如 `/tags/hugo/`，点击就能看到同标签的所有文章 |
| `categories` | 分类 | 一般只填一个，比如「技术」「生活」。会自动生成分类页 |

> **标签和分类的区别**：分类是大类，一篇文章通常只属于一个，就像书架上的区域；标签是关键词，一篇文章可以有很多个，就像书上贴的便签。

### 可选字段

| 字段 | 作用 | 什么时候用 |
|---|---|---|
| `lastmod` | 最后修改日期 | 文章发布后又修改过，就更新这个日期，页面上会显示「更新于」 |
| `slug` | 自定义网址的最后一段 | 不想用文件夹名作为网址时填写。比如 `slug: "hello"`，网址就是 `/posts/hello/` |
| `description` | 搜索引擎结果里显示的描述 | 在意搜索引擎排名时填写，不填就用 `summary` |
| `keywords` | 给搜索引擎的关键词 | 同上 |
| `series` | 所属系列的名称 | 写系列文章时用。同一系列的每篇文章，名字必须一字不差 |
| `series_order` | 在系列里排第几篇 | 和 `series` 一起使用，填数字 |
| `featureimage` | 封面图的网址 | 只有封面用**网络图片**时才需要。用本地 `feature.jpg` 不需要填 |

### 单篇覆盖全局设置

`config/_default/params.toml` 的 `[article]` 里设置的开关，对所有文章生效。如果某一篇想要不一样，可以在这篇文章的 front matter 里重新设置，**只影响这一篇**。

| 字段 | 作用 | 适用场景 |
|---|---|---|
| `showTableOfContents` | 是否显示文章目录 | 短文章没必要显示目录，设为 `false` |
| `showReadingTime` | 是否显示预计阅读时间 | — |
| `showWordCount` | 是否显示字数 | — |
| `showComments` | 是否显示评论区 | 某些文章不想开放评论，设为 `false` |
| `showHero` | 是否在顶部显示封面大图 | 封面图不适合放大时设为 `false` |
| `heroStyle` | 封面图样式 | `basic` 普通 / `big` 大图 / `background` 背景 / `thumbAndBackground` 缩略图加背景 |
| `showDate` | 是否显示日期 | 「关于」这类页面不需要日期，设为 `false` |
| `showAuthor` | 是否显示作者信息 | 同上 |

---

## 四、让新文章自动套用模板

每次都手动复制模板很麻烦。Hugo 有一个叫 **archetypes**（原型）的功能：执行新建文章命令时，自动把模板内容填进去。

### 设置方法

在博客根目录的 `archetypes` 文件夹里，新建文件 `posts.md`，内容如下：

```markdown
---
title: "{{ replace .File.ContentBaseName "-" " " | title }}"
date: {{ .Date }}
lastmod: {{ .Date }}
draft: true
summary: ""
tags: []
categories: []
# series: [""]
# series_order: 1
showTableOfContents: true
---

<!-- 在这里写引言 -->

## 
```

**这个模板做了什么：**

| 内容 | 作用 |
|---|---|
| 文件名 `posts.md` | 只对 `content/posts/` 下的新文章生效。在其他位置新建的页面（比如「关于」）仍然使用 `archetypes/default.md` |
| `{{ .Date }}` | 自动填入新建文章时的日期和时间 |
| `{{ replace .File.ContentBaseName "-" " " \| title }}` | 用文件夹名自动生成一个临时标题，比如 `my-first-post` 会变成 `My First Post`。之后改成中文标题即可 |
| `draft: true` | 新文章默认是草稿，避免没写完就被发布。**写完记得改成 `false`** |
| `# series` 那两行 | `#` 开头表示注释，不会生效。写系列文章时删掉 `#` 就能启用 |
| `<!-- ... -->` | HTML 注释，不会显示在网页上，用来提醒自己在哪里开始写 |

### 使用效果

设置好之后，执行：

```bash
npm run new -- posts/my-first-post/index.md
```

打开生成的 `index.md`，front matter 已经自动填好，只需要补充标题、摘要、标签，然后开始写正文。

---

## 五、正文写作规范

### 5.1 标题层级从 `##` 开始

front matter 里的 `title` 在页面上已经是最大的一级标题。正文里再用 `#` 的话，页面上会出现两个同样大的标题，自动生成的目录也会混乱。

```markdown
## 二级标题      ← 正文里最大的标题，会出现在目录中
### 三级标题     ← 二级标题下面的小节
#### 四级标题    ← 一般最多用到这一级
```

> `#` 后面要有一个空格：`## 标题` 是对的，`##标题` 不会被识别成标题。

### 5.2 段落和换行

```markdown
这是第一段。

这是第二段。段落之间要空一行。
```

> 只按一次回车不会换行，两行会连在一起显示。要分段就空一行。

### 5.3 文字样式

| 效果 | 写法 |
|---|---|
| **加粗** | `**加粗**` |
| *斜体* | `*斜体*` |
| ~~删除线~~ | `~~删除线~~` |
| `行内代码` | `` `行内代码` `` |

> 加粗适合标记关键结论，不要整段加粗，否则就没有重点了。

### 5.4 链接

```markdown
[Hugo 官网](https://gohugo.io)
[我的另一篇文章](/posts/hello-world/)
```

- 方括号里是显示的文字，圆括号里是网址
- 链接到自己博客的其他文章，用 `/posts/文件夹名/` 这种以 `/` 开头的路径，不用写完整域名

### 5.5 图片

图片和 `index.md` 放在同一个文件夹，直接写文件名：

```markdown
![图片说明](img-1.png)
```

- 方括号里是图片说明（图片加载失败时显示，也方便搜索引擎理解图片内容），不要留空
- 文件名建议用英文和数字，不要有空格
- 截图尽量裁剪掉无关的部分，太大的图片可以先压缩一下

### 5.6 代码块

用三个反引号把代码包起来，第一行**写上语言名称**，这样才会有语法高亮：

````markdown
```bash
npm run dev
```

```toml
title = "我的博客"
```

```javascript
console.log("Hello");
```
````

常用的语言名称：`bash`（终端命令）、`powershell`、`toml`、`yaml`、`json`、`markdown`、`html`、`css`、`javascript`、`python`、`go`、`rust`。不确定写什么就写 `text`。

> 反引号 ` 在键盘左上角，和 `~` 在同一个键上（英文输入法下）。

### 5.7 列表

```markdown
- 无序列表项
- 无序列表项
  - 缩进两个空格变成子项

1. 有序列表项
2. 有序列表项

- [ ] 待办事项
- [x] 已完成事项
```

### 5.8 引用

```markdown
> 这是一段引用，适合放名言、别人的观点或者补充说明。
```

### 5.9 表格

```markdown
| 表头 1 | 表头 2 |
|---|---|
| 内容 | 内容 |
| 内容 | 内容 |
```

- 第二行的 `|---|---|` 不能省略，它用来分隔表头和内容
- 列不需要对齐，竖线 `|` 的数量对上就行

### 5.10 分割线

```markdown
---
```

> 分割线上下各空一行。注意它和 front matter 的 `---` 长得一样，但出现在正文里就是分割线。

---

## 六、Blowfish 扩展组件（shortcodes）

Markdown 本身的功能有限，Blowfish 提供了一些额外的组件，叫 **shortcode（短代码）**。写法是两层大括号加尖括号，有开始标签和结束标签：

```
{{</* 组件名 参数="值" */>}}
内容
{{</* /组件名 */>}}
```

有些组件不需要包裹内容，只有一个标签。

### 6.1 提示框 alert

用来突出显示注意事项、警告或技巧。

```
{{</* alert */>}}
**注意**：这是一个提示框。
{{</* /alert */>}}
```

效果：

{{< alert >}}
**注意**：这是一个提示框。
{{< /alert >}}

可以自定义图标和颜色：

```
{{</* alert icon="circle-info" cardColor="#e0f2fe" textColor="#0c4a6e" iconColor="#0c4a6e" */>}}
这是一个自定义颜色的提示框。
{{</* /alert */>}}
```

| 参数 | 作用 |
|---|---|
| `icon` | 左侧图标，默认是感叹号三角形 |
| `cardColor` | 背景颜色 |
| `textColor` | 文字颜色 |
| `iconColor` | 图标颜色 |

### 6.2 引言 lead

把一段文字用大号字体显示，适合放在文章开头当作导语。

```
{{</* lead */>}}
这篇文章介绍如何用 Hugo 搭建一个属于自己的博客。
{{</* /lead */>}}
```

### 6.3 带说明的图片 figure

比普通图片语法多了图片下方的说明文字。

```
{{</* figure src="img-1.png" alt="首页截图" caption="图 1：博客首页效果" */>}}
```

| 参数 | 作用 |
|---|---|
| `src` | 图片文件名或网址 |
| `alt` | 图片描述，图片加载失败时显示 |
| `caption` | 显示在图片下方的说明文字 |

### 6.4 按钮 button

```
{{</* button href="https://github.com/你的用户名" target="_blank" */>}}
访问我的 GitHub
{{</* /button */>}}
```

| 参数 | 作用 |
|---|---|
| `href` | 点击后跳转的网址 |
| `target` | 填 `"_blank"` 表示在新标签页打开 |

### 6.5 GitHub 仓库卡片 github

自动显示一个 GitHub 仓库的名称、简介和星标数。

```
{{</* github repo="nunocoracao/blowfish" */>}}
```

### 6.6 站内文章卡片 article

在正文里插入自己博客另一篇文章的卡片，适合写「相关阅读」。

```
{{</* article link="/posts/hello-world/" */>}}
```

### 6.7 流程图 mermaid

用文字描述画出流程图。

```
{{</* mermaid */>}}
graph LR
  A[写文章] --> B[本地预览] --> C[git push] --> D[自动上线]
{{</* /mermaid */>}}
```

效果：

{{< mermaid >}}
graph LR
  A[写文章] --> B[本地预览] --> C[git push] --> D[自动上线]
{{< /mermaid >}}

> `graph LR` 表示从左到右（Left → Right）。改成 `graph TD` 就是从上到下（Top → Down）。`-->` 是箭头，方括号里是方框中的文字。更多写法见 <https://mermaid.js.org>。

### 6.8 数学公式 katex

先在文章里任意位置放一次 `katex` 组件，开启公式功能，然后就可以写公式：

```
{{</* katex */>}}

行内公式：\\(E = mc^2\\)

独立公式：
$$
\sum_{i=1}^{n} i = \frac{n(n+1)}{2}
$$
```

- 行内公式用 `\\(` 和 `\\)` 包裹，和文字在同一行
- 独立公式用 `$$` 包裹，单独占一行并居中显示

### 6.9 图标 icon 和徽章 badge

```
{{</* icon "github" */>}}

{{</* badge */>}}新文章{{</* /badge */>}}
```

### 6.10 更多组件

Blowfish 还提供画廊（`gallery`）、轮播图（`carousel`）、时间线（`timeline`）、图表（`chart`）、YouTube 视频（`youtubeLite`）等组件。不同版本的参数可能有差别，完整列表以官方文档为准：<https://blowfish.page/docs/shortcodes/>

### 6.11 ⚠️ 想在文章里展示 shortcode 的写法怎么办

如果直接把 shortcode 写进代码块里，Hugo 仍然会把它当作真的组件去执行，读者看不到代码本身。

要让它**原样显示**，需要在开始符号后面加 `/*`，在结束符号前面加 `*/`。本文所有代码块里的 shortcode 都是这样处理的，读者看到的是正常写法。打开本文的 Markdown 源文件就能看到具体怎么写。

> 这个问题只在写教程、展示组件写法时才会遇到，平时正常使用组件不需要管。

---

## 七、发布前检查清单

写完一篇文章，发布前对照检查一遍：

**文章信息**
- [ ] `draft` 改成了 `false`
- [ ] `date` 不晚于今天
- [ ] `title`、`summary`、`tags`、`categories` 都填好了
- [ ] front matter 里没有中文引号、中文逗号

**正文**
- [ ] 正文标题从 `##` 开始，层级没有跳级（比如 `##` 下面直接用 `####`）
- [ ] 段落之间空了一行
- [ ] 图片放在文章文件夹里，都有图片说明
- [ ] 代码块都标明了语言
- [ ] 链接都能正常打开

**预览和发布**
- [ ] 执行 `npm run dev`，终端没有报错
- [ ] 在浏览器里从头到尾看一遍，检查排版和图片
- [ ] 有封面需求的话，确认 `feature.jpg` 正常显示

全部确认后发布：

```bash
git add .
```

> **作用**：把所有修改加入待提交清单。

```bash
git commit -m "新文章：文章标题"
```

> **作用**：保存一个版本，引号里写这次修改的说明。

```bash
git push
```

> **作用**：上传到 GitHub。Cloudflare Pages 会在一两分钟内自动更新网站。

---

## 附：完整示例

一篇实际文章的样子，可以直接复制，然后修改成自己的内容：

```markdown
---
title: "我的第一篇博客"
date: 2026-11-02
draft: false
summary: "博客终于搭好了，记录一下为什么要写博客，以及接下来打算写什么。"
tags: ["随笔", "博客"]
categories: ["生活"]
showTableOfContents: false
---

折腾了几天，博客终于上线了。

## 为什么要写博客

一直想找个地方记录学到的东西。笔记软件里的内容太零散，**写成文章能逼自己把事情想清楚**。

## 接下来打算写什么

- 开发小工具的过程
- 踩过的坑和解决办法
- 读书笔记

![博客首页](home.png)

> 写作是思考的最好方式。
```

对应的文件夹：

```
content/posts/my-first-blog/
├── index.md
├── feature.jpg
└── home.png
```
