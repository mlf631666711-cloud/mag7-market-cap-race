# Remotion Gallery

**单文件 HTML 动画作品集 · Single-file HTML animation showcase**

**中文** · [English](README.en.md)

![Remotion Gallery](preview.png)

▶ **在线直接看（点开即播）**：<https://mlf631666711-cloud.github.io/remotion-gallery/>

这里放我们做的 HTML 动画：每个作品都是一份**零依赖的单文件 HTML**，双击即播、不需要构建、源码可读，同时附带渲染好的 MP4 成片。

## 作品

| # | 作品 | 时长 | 在线看 | 源码 |
|---|---|---|---|---|
| 01 | 科技七雄 · 十年市值赛跑<br /><sub>Magnificent 7 · Market Cap Race</sub> | 10.4s | [▶ 播放](https://mlf631666711-cloud.github.io/remotion-gallery/demos/mag7-market-cap-race/) | [目录](demos/mag7-market-cap-race/) |

## 目录结构

```
.
├── index.html                      # 画廊首页（卡片墙）
├── preview.png                     # 仓库封面 / 社交分享图
├── favicon.svg
├── README.md  README.en.md  LICENSE
└── demos/
    └── mag7-market-cap-race/       # 作品 01
        ├── index.html              # 详情页（在线播放 + 入口）
        ├── effects.html            # 主文件：双击即播
        ├── effects.render.html     # 出片用：可逐帧 seek
        ├── mag7_1920x1080.mp4      # 成片
        ├── preview.png             # 1920×1080 落版静帧
        └── thumb.png               # 960 宽卡片缩略图
```

## 加一个新作品

**1. 建目录** —— 一个作品一个目录，别往根目录堆：

```
demos/<slug>/            # slug 用短横线命名，如 spring-launch-teaser
  ├── index.html         # 详情页（可复制作品 01 的改）
  ├── <主文件>.html       # 零依赖，双击即播
  ├── <主文件>.render.html# 暴露 window.__seek(t) / window.__duration
  ├── <slug>_1920x1080.mp4
  ├── preview.png        # 1920×1080 落版静帧
  └── thumb.png          # 卡片缩略图
```

**2. 压一张卡片缩略图**（首页别直接挂 1080p 原图）：

```bash
ffmpeg -y -i preview.png -vf scale=960:-1 thumb.png
```

**3. 上首页** —— 在根 `index.html` 的 `.grid` 里复制一整段 `<article class="card">`，改封面路径、标题、描述、标签、三个链接、左上角序号。

**4. 登记** —— 在上面那张「作品」表里加一行，然后：

```bash
git add -A && git commit -m "add: <作品名>" && git push
```

## 所有作品共同遵守的约定

- **零依赖**：不引 CDN、不装包。一份 HTML 丢进浏览器就能跑。
- **一作品两版本**：主文件负责"好看好播"，`.render.html` 负责"可出片"——暴露 `window.__seek(t)` 与 `window.__duration`，剥离 CSS 动画与一切累积状态，保证同一个 `t` 反复渲染结果完全一致。
- **详情页可播**：每个 `demos/<slug>/index.html` 打开就能看片，并带返回画廊的入口。
- **成片命名**：`<slug>_1920x1080.mp4`，1920×1080 / 30fps / H.264。

---

## 作品 01 · 科技七雄 · 十年市值赛跑

**Magnificent 7 · A Decade of Market Cap（2014 → 2024）**

▶ <https://mlf631666711-cloud.github.io/remotion-gallery/demos/mag7-market-cap-race/>

七家科技巨头十年市值条形赛跑。片头逐字弹入 + 扫光，条形按真实市值逐年插值增长、名次实时换位，落版突出英伟达十年约 300 倍。

> 以下提到的文件名都在 `demos/mag7-market-cap-race/` 目录下。

### 文件

| 文件 | 说明 |
|---|---|
| `effects.html` | **原始版**。自包含单文件（零外部依赖），双击即播。由 CSS `@keyframes` + `requestAnimationFrame` 驱动 |
| `effects.render.html` | **逐帧渲染版**。暴露 `window.__seek(t)` / `window.__duration`，剥离全部 CSS 动画与累积状态（粒子改确定性重放），任意 t 可独立求解 —— 用于稳定逐帧出片 |
| `mag7_1920x1080.mp4` | 成片：1920×1080 / 30fps / 10.4s / 1.6MB |
| `preview.png` | 落版静帧（详情页封面） |

### 画面结构（总长 10.4s）

| 段 | 时间 | 内容 |
|---|---|---|
| 片头 | 0 – 1.75s | kicker + 主标题逐字弹入（`translateY(34px) scale(.6)` → 归位），扫光带自左向右掠过 |
| 赛跑 | 1.75 – 9.0s | 7 根条形按市值线性插值增长，`dynMax = maxV × 1.08` 动态定标；排名用 `_disp += (rank - _disp) × 0.16` 缓动换位；年号跟随 |
| 落版 | 9.0 – 10.4s | 字幕条：英伟达 NVDA 十年 `$11B → $3,355B`，暴涨约 **300 倍** |

### 数据（单位：十亿美元）

| 公司 | 2014 | 2015 | 2016 | 2017 | 2018 | 2019 | 2020 | 2021 | 2022 | 2023 | 2024 |
|---|---|---|---|---|---|---|---|---|---|---|---|
| 苹果 AAPL | 643 | 584 | 609 | 861 | 746 | 1287 | 2255 | 2901 | 2066 | 2994 | **3863** |
| 微软 MSFT | 382 | 440 | 483 | 660 | 780 | 1200 | 1681 | 2522 | 1787 | 2794 | **3200** |
| 英伟达 NVDA | 11 | 18 | 58 | 117 | 81 | 144 | 323 | 735 | 364 | 1223 | **3355** |
| 谷歌 GOOG | 360 | 528 | 539 | 729 | 724 | 921 | 1185 | 1917 | 1145 | 1756 | **2365** |
| 亚马逊 AMZN | 144 | 318 | 356 | 564 | 737 | 920 | 1634 | 1691 | 857 | 1570 | **2352** |
| Meta | 217 | 297 | 332 | 513 | 374 | 585 | 778 | 922 | 320 | 910 | **1514** |
| 特斯拉 TSLA | 28 | 32 | 34 | 52 | 57 | 76 | 669 | 1061 | 389 | 790 | **1385** |

数据来源：Visual Capitalist / CompaniesMarketCap（单位：十亿美元，取每年年末口径）

### 改造成你自己的内容

全部内容都在 `PRESETS` 数组里，加一个预设对象即可：

```js
{
  id:'your-id', label:'下拉里显示的名字',
  kicker:'顶部小字', title:'主标题', sub:'副标题', unit:'单位行',
  years:[2020,2021,2022,2023,2024],
  fmt: v => '$' + Math.round(v) + 'B',
  outro:{ emoji:'🏆', title:'落版标题', desc:'落版描述' },
  companies:[ {key,name,tk,c,v:[...每年一个数]} ]
}
```

### 逐帧渲染成片（如何从 HTML 得到 mp4）

```
1. 用 Playwright/Puppeteer 打开 effects.render.html
2. 逐 t（t = i/30，i = 0..N-1）执行 window.__seek(t)，截图
3. ffmpeg -framerate 30 -i f_%04d.png -c:v libx264 -crf 14 -pix_fmt yuv420p out.mp4
```

若要高倍超采样：截图时用 `deviceScaleFactor: 2`（渲染 3840×2160），编码时 `-vf scale=1920:1080` 降采样。

### 两个实现细节（改的时候别踩）

- **条宽有夹取**：`pct = clamp(v / dynMax * 100, 2, 94)`。所以最小值那根（特斯拉）看起来比线性比例长，这是设计下限 2%，不是 bug。
- **`effects.html` 不满足逐帧 seek 契约**：它用 CSS 动画 + rAF 累积状态，同一 t 两次渲染结果可能不同。要出片请用 `effects.render.html`。

## 许可

代码可自由参考 / remix。各作品的数据为公开来源整理，署名见作品小节。
