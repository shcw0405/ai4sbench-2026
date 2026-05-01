# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 仓库性质

这是 **Foundation Models in the Wild** 系列 workshop 的官方网站,通过 GitHub Pages 托管(仓库名 `fm-wild-community.github.io`)。这是一个 **纯静态 HTML 站点,没有构建系统**:没有 Jekyll、没有 npm/yarn、没有 CI(`.github/workflows/main.yml` 是空文件),GitHub Pages 直接从 `main` 分支根目录提供文件。

## 本地预览

无需安装依赖,直接启动一个静态服务器即可:

```bash
python3 -m http.server 8000
# 然后访问 http://localhost:8000/index.html 或 /index_2024.html
```

也可以直接用浏览器打开 `index.html`,但相对路径的字体 CSS 在 `file://` 下可能无法加载,推荐用 HTTP 服务器。

## 页面与归档约定

每届 workshop 在仓库里以一个独立的 HTML 文件归档,**根目录的 `index.html` 始终代表"当前/最新的"那一届**:

- `index.html` — 当前届:**ICLR 2025**(2nd Workshop, 2025 年 4 月 27 日)
- `index_2024.html` — 上一届:**ICML 2024**(1st Workshop)

`index.html` 的 header 里通过 `<a href="index_2024.html" class="btn">ICML 2024</a> ` 提供回到上一届的入口。新增下一届时遵循同样模式:把当前 `index.html` 复制为 `index_<year>.html`,再把新一届写入 `index.html`,并在新 header 里加回上一届的链接。

## 资源目录的特殊性

资源目录有历史遗留的重复,**修改前必须确认正在编辑的页面引用的是哪一份**:

- `files_new/` — `index.html` (ICLR 2025) **实际引用的目录**,同时混放了:
  - 组织者/演讲者照片(以姓名首字母命名,如 `AA.jpeg`、`Chelsea_Finn.jpeg`)
  - 该届实际使用的 CSS/JS(`bootstrap.min.css`、`agency.css`、`agency.js` 等)
  - 无扩展名的 `css`、`css(1)`、`css(2)`、`css(3)`、`css2` —— 这些是 Google Fonts 等外链 CSS 被另存为本地副本,通过 `<link href="./files_new/css">` 加载,**不是误命名,不要"修复"**
- `css/`、`js/`、`stylesheets/` — 早期版本遗留,`index.html` 已不引用(`index_2024.html` 部分引用),改 ICLR 2025 页面样式时改 `files_new/` 下的副本,**不是** `css/` 下的同名文件
- `img/` — 仅存放 header 背景图(`bg.avif`/`bg.jpeg` 等)
- `favicon/`、`favicon.ico` — favicon 资源

新增演讲者/组织者头像时按现有命名约定(姓名首字母大写,如 `XY.jpeg`)放进 `files_new/`,然后在对应 HTML 的 `#speaker` 或 `#organization` section 内加 `<div class="team-member">` 卡片。

## 论文样式文件

`iclr_fmwild.sty` 和 `icml_fmwild.sty` 是给投稿者下载的 LaTeX 样式文件(从 `index.html` 的 Call for Papers section 直链),属于站点产物,不是站点本身的依赖。改样式文件不会影响网站渲染。

## HTML 结构约定

每届的 `index*.html` 都是单页布局,内部用 `<section id="...">` 锚点串起来,导航栏的 `.page-scroll` 链接靠 jQuery + `cbpAnimatedHeader.js` 滚动。常见 section 顺序:`#news` → `#introduction` → `#call` → `#program`(schedule)→ `#speaker` → `#organization`。每届会按需加 `#dates`、`#sponsor` 等。

样式大量使用页内 `<style>` 块覆盖 Bootstrap,而非集中在 `agency.css`,**改某个 section 的视觉表现时优先在该 section 自带的 `<style>` 里改**,避免全局 CSS 牵连其他届的存档页面。

## 部署

push 到 `main` 即上线,无需手工触发。`CNAME` 文件为空(未绑定自定义域名),站点对外地址为仓库的 `*.github.io` 域名。`params.json` 是早年 GitHub Pages 自动生成器留下的元数据文件,文件本身注释里写了 "Don't delete this file",保留即可。
