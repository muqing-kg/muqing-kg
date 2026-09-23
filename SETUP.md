# GitHub 主页使用说明

当前已填入：名字 `沐倾`，用户名 `muqing-kg`，副标题 `把夜晚写成一行一行的光`。

主页结构：hero 大标题 → 终端 / 轨道双栏 → 滚动条幅 → 三枚徽章与访问计数 → 深夜频道（开机日志面板）→ 深夜流水线（流程图）→ 面板清单（表格）→ 手绘笔记（三段代码）→ 这块主页（表格）→ 波浪分隔线。

## 一、最快上线

1. 在 GitHub 新建一个 Public 仓库，仓库名必须与用户名完全一致：`muqing-kg`。
2. 把本目录的 `README.md` 和 `assets/` 整个上传到该仓库根目录。
3. 打开 `https://github.com/muqing-kg`，主页即刻生效。

## 二、改名字 / 用户名 / 副标题

三处内容分别出现在：名字（`assets/hero.svg` 大标题）、用户名（`assets/hero.svg` 角落小字、`assets/terminal.svg` 标题栏、`assets/log.svg` 标题栏、README 的访问计数徽章）、副标题（`assets/hero.svg` 与 `assets/log.svg` 第二行）。

在本目录执行（Python 3），把等号左边的旧值换成新值：

```powershell
python -c "import pathlib;[p.write_text(p.read_text(encoding='utf-8').replace('沐倾','新名字').replace('muqing-kg','新用户名').replace('把夜晚写成一行一行的光','新副标题'),encoding='utf-8',newline='\n') for p in list(pathlib.Path('.').rglob('*.svg'))+[pathlib.Path('README.md')]]"
```

不要用 `Get-Content` / `Set-Content` 直接改写：Windows PowerShell 5.1 的默认编码会把中文写成乱码。

## 三、推送前的校验

```powershell
python -c "import glob,xml.dom.minidom as m;[m.parse(p) for p in glob.glob('assets/*.svg')];print('SVG OK')"
```

XML 不合法时，GitHub 上对应的图会整块空白。README 里引用的文件名要与 `assets/` 下实际存在的文件一一对应，改图或删图时两边都要动。

## 四、常见调整

| 需求 | 改动位置 |
| --- | --- |
| 名字变长顶到边框 | `assets/hero.svg` 中三处 `font-size="150"` 调小到 120 或 104，同时三处 `letter-spacing="20"` 调到 10 |
| 换大标题字体 | `assets/hero.svg` 中 `.kai` 那条 `font-family`；楷体缺失的机器会退到 `Songti SC` / 宋体 / 衬线，观感仍然区别于默认黑体 |
| 副标题太长 | `assets/hero.svg` 中 `y="206"` 那行，`font-size="19"` 调小 |
| 换配色 | 每个 SVG 顶部的 `<style>` 与 `<linearGradient>` / `<radialGradient>` 里的十六进制色值 |
| 动画速度 | SVG 内的 `<animate dur="...">` 与 CSS 里的 `animation` 时长 |
| 不要访问计数徽章 | 删除 `README.md` 中 `komarev.com` 那一行 |
| 不要终端 / 轨道面板 | 删除 `README.md` 中对应的 `<img>` 行，并把保留那行的 `width="49%"` 改成 `100%` |
| 不要滚动条幅 / 波浪线 / 流程图 / 开机日志 | 删除 `README.md` 中对应的 `<img>` 行，并删掉 `assets/` 下对应文件 |
| 条幅文案 | `assets/ticker.svg` 中两处 `<text>` 内容需同步改，两处必须完全一致才能无缝循环；改文案或改字数时，同步调整两处的 `textLength`、第二个 `<text>` 的 `x`（等于第一个 `x` 加 `textLength`）与 `@keyframes marquee` 的位移量，三者必须相等 |
| 开机日志的文字 | `assets/log.svg` 中一行一个 `<g class="row">`，改里面的 `<text>` 即可；增删行时同步改「面板清单」与「这块主页」里的行数描述 |
| 日志逐行浮现的节奏 | `assets/log.svg` 中每行 `<g>` 的 `animation-delay`，现在是每行相差 `0.42s` |
| 流程图节点文字 | `assets/flow.svg` 里五个 `<rect>` 加一个菱形，文字在各自下方的 `<text>` 中，节点宽度与文字长度不匹配时改 `<rect>` 的 `width` 与 `x` |

## 五、设计约束与说明

- 字体只用系统字体栈（含 `PingFang SC`、`Microsoft YaHei` 等中文字族），不引用外部字体或外部图片。GitHub 把 SVG 当图片渲染时，SVG 内部不允许加载外部资源，引用了会静默失效。
- 图片使用仓库内相对路径（`assets/xxx.svg`）。若个别环境下未显示，可改用 CDN 绝对地址：`https://cdn.jsdelivr.net/gh/muqing-kg/muqing-kg@main/assets/hero.svg`（需已推送到 `main` 分支）。
- 七张图都自带深色底，浅色主题与深色主题下观感一致，无需做深浅双版本。
- SVG 作为图片渲染时无法响应 hover 或点击，所有效果都是自动播放的动画。
- 尺寸：`hero.svg` 1200x340、`terminal.svg` 与 `orbit.svg` 各 600x340、`ticker.svg` 1200x150、`log.svg` 1200x380、`flow.svg` 1200x300、`wave.svg` 1200x130；README 中第一组按 100% / 49% 排版，其余按 100% 排版。
- 已放弃两种客户端二次渲染：mermaid 与 ```` ```ansi ```` 彩色代码块。前者加载失败时访客只会看到一坨源码，后者的着色完全依赖前端脚本，本地无法验证是否真的上色。深夜频道因此改成手绘 SVG 面板，观感与渲染结果都握在自己手里。
- 加新面板时：SVG 放进 `assets/`，README 用 `<img src="assets/xxx.svg" width="100%">` 引入，并同步「面板清单」表格与本节尺寸列表。
