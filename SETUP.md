# GitHub 主页使用说明

当前已填入：名字 `沐倾`，用户名 `muqing-kg`，副标题 `把夜晚写成一行一行的光`。

主页结构：hero 大标题 → 终端 / 轨道双栏 → 滚动条幅 → 三枚徽章与访问计数 → 深夜频道（ANSI 彩色日志）→ 深夜流水线（mermaid）→ 这块主页（表格）→ 波浪分隔线。

## 一、最快上线

1. 在 GitHub 新建一个 Public 仓库，仓库名必须与用户名完全一致：`muqing-kg`。
2. 把本目录的 `README.md` 和 `assets/` 整个上传到该仓库根目录。
3. 打开 `https://github.com/muqing-kg`，主页即刻生效。

## 二、改名字 / 用户名 / 副标题

三处内容分别出现在：名字（`assets/hero.svg` 大标题）、用户名（终端面板与 hero 角落小字、终端标题栏、访问计数徽章）、副标题（`assets/hero.svg` 与 `README.md` 深夜频道）。

在本目录执行（Python 3），把等号左边的旧值换成新值：

```powershell
python -c "import pathlib;[p.write_text(p.read_text(encoding='utf-8').replace('沐倾','新名字').replace('muqing-kg','新用户名').replace('把夜晚写成一行一行的光','新副标题'),encoding='utf-8',newline='\n') for p in list(pathlib.Path('.').rglob('*.svg'))+[pathlib.Path('README.md')]]"
```

不要用 `Get-Content` / `Set-Content` 直接改写：Windows PowerShell 5.1 的默认编码会把中文写成乱码。

## 三、推送前的校验

```powershell
python -c "import xml.dom.minidom as m;[m.parse(p) for p in ['assets/hero.svg','assets/terminal.svg','assets/orbit.svg']];print('SVG OK')"
```

XML 不合法时，GitHub 上对应的图会整块空白。

## 四、常见调整

| 需求 | 改动位置 |
| --- | --- |
| 名字变长顶到边框 | `assets/hero.svg` 中三处 `font-size="136"` 调小到 110 或 96，同时三处 `letter-spacing="18"` 调到 8 |
| 副标题太长 | `assets/hero.svg` 中 `y="206"` 那行，`font-size="19"` 调小 |
| 换配色 | 每个 SVG 顶部的 `<style>` 与 `<linearGradient>` / `<radialGradient>` 里的十六进制色值 |
| 动画速度 | SVG 内的 `<animate dur="...">` 与 CSS 里的 `animation` 时长 |
| 不要访问计数徽章 | 删除 `README.md` 中 `komarev.com` 那一行 |
| 不要终端 / 轨道面板 | 删除 `README.md` 中对应的 `<img>` 行，并把保留那行的 `width="49%"` 改成 `100%` |
| 不要滚动条幅或波浪线 | 删除 `README.md` 中对应的 `<img>` 行，并删掉 `assets/ticker.svg` 或 `assets/wave.svg` |
| 条幅文案 | `assets/ticker.svg` 中两处 `<text>` 内容需同步改，两处必须完全一致才能无缝循环 |
| 不要深夜流水线 | 删除 `README.md` 中 ```mermaid 那一整段 |
| 不要深夜频道 | 删除 `README.md` 中 ```ansi 那一整段 |

## 五、设计约束与说明

- 字体只用系统字体栈（含 `PingFang SC`、`Microsoft YaHei` 等中文字族），不引用外部字体或外部图片。GitHub 把 SVG 当图片渲染时，SVG 内部不允许加载外部资源，引用了会静默失效。
- 图片使用仓库内相对路径（`assets/xxx.svg`）。若个别环境下未显示，可改用 CDN 绝对地址：`https://cdn.jsdelivr.net/gh/muqing-kg/muqing-kg@main/assets/hero.svg`（需已推送到 `main` 分支）。
- 三张图自带深色底，浅色主题与深色主题下观感一致，无需做深浅双版本。
- SVG 作为图片渲染时无法响应 hover 或点击，所有效果都是自动播放的动画。
- `README.md` 的深夜频道代码块里含真实的 ESC 控制字符（`ansi` 语法高亮用）。用会吞控制字符的编辑器改写该段落时，颜色会失效；此时把该段换成普通 `text` 代码块即可。
- 尺寸：`hero.svg` 1200x340、`terminal.svg` 与 `orbit.svg` 各 600x340、`ticker.svg` 1200x150、`wave.svg` 1200x130；README 中前一组按 100% / 49% 排版，后两张按 100% 排版。
