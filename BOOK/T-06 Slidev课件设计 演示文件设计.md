# T-06 Slidev课件设计 演示文件设计

> **模板编号**：T-06
> **文件角色**：🎬 Slidev 演示文件（可直接渲染的 slides.md）
> **所属阶段**：课件制作阶段（最终产物）
> **配套输入**：T-05 Slidev课件设计 脚本安排设计.md
> **配套下游**：Slidev 渲染产物（`slides/第X课-标题/slides.md`）

> **使用场景**：在 `T-05 Slidev 脚本安排` 已经把"每张幻灯片该长什么样"敲定之后，
> 按 015 的脚本表，**逐页翻译**成 Slidev 能直接渲染的 Markdown 文件。
>
> **核心目标**：本文件**直接复制**到 `slides/第X课-标题/slides.md` 即可被 `npx slidev` 渲染为浏览器演示稿，
> 不需要再二次排版。
>
> **所属章节**：第 ____ 章 第 ____ 节
> **课时类型**：□ 📘 知识讲解课　□ 🐢 海龟绘图课　□ 📝 考级练习课　□ 🔄 章节复习课
> **课时长度**：120 分钟（2 小时）
> **本课涉及的知识点编号**：0001、0002、0003 ……
> **配套 T-05 脚本**：`BOOK/T-05 Slidev课件设计 脚本安排设计.md`（本课实例）
> **配套 T-02 教材草稿**：`BOOK/NN 章节名/草稿 012 第 X 课_标题.md`

---

## 📦 一、文件结构（目录与依赖）

> 一份完整的 Slidev 课件**至少**需要 3 类文件，**不要把所有内容塞进一个 md**。

```
slides/第X课-标题/
├── slides.md            # 本模板（主讲稿）
├── package.json         # 主题与依赖锁定
├── components/          # 自定义 Vue 组件（可选）
│   └── MultiTrixBadge.vue   # Multi-TRIX 角标 / Bug 标记 等
├── public/              # 静态资源（图片 / 视频 / 字体）
│   ├── cover.png        # 封面插图
│   ├── turtle-demo.gif  # 海龟课演示动画
│   └── bgm.mp3          # （可选）背景音乐
└── snippets/            # 外部代码片段（可选）
    └── example.py
```

### package.json（最小可用）

```json
{
  "name": "mtrix-lesson-x",
  "private": true,
  "scripts": {
    "dev": "slidev --open",
    "build": "slidev build",
    "export": "slidev export"
  },
  "dependencies": {
    "@slidev/theme-seriph": "^0.20.0",
    "vue": "^3.4.0"
  }
}
```

---

## 🎨 二、全局 frontmatter（写在 slides.md 顶部）

> 这些字段**控制整份课件的渲染行为**，每节课都**只改**带 `[ ]` 的占位项。

```yaml
---
theme: seriph              # [ ] 主题名（默认 seriph；海龟课可换 apple-basic）
title: 第 X 课：标题        # [ ] 浏览器标签页标题
info: |
  ## Multi-TRIX THON 教学手册
  第 X 章 第 X 节：标题
  配套 T-02 教材草稿 + T-05 脚本安排
author:                    # [ ] 主讲老师（多个用数组）
  - Multi-Boy 老师
keywords: python, mtrix, gesp, 1-2-3-4
favicon: /favicon.ico
class: text-center          # [ ] 全局文本对齐：text-center / text-left
highlighter: shiki
lineNumbers: true           # 代码块显示行号
mdc: true                   # 启用 MDC 语法
fonts:
  sans: 'Inter, "Noto Sans SC", sans-serif'   # 思源黑体 / Inter
  serif: 'serif'
  mono: 'Fira Code, monospace'
aspectRatio: 16/9           # 课件比例（与投影仪一致）
canvasWidth: 960            # 画布宽（不要超过 980 避免溢出）
addons:
  - slidev-addon-components
transition: slide-left      # [ ] 全局转场（与 015 锁定一致）
---
```

---

## 🎞 二·a、媒体素材清单 + MiniMax(mmx) 命令速查

> **为什么单独列一节**：课件里**几乎每一页**都可能用到图片、视频、音频。
> 提前**一次性**把所有素材列清楚、生成/准备好，可以避免写到第 20 页才发现"封面图还没画"。
> 全部执行的mmx指令全部在脚本安排的末尾记录下来，方便用户后续修改完善

### 1. 协议总览：mmx 生成 vs 手动填充

| 方式                | 何时用                                               | 标记                                          | 优势                               |
| :------------------ | :--------------------------------------------------- | :-------------------------------------------- | :--------------------------------- |
| **`[MMX: ...]`**    | 用 `mmx` 命令**自动生成**（图片/视频/音频/语音）     | `[MMX 图]` / `[MMX 音]` / `[MMX 视]`          | 一次跑命令自动出素材，**风格统一** |
| **`[MANUAL: ...]`** | `mmx` 不可用 / 想用**自有素材库** / 想要手工精细控制 | `[MANUAL 图]` / `[MANUAL 音]` / `[MANUAL 视]` | **完全控制**版权、风格、可商用性   |

> 两种标记**只能二选一**。
> - 想**省事 / 风格统一** → 选 `[MMX]`。
> - 想**正式发布 / 自有素材** → 选 `[MANUAL]`，把图片描述写进清单，老师用 Midjourney / Stable Diffusion / 摄影等方式自行准备。

### 2. 媒体素材清单表（**每课必填**）

> 把所有需要的素材**一次性**列清楚。每节课开写前先填这张表，再开跑 mmx。

|   #   | 类型   | 文件名（建议）     |   所在页面   | 用途 / 描述（给 mmx 或老师看）                                  |     方式      |   状态   |
| :---: | :----- | :----------------- | :----------: | :-------------------------------------------------------------- | :-----------: | :------: |
|   1   | 🖼 图片 | `cover.png`        |    A.封面    | 课程主题插图：Multi-Boy 站在 Minetest 草原，手持 [本课核心比喻物品] |  `[MMX 图]`   | ⬜ 待生成 |
|   2   | 🎵 音频 | `bgm-main.mp3`     |   全课背景   | 轻快儿童探险风，60~120s 平滑循环                                |  `[MMX 音]`   | ⬜ 待生成 |
|   3   | 🖼 图片 | `kpt-{name}.png`   |  C.7 / C.10  | 知识点 1/2 的拟物图（如 `kpt-print.png` = 喊话器）              |  `[MMX 图]`   | ⬜ 待生成 |
|   4   | 🎬 视频 | `demo-{topic}.mp4` | D.第一次运行 | 海龟绘图录屏 / 代码运行录屏，5~10s                              | `[MANUAL 视]` | ⬜ 待录制 |
|   5   | 🔊 音效 | `sfx-bug.mp3`      |   E.Bug 课   | "哐当"一声，红色报错时播放                                      |  `[MMX 音]`   | ⬜ 待生成 |
|   6   | 🔊 音效 | `sfx-win.mp3`      |  G.通关仪式  | 胜利号角 3 秒                                                   |  `[MMX 音]`   | ⬜ 待生成 |
|   7   | 🖼 图片 | `end-bg.png`       |    H.结尾    | 课程收尾插图：Multi-Boy 挥手告别                                    |  `[MMX 图]`   | ⬜ 待生成 |

> 状态列：`⬜ 待生成` / `🟡 生成中` / `✅ 已就位`。
> 知识点拟物图数量 = 本课新知识点数量（如本节 2 个新知识点 = 2 张图）。

### 3. mmx 常用命令速查

> **`mmx` 是什么**：[minimax 公司](https://minimax.io) 的命令行 AI 工具（`mmx-cli` skill），
> 支持文本对话、**图片生成、语音合成、音乐生成、视频生成**、视觉理解、联网搜索六大能力。
>
> **安装与登录**（已使用npm全局安装，直接可用）：
> ```bash
> mmx auth status                        # 验证登录
> mmx quota show                         # 查看配额
> ```
>
> **Agent 常用 flag**（AI 自动化场景下建议加）：
> | Flag                | 作用                            |
> | :------------------ | :------------------------------ |
> | `--non-interactive` | 缺参数时立即失败，不交互式提示  |
> | `--quiet`           | 抑制进度条，stdout 只输出纯数据 |
> | `--output json`     | 输出 JSON 格式（便于 jq 解析）  |
> | `--yes`             | 跳过确认提示                    |
> | `--dry-run`         | 预览 API 请求但不实际执行       |

#### 3.1 🖼 生成图片（`mmx image generate`）

> **模型**：`image-01`　|　**注意**：图片**不能直接保存到指定文件名**，需用 `--out-dir` + `--out-prefix`，生成后**手动重命名**到 `public/`。

```bash
# === 1. 基础用法：单张图片（生成到临时目录，再用 mv 重命名） ===
mmx image generate \
  --prompt "Multi-Boy 冒险者站在 Minetest 草原上，手持一个发光的喊话器，背景是方块草地，鲜艳卡通风格，儿童友好" \
  --aspect-ratio 16:9 \
  --out-dir ./tmp-img/ --out-prefix cover --quiet
# stdout: ./tmp-img/cover-1.png
mv ./tmp-img/cover-1.png public/cover.png

# === 2. 保持角色一致性：用 --subject-ref 引用第一张图作为角色锚点 ===
# 先生成一张"标准 Multi-Boy"作为参考
mmx image generate \
  --prompt "Multi-Boy 冒险者正面立绘：12 岁男孩，卡通风格，蓝色 T 恤，棕色短发，红色背包" \
  --aspect-ratio 1:1 --out-dir ./tmp-img/ --out-prefix mboy-ref --quiet

# 后续所有插图都引用上面这张，保持角色一致
mmx image generate \
  --prompt "Multi-Boy 站在宝箱前打开战利品，表情惊喜" \
  --subject-ref "type=character,image=./tmp-img/mboy-ref-1.png" \
  --aspect-ratio 16:9 --out-dir ./tmp-img/ --out-prefix kpt-1 --quiet
mv ./tmp-img/kpt-1-1.png public/kpt-1.png

# === 3. 批量生成知识点拟物图（**先把 4~6 张图一次性生成好**） ===
for kpt in print variable list len index append; do
  mmx image generate \
    --prompt "把 Python 的 ${kpt} 概念拟物化为一个具体的 Minetest 物品，鲜艳卡通，2D 平面插图，儿童友好" \
    --aspect-ratio 1:1 --out-dir ./tmp-img/ --out-prefix "kpt-${kpt}" --quiet
  mv ./tmp-img/kpt-${kpt}-1.png public/kpt-${kpt}.png
done
```

**常用 flag**：

| Flag                         | 说明                                                          |
| :--------------------------- | :------------------------------------------------------------ |
| `--prompt`                   | 必填，图片描述（**用英文写最稳**）                            |
| `--aspect-ratio`             | 比例（`16:9` / `1:1` / `4:3` 等），与 `--width/--height` 互斥 |
| `--n`                        | 生成数量（默认 1）                                            |
| `--subject-ref`              | 角色一致性锚点（`type=character,image=path`）                 |
| `--out-dir` + `--out-prefix` | 输出到目录，文件名形如 `{prefix}-1.png`                       |
| `--prompt-optimizer`         | 自动优化 prompt                                               |
| `--aigc-watermark`           | 嵌入 AI 生成水印（**关闭**）                                  |
| `--seed`                     | 随机种子（复现结果）                                          |

#### 3.2 🎵 生成音频（`mmx music generate` / `mmx speech synthesize`）

> mmx **没有专门的 sfx 命令**，短音效有两种做法：
> 1. 用 `mmx music generate --instrumental` 生成**极短的音乐片段**作为音效（推荐）。
> 2. 用 `mmx speech synthesize` + 单音节文本（如 "咔嚓！"）+ `--speed` 加速。
>
> **音乐模型**：`music-2.6-free`（API key 用户无限量，RPM=3）。
> **语音模型**：`speech-2.8-hd`（默认）/ `speech-2.6` / `speech-02`。

```bash
# === 1. 背景音乐 BGM（轻快、循环、无人声） ===
mmx music generate \
  --prompt "轻快、活泼的儿童探险风格背景音乐，钢琴 + 木琴主旋律，阳光明亮，无人声，60~120s 平滑循环" \
  --instrumental \
  --genre "children's music" \
  --mood "uplifting, cheerful" \
  --instruments "piano, xylophone, glockenspiel" \
  --bpm 110 \
  --out public/bgm-main.mp3 --quiet
# stdout: public/bgm-main.mp3

# === 2. Bug 出现音效（短促、不吓人） ===
# 做法 A：用 music 生成 2 秒短片段
mmx music generate \
  --prompt "游戏错误提示音，短促的'哐当'一声，幽默不吓人，木琴下行音阶" \
  --instrumental --instruments "xylophone" --bpm 80 \
  --out public/sfx-bug.mp3 --quiet

# 做法 B：用 speech 合成单音节（适合语音类音效，如"哎呀！"）
mmx speech synthesize --text "哎呀！" --voice Chinese_male_child \
  --speed 1.5 --out public/sfx-bug.mp3 --quiet

# === 3. 通关音效（欢快号角） ===
mmx music generate \
  --prompt "通关胜利号角，3 秒，欢快上扬，铜管 + 钟琴" \
  --instrumental --instruments "trumpet, glockenspiel" --bpm 140 \
  --out public/sfx-win.mp3 --quiet

# === 4. 老师口播 TTS（给课件预录旁白） ===
mmx speech synthesize \
  --text "同学们好，今天我们学习 print 函数。它就像一个喊话器，能把你说的话显示在屏幕上。" \
  --voice Chinese_male_narrator \
  --speed 1.0 --subtitles \
  --out public/narration-greeting.mp3 --quiet
# --subtitles 会同时生成 .srt 字幕文件，便于课件嵌入
```

**音乐常用 flag**：

| Flag                                             | 说明                                    |
| :----------------------------------------------- | :-------------------------------------- |
| `--prompt`                                       | 音乐风格描述（推荐**英文 + 详细结构**） |
| `--instrumental`                                 | **无人声纯音乐**（BGM 必加）            |
| `--lyrics` / `--lyrics-file`                     | 歌词（与 `--instrumental` 互斥）        |
| `--lyrics-optimizer`                             | 自动生成歌词                            |
| `--vocals`                                       | 人声特征（如 `warm male baritone`）     |
| `--genre` / `--mood` / `--instruments` / `--bpm` | 流派 / 情绪 / 乐器 / 速度               |
| `--out`                                          | 直接保存到指定文件路径 ✅                |
| `--format`                                       | 输出格式（默认 `mp3`）                  |

**语音常用 flag**：

| Flag                     | 说明                                                                              |
| :----------------------- | :-------------------------------------------------------------------------------- |
| `--text` / `--text-file` | 必填，要合成的文字                                                                |
| `--voice`                | 音色 ID（如 `Chinese_male_narrator`，**用 `mmx speech synthesize --help` 查表**） |
| `--speed`                | 语速倍数                                                                          |
| `--subtitles`            | 同步生成 `.srt` 字幕                                                              |
| `--out`                  | 直接保存到指定文件路径 ✅                                                          |
| `--sound-effect`         | 加音效（**实验性**）                                                              |

#### 3.3 🎬 生成视频（`mmx video generate`）

> **模型**：`MiniMax-Hailuo-2.3`（默认）/ `MiniMax-Hailuo-2.3-Fast`（更快）。
> **特性**：视频生成是**异步任务**，可立即返回 task ID 后台轮询。

```bash
# === 1. 同步等待：直接下载到指定文件 ===
mmx video generate \
  --prompt "2D 卡通风格，2D 卡通风格，2D 卡通风格，重要的事情说三遍。小海龟助手从坐标 (0,0) 出发，依次 forward(100)、left(90)、forward(100)、left(90)、forward(100)、left(90)、forward(100)，画出一个正方形，每步 0.5 秒" \
  --download public/demo-turtle-square.mp4 --quiet
# stdout: public/demo-turtle-square.mp4（阻塞等待生成完成）

# === 2. 异步任务：拿到 task ID 后可稍后再来下载 ===
TASK_ID=$(mmx video generate \
  --prompt "海龟画五角星，5 秒" --async --quiet | jq -r '.taskId')
echo "已提交任务：$TASK_ID"
# ... 之后用 task ID 查状态 / 下载
mmx video task get --task-id "$TASK_ID" --output json
mmx video download --task-id "$TASK_ID" --out public/demo-star.mp4

# === 3. 用首帧图引导视频（推荐：先有图 → 再生成视频） ===
# 先生成海龟的"第一帧"
mmx image generate --prompt "海龟从 (0,0) 出发，初始朝向东方" \
  --aspect-ratio 16:9 --out-dir ./tmp-img/ --out-prefix turtle-start --quiet
# 把首帧当 input
mmx video generate \
  --prompt "小海龟按 forward(100) left(90) 的命令边走边画" \
  --first-frame ./tmp-img/turtle-start-1.png \
  --download public/demo-turtle.mp4 --quiet
```

**视频常用 flag**：

| Flag                    | 说明                                                                      |
| :---------------------- | :------------------------------------------------------------------------ |
| `--prompt`              | 必填，视频描述（**用英文最稳**）                                          |
| `--model`               | `MiniMax-Hailuo-2.3`（默认，质量高）/ `MiniMax-Hailuo-2.3-Fast`（速度快） |
| `--first-frame`         | 用首帧图引导（提升一致性）                                                |
| `--download`            | 阻塞等待 + 直接保存到指定文件 ✅                                           |
| `--async` / `--no-wait` | 立即返回 task ID，不阻塞                                                  |
| `--callback-url`        | 完成后 webhook 回调                                                       |
| `--poll-interval`       | 轮询间隔秒数（默认 5）                                                    |

**视频任务查询**：

```bash
mmx video task get --task-id <TASK_ID> --output json   # 查状态
mmx video download --task-id <TASK_ID> --out path.mp4  # 下载成品
```

### 4. 命名与目录规范

> **强约束**：所有素材**必须**放在 `public/` 下，命名严格遵循下表（避免风格混乱）。

```
public/
├── cover.png            # 封面（命名固定，全课统一）
├── end-bg.png           # 结尾（命名固定）
├── bgm-main.mp3         # 主背景音乐
├── bgm-bug.mp3          # Bug 环节专用紧张感 BGM（可选）
├── sfx-{name}.mp3       # 短音效（如 sfx-bug / sfx-win / sfx-click）
├── kpt-{name}.png       # 知识点拟物图（kpt = knowledge point）
├── kp-{name}.png        # 同一知识点不同页的变体（kp = knowledge point page）
├── demo-{topic}.mp4     # 演示视频（录屏 / 动画）
├── char-{name}.png      # 角色立绘（Multi-Boy / 海龟助手 / 村长等）
└── bg-{theme}.png       # 章节背景图
```

### 5. 在 slides.md 中引用素材

> Slidev 把 `public/` 下的文件**直接映射到根路径**，所以 `public/cover.png` 在 markdown 里**写** `/cover.png`。

```markdown
---
layout: cover
background: /cover.png          # 封面图，引用 public/cover.png
---

# 第 X 课：标题

<!-- 单张插图 -->
![print() 喊话器](/kpt-print.png)

<!-- 视频自动播放 -->
<video src="/demo-turtle-square.mp4" autoplay loop muted class="rounded shadow" />

<!-- 背景音乐（多数浏览器要求点击触发） -->
<audio src="/bgm-main.mp3" autoplay loop />

<!-- 全屏背景图（半透明叠加在文字下方） -->
![bg](/bg-magic-bag.png){.absolute inset-0 w-full h-full object-cover opacity-30}
```

### 6. 媒体素材的"半自动"工作流

> **推荐流程**：先把清单表填好 → **批量跑 mmx** → 把生成的素材人工**抽样检查** → 不满意的**重新生成**或**改用 [MANUAL]**。

```bash
# === 0. 准备：登录 + 准备目录 ===
cd slides/第X课-标题
mmx auth status                          # 确认已登录
mkdir -p public tmp-img                  # public 放最终素材，tmp-img 放临时

# === 1. 一次性生成封面 + 结尾 + 主背景音乐 ===
mmx image generate --prompt "课程封面：Multi-Boy 站在 Minetest 主世界，手持本课核心比喻物品，鲜艳卡通 2D" \
  --aspect-ratio 16:9 --out-dir ./tmp-img/ --out-prefix cover --quiet
mv ./tmp-img/cover-1.png public/cover.png

mmx image generate --prompt "课程结尾：Multi-Boy 站在城堡顶端挥手，背景夕阳，鲜艳卡通 2D" \
  --aspect-ratio 16:9 --out-dir ./tmp-img/ --out-prefix endbg --quiet
mv ./tmp-img/endbg-1.png public/end-bg.png

mmx music generate --prompt "轻快儿童探险风 BGM，钢琴+木琴，阳光明亮" \
  --instrumental --bpm 110 --out public/bgm-main.mp3 --quiet

# === 2. 批量生成所有 [MMX] 项 ===
# （按清单表逐条跑命令，建议用 5.1 节的 for 循环）

# === 3. 验证全部就位 ===
ls -lh public/
# 期望：看到 cover.png / end-bg.png / kpt-*.png / sfx-*.mp3 / demo-*.mp4 / bgm-*.mp3

# === 4. 清理临时文件 ===
rm -rf ./tmp-img/

# === 5. 渲染验证 ===
npm run dev
# 浏览器打开后逐页检查图片/视频/音频是否正确加载
```

### 7. 进阶：把 mmx 集成到 build 流程（可选）

> 如果你想让 npm 脚本**自动生成缺失素材**（避免手动跑命令），可以加一条 npm script：

```json
{
  "scripts": {
    "gen:assets": "node scripts/generate-assets.js",
    "predev": "node scripts/generate-assets.js --missing-only"
  }
}
```

```js
// scripts/generate-assets.js（**老师按需自建**，不是必装）
// 读取 slides.md 前置的"媒体素材清单"代码块（```yaml ... ```），
// 检查 public/ 下每个文件是否存在，缺失的自动跑对应的 mmx 命令。
// 简单实现：扫描 public/，对每个 [MMX] 描述调用 mmx image/music/speech/video。
```

> 这一步**不强制**。如果老师更愿意**手动控制**每张图的风格，**直接用第 6 节的手动流程**即可。

### 8. 当 mmx 不可用时的 fallback

> 如果**本机没有 `mmx`**，把清单表中的 `[MMX 图]` 全部**降级为** `[MANUAL 图]`，并按以下任一方式准备：

| 备选方式        | 适用              | 操作                                                                         |
| :-------------- | :---------------- | :--------------------------------------------------------------------------- |
| **AI 绘图工具** | 单张/少量素材     | 用 Midjourney / Stable Diffusion / DALL-E 跑清单表中的描述，下载到 `public/` |
| **库存图库**    | 通用插图          | 从 Unsplash / Pixabay 等免费图库下载（注意协议 / 署名要求）                  |
| **手绘**        | 海龟课 / 复杂动画 | 老师用 Procreate / Krita 画好后导出 PNG                                      |
| **录屏**        | 视频素材          | 用 OBS 录制 PyCharm / VS Code + Turtle 真实运行过程                          |
| **Freesound**   | 音效 / BGM        | 从 freesound.org 下载 CC0 协议音效                                           |

> **降级后的素材仍要遵守命名规范**（如 `kpt-print.png`），让 015/016 其它字段保持不变。

---

## 📚 三、课件骨架（与 015 脚本表一一对应）

> 下面是一份**完整、可直接运行**的 slides.md 骨架。
> **每一段都用 `---` 分隔**，**每一页都用 `layout:` 注明版式**。
> 写课时**只改**带 `[填]` 的字段，**结构不要动**。

---

## 📍 第 1 页 · A. 封面

```markdown
---
layout: cover
background: /cover.png          # [填] 封面图路径（public/ 下）
---

# 第 X 课：标题（知识点）

## 📘 知识讲解课 / 🐢 海龟绘图课

<div class="text-sm mt-8">
  所属章节：第 __ 章 第 __ 节<br>
  主讲老师：[填]<br>
  课时长度：120 分钟
</div>

<div class="abs-br m-6 text-xs opacity-50">
  本课知识点：0001、0002、0003 ……
</div>
```

<!-- 60s：本页用时 30 秒，翻页后开始 B 段 -->
```

---

## 📍 第 2~4 页 · B. 上节回顾（可选，第 1 课必须删除整段）

> ⚠️ **第 1 课**必须把 B 段（页 2~4）**全部删除**。从第 2 课起才出现。

### 第 2 页 · 速记挑战

```markdown
---
layout: default
---

# 🕰 上节回顾：还记得它们吗？

<v-clicks>

- 📢 `print()` = <span v-mark.red="3">**喊话器**</span>
- 🎒 **变量** = 背包（存数字 / 文字）
- 📝 **注释** = 便利贴（`#` 开头）
- ⌨️ `input()` = 对讲机（接收玩家的话）

</v-clicks>

<div v-click class="text-sm opacity-70 mt-8">
  💡 1 分钟速记挑战：在笔记本上写出 3 个以上。
</div>
```

<!-- 60s -->
```

### 第 3 页 · 上节代码热身

```markdown
---
layout: two-cols
---

# 🏃 跟着我敲一遍

```python {all|2,4|all}
# 示例 1：最小化——只输出字符串
print("Hello, World!")

# 示例 2：把钻石数量存到变量里
diamonds = 3
print(diamonds)
```

::right::

## 🎯 运行结果

```
Hello, World!
3
```

<div v-click class="text-sm mt-4">
  ✅ 答对了吗？<br>
  答错的同学举手，我们一起再敲一次。
</div>
```

<!-- 90s -->
```

### 第 4 页 · 上节提问回顾

```markdown
---
layout: default
---

# 🎯 选问回顾

> 请问如何定义一个变量来存储你的年龄，并输出它？

<v-click>

> ✅ 参考解答（折叠）：
> ```python
> age = 25
> print(age)
> ```

</v-click>
```

<!-- 60s -->
```

---

## 📍 第 5~15 页 · C. 章节引入 + 知识点

> 这是课件的**信息密度核心**，单页文字 ≤ 4 行，**多则拆页**。

### 第 5 页 · 模块章节页

```markdown
---
layout: section
---

# 📘 1.1 章节引入与知识点介绍

<div class="text-sm opacity-70 mt-4">
  Minetest 主线：[填]（如"魔法背包"）
</div>
```

<!-- 30s -->
```

### 第 6 页 · 背景介绍（Minetest 情境）

```markdown
---
layout: default
---

# 🌅 背景介绍

<v-clicks>

- 你出生在 Minetest 的大草原，身上只有 **3 个钻石**。
- 你按 `T` 键说话，但系统**听不懂中文**。
- 你必须学会 **Python 语言**，才能告诉系统你的财富！💎

</v-clicks>

<div v-click class="mt-8 text-sm">
  👉 钩子：你需要学会"喊话"。
</div>
```

<!-- 60s -->
```

### 第 7 页 · 知识点 1 比喻 + 定义

```markdown
---
layout: default
---

# 🔔 知识点 1：`print()`

> **<span v-mark.yellow="2">`print()` = 喊话器</span>**：可以让你在屏幕上显示任何你想说的话。

<v-click>

> **使用方法**：`print(内容)`
>
> **示例**：`print("Hello, World!")` 就是对着屏幕喊一句话。

</v-click>

<div v-click class="mt-6 text-xs opacity-60">
  💡 红石小技巧：`print` 是 Python 的**关键字**，不要写成 `pring` 或 `pritn`。
</div>
```

<!-- 90s -->
```

### 第 8 页 · 知识点 1 使用方法

```markdown
---
layout: default
---

# 🔧 最小化示例

```python {1|2|all}
# 这是一个注释，不会被执行
# 喊话器最简单的用法
print("Hello, World!")     # 在屏幕上显示 Hello, World!
```

<v-click>

运行后屏幕上会显示：

```
Hello, World!
```

</v-click>
```

<!-- 90s -->
```

### 第 9 页 · 知识点 1 贴近情境

```markdown
---
layout: two-cols
---

# 🎮 在 Minetest 里这样用

```python {1,3-4|2,5-7|all}
# Multi-Boy 的财富清单
diamonds = 3
gold = 100
print("我的钻石：", diamonds)
print("我的金币：", gold)
```

::right::

## 📺 运行结果

```
我的钻石： 3
我的金币： 100
```

<v-click>

<div class="text-sm mt-4">
  💬 学生问：为什么数字前有空格？<br>
  👉 答：`print` 自动用空格分隔多个参数。
</div>

</v-click>
```

<!-- 120s -->
```

### 第 10~11 页 · 知识点 2

> 复制第 7~9 页的模板结构，**替换比喻和代码**即可。举例省略。

---

## 📍 第 16~22 页 · E. Bug 猎人 + 代码修复家

> **报错必须真实可复现**，写之前先在本地跑过一遍！

### 第 16 页 · 模块章节页

```markdown
---
layout: section
---

# 🐛 1.2 Bug 猎人 + 代码修复家

<div class="text-sm opacity-70 mt-4">
  老师报错啦！同学们一起找虫虫 🐞
</div>
```

<!-- 30s -->
```

### 第 17 页 · 错误类型总结

```markdown
---
layout: default
---

# ⚠️ 本节常见错误类型

| 错误类型         | 触发场景                  | 怎么修                       |
| :--------------- | :------------------------ | :--------------------------- |
| `IndexError`     | 列表/字符串索引越界       | 检查 `len()` 是否 ≥ 索引+1   |
| `ValueError`     | `remove()` 删不存在的值   | 删之前先 `in` 判断           |
| `TypeError`      | 字符串和数字直接相加      | 用 `str()` 或 `f-string`     |
| `SyntaxError`    | 缺逗号、漏括号            | 数清楚 `(` `)` `[` `]` 配对  |
| `AttributeError` | 方法名拼错（如 `.apend`） | 查文档，列表方法是 `.append` |

<v-click>

<div class="text-sm mt-4 opacity-70">
  💡 本节 Bug 至少覆盖其中 3 种。
</div>

</v-click>
```

<!-- 60s -->
```

### 第 18 页 · Bug 1 示例

```markdown
---
layout: default
---

# 🐞 Bug 1：列表越界

```python {1|2|3,5|all}
backpack = ["钻石", "金锭", "红石"]
print("我有 3 件宝物")
print(backpack[3])          # 💥 越界！
print("任务完成")
```

<v-click>

运行后报错：

```
IndexError: list index out of range
```

</v-click>

<v-click>

### ✅ 你的修复

```python {1,4|all}
backpack = ["钻石", "金锭", "红石"]
print("我有", len(backpack), "件宝物")   # 用 len() 动态拿长度
print(backpack[2])                       # 取最后一个：索引是 len-1
print("任务完成")
```

</v-click>
```

<!-- 120s -->
```

### 第 19~21 页 · Bug 2~4

> 复制第 18 页结构，**替换代码和报错**，确保错误类型不重复。

### 第 22 页 · 代码修复家（综合）

```markdown
---
layout: default
---

# 🛠 代码修复家：综合挑战

> **目标**：修复下面这段代码，**运行后应输出** `战利品报告：3 件宝物`

```python {all|3,6|all}
# 有 3 个 Bug 的代码
lootbox = ["钻石", 金锭, "红石"]   # 🐞 Bug 1：字符串忘了引号
print("战利品报告：", lootbox[3], "件宝物")   # 🐞 Bug 2：越界
print("宝箱里还有：", lootbox.len())   # 🐞 Bug 3：list 没有 .len()
```

<v-click>

### ✅ 参考修复

```python {all|2,4,6|all}
lootbox = ["钻石", "金锭", "红石"]
print("战利品报告：", len(lootbox), "件宝物")
print("最后一个是：", lootbox[len(lootbox) - 1])
```

</v-click>

<div v-click class="text-sm mt-4">
  💡 修复完 3 个 Bug 后，对照"目标输出"逐字比对。
</div>
```

<!-- 300s -->
```

---

## 📍 第 23~30 页 · F. 创造与协作

> **任务一题一页**，避免一屏塞多题。**基础任务**给完整步骤，**进阶任务**给 2~3 个小目标。

### 第 23 页 · 模块章节页

```markdown
---
layout: section
---

# 🛠 1.3 创造与协作

<div class="text-sm opacity-70 mt-4">
  现在轮到 Multi-Boy 冒险者出手啦！
</div>
```

<!-- 30s -->
```

### 第 24 页 · 基础任务 1

```markdown
---
layout: default
---

# 📦 基础任务 1：战利品数量统计员

**任务描述**：

<v-clicks>

1. 创建一个变量 `backpack`，里面装 5 件宝物（用列表）。
2. 用 `print()` 打印宝物数量。
3. 用 `print()` 打印第一件和最后一件宝物。

</v-clicks>

<div v-click>

> ✅ 参考实现（折叠）：
> ```python
> backpack = ["钻石", "金锭", "红石", "绿宝石", "下界石英"]
> print("我有", len(backpack), "件宝物")
> print("第一件：", backpack[0])
> print("最后一件：", backpack[-1])
> ```

</div>

<div class="abs-br m-4 text-xs opacity-50">
  ⏱ 预计用时：5 分钟　|　完成率预期：≥ 80%
</div>
```

<!-- 300s -->
```

### 第 25 页 · 基础任务 2

> 复制第 24 页结构，**替换任务内容**。

### 第 26~27 页 · 进阶任务（挑战）

```markdown
---
layout: default
---

# 🏆 进阶任务 1（挑战）：战利品报告生成器

**任务描述**（**任选 2 项**完成即可）：

<v-clicks>

- [ ] 让玩家**输入自己的名字**，再打印 "Multi-Boy [名字] 的战利品报告"。
- [ ] 打印宝物数量后，**追加一句**"其中 3 件是稀有物品"（用变量存稀有数）。
- [ ] 用 `input()` 接收玩家输入的"想添加的宝物"，**追加到列表**再打印。

</v-clicks>

<div v-click>

> ✅ 参考实现（折叠）：
> ```python
> name = input("请输入你的名字：")
> backpack = ["钻石", "金锭", "红石"]
> rare = 2
> backpack.append(input("想添加什么？"))
> print("Multi-Boy", name, "的战利品报告")
> print("共", len(backpack), "件，其中", rare, "件稀有")
> ```

</div>

<div class="abs-br m-4 text-xs opacity-50">
  ⏱ 预计用时：10 分钟　|　完成率预期：50%
</div>
```

<!-- 600s -->
```

---

## 📍 第 31~36 页 · G. 复盘与固化

### 第 31 页 · 模块章节页

```markdown
---
layout: section
---

# 🔄 1.4 复盘与固化

<div class="text-sm opacity-70 mt-4">
  把今天的收获装进口袋 🎒
</div>
```

<!-- 30s -->
```

### 第 32 页 · 速记挑战

```markdown
---
layout: default
---

# ⏱ 速记挑战（1 分钟）

<v-clicks>

| 术语        | 比喻           | 5 秒写下它 |
| :---------- | :------------- | :--------- |
| `print()`   | 喊话器         | ____       |
| `len()`     | 物品计数器     | ____       |
| 列表 `list` | 魔法背包       | ____       |
| 索引 `[0]`  | 抽屉编号       | ____       |
| `append()`  | 把物品塞进背包 | ____       |

</v-clicks>

<div v-click class="text-sm mt-4">
  ⏱ 计时开始！5 个术语，看你能记几个？
</div>
```

<!-- 180s -->
```

### 第 33 页 · 我的冒险笔记

```markdown
---
layout: default
---

# 📓 我的冒险笔记

| 知识点    | 解释              | 例子                      |
| :-------- | :---------------- | :------------------------ |
| `print()` | 在屏幕上显示内容  | `print("Hi")`             |
| 变量      | 存数据的容器      | `diamonds = 3`            |
| 列表      | 多个数据的背包    | `loot = ["钻石", "金锭"]` |
| `len()`   | 统计列表长度      | `len(loot)` → `2`         |
| 索引      | 抽屉编号，从 0 起 | `loot[0]` → `"钻石"`      |

<div v-click class="text-sm mt-4 opacity-70">
  💡 口诀：**打印用 print，长度用 len，索引从 0 起，append 加新物品。**
</div>
```

<!-- 480s -->
```

### 第 34 页 · 易错点

```markdown
---
layout: default
---

# ⚠️ 易错点 / 红石小技巧

<v-clicks>

- ❌ `print(loot[3])` 当列表只有 3 件 → `IndexError`
- ✅ 改用 `print(loot[-1])` 取最后一件，永远不会越界
- ❌ 字符串忘了加引号 → `NameError`
- ✅ 字符串必须用 `"` 或 `'` 包起来
- ❌ `loot.len()` → `AttributeError`
- ✅ 列表长度是函数 `len(loot)`，**不是方法**

</v-clicks>
```

<!-- 60s -->
```

### 第 35 页 · 通关仪式（**必填，不留空！**）

```markdown
---
layout: default
---

# 🎉 通关仪式

🎉 **恭喜你！你已经成功掌握了 `print()`、变量、列表与 `len()` 的联合用法。**

你现在可以：

<v-clicks>

- ✅ 用 `print()` 在屏幕上喊出任何一句话
- ✅ 用变量存你的钻石数量
- ✅ 用列表装下所有战利品
- ✅ 用 `len()` 告诉村长"我有几件宝物"
- ✅ 用索引取出指定那件宝物

</v-clicks>

<div v-click class="mt-8">

> 带上你新学到的 **"魔法背包"**，继续在 Minetest 和编程的世界里冒险吧！
> 下次，我们将学习 **列表的截取与切片**。冒险家，下节课见！🚀

</div>
```

<!-- 180s -->
```

---

## 📍 第 36 页 · H. 结尾

```markdown
---
layout: end
background: /end-bg.png         # [填] 结尾图
---

# 谢谢观看 🙌

<div class="text-sm mt-4 opacity-70">
  下节课：第 X+1 课：标题（知识点）<br>
  投稿邮箱：[填]<br>
  课程主页：[填]
</div>

<div class="abs-br m-6 text-xs opacity-50">
  Multi-TRIX THON 教学手册 · 第 X 章 · 第 X 课
</div>
```

<!-- 30s -->
```

---

## 📚 四、Slidev 常用语法速查（写课时随手查）

> 下面这些是**写课件时一定会用**的语法片段，**遇到再回头查**比临时发明更高效。

### 1. 翻页与版式

```markdown
---
layout: default            # 默认版式
layout: cover              # 封面
layout: section            # 章节页
layout: two-cols           # 左右分栏
layout: end                # 结尾
layout: center             # 居中（适合单图 / 单行口号）
layout: image-right        # 右侧大图 + 左侧文字
---
```

### 2. 点击出现 / 高亮

```markdown
<v-clicks>            # 整块逐条出现
- 第一条
- 第二条
</v-clicks>

<div v-click>         # 单个元素点击出现
  重点强调
</div>

<span v-mark.red="3">第 3 步标红</span>
<span v-mark.circle="2">第 2 步画圈</span>
<span v-mark.underline="1">第 1 步加下划线</span>
```

### 3. 代码块行高亮

````markdown
```python {1|2-3|all}
print("a")
print("b")
print("c")
```
````

> - `{1}`：只高亮第 1 行
> - `{1,3-4}`：高亮第 1 行和第 3~4 行
> - `{1|2-3|all}`：**三段式**——先讲第 1 行 → 再讲 2~3 行 → 全部展开
> - `{all}`：全部高亮

### 4. 注释 / 备注 / 时间戳

```markdown
<!-- 这是一个普通 HTML 注释，不会在课件上显示 -->

<!--
_Notes:_
老师讲到这里可以慢一点，配合手势比划。
-->

<!-- 60s -->          <!-- 015 脚本表的"用时"标记，不渲染 -->
```

### 5. Multi-TRIX THON 项目自定义组件（推荐封装到 `components/`）

> 把角标、Bug 标记、Multi-Boy 头像等**重复使用的元素**封装成 Vue 组件，**避免每页重复写**。

```vue
<!-- components/MultiTrixBadge.vue -->
<template>
  <span class="mtrix-badge" :class="type">
    <slot />
  </span>
</template>

<style scoped>
.mtrix-badge { padding: 2px 8px; border-radius: 4px; font-size: 0.85em; }
.mtrix-badge.bug   { background: #C0392B; color: white; }
.mtrix-badge.tip   { background: #FFC107; color: black; }
.mtrix-badge.win   { background: #5BA84A; color: white; }
</style>
```

```markdown
<MTrixBadge type="bug">Bug 出现啦！</MTrixBadge>
<MTrixBadge type="tip">红石小技巧</MTrixBadge>
<MTrixBadge type="win">通关！</MTrixBadge>
```

### 6. 转场与动画

```markdown
---
transition: slide-left            # 整页用转场
layout: default
---

<div v-motion :initial="{ x: -50, opacity: 0 }" :enter="{ x: 0, opacity: 1 }">
  飞入的标题
</div>
```

---

## ✅ 五、自检清单（写完 slides.md 后逐项打勾）

### 元信息
- [ ] 顶部 frontmatter 已填主题、标题、作者、字号、比例。
- [ ] 章节路径 / 课次 / 课时类型 / 知识点编号均已标注。
- [ ] 配套 T-05 脚本 + T-02 教材草稿路径已留。

### 5 段完整性
- [ ] **A 封面** 1 页。
- [ ] **B 上节回顾** 1~3 页（**第 1 课必须整段删除**）。
- [ ] **C 章节引入** 8~12 页（含 Multi-TRIX百科 + 第一次运行）。
- [ ] **E Bug 猎人** 8~12 页，**至少 3 个 Bug + 3 种错误类型 + 1 个代码修复家**。
- [ ] **F 创造与协作** 10~15 页，**基础 ≥ 2 + 进阶 ≥ 2**。
- [ ] **G 复盘** 4~6 页，**速记 + 笔记 + 易错点 + 通关仪式**齐全。
- [ ] **H 结尾** 1 页。
- [ ] **总页数 40~60**。

### 教学硬性约定
- [ ] 比喻全部用**具体物品/角色**（喊话器、背包、便利贴），**无**"工具"、"概念"空泛词。
- [ ] 情境引入全部以 `Multi-Boy 冒险者` 或 `玩家` 为主语，**无**"我们"、"你们"。
- [ ] 5 段共享同一个 Minetest 主线（封面→结尾一以贯之）。
- [ ] 变量命名沿用（第一节叫 `backpack`，后面不突然换 `inventory`）。
- [ ] 提问页**有"参考解答"且用 `>` 引用块折叠**。
- [ ] **我的冒险笔记**是表格或口诀，**80~150 字内可抄完**。
- [ ] **通关仪式**已填 3~5 条 ✅，**不留空**，并呼应章节引入的主人公/事件。

### 代码与交互
- [ ] 所有代码块**带 `# 行内注释**。
- [ ] 代码示例**可直接运行**，**无**伪代码、`<xxx>` 占位符。
- [ ] 第一次运行遵循 3 步递进（最小化 → 贴近情境 → 联合使用）。
- [ ] Bug 报错**真实可复现**（写之前在本地跑过一遍）。
- [ ] 修复代码用 `v-click` 标出修复点。
- [ ] 翻页转场**全课统一**（默认 `slide-left`，不混用）。

### 跨页一致性
- [ ] 全课**只**用一种动画约定（`v-click` 为默认）。
- [ ] 全课**只**用一种强调色（红石红 #C0392B）。
- [ ] 全课**只**用一种 Multi-Boy 头像 / 章节插图风格。
- [ ] 学生独立操作环节（F 段）**累计用时 ≥ 25 分钟**。

### 媒体素材
- [ ] 媒体素材清单表**已逐行填写**（类型 / 文件名 / 所在页面 / 描述 / 方式 / 状态）。
- [ ] 每条素材**已选定方式**（`[MMX]` 或 `[MANUAL]`，**不能两种混着标同一条**）。
- [ ] 所有 `[MMX]` 素材**已跑 mmx 命令生成**，且 `ls public/` 能看到对应文件。
- [ ] 所有 `[MANUAL]` 素材**已通过 fallback 流程准备到位**（AI 绘图 / 图库 / 手绘 / 录屏 / Freesound）。
- [ ] 命名严格遵守"命名与目录规范"（`kpt-*.png` / `sfx-*.mp3` / `demo-*.mp4` 等）。
- [ ] slides.md 中**所有素材引用路径正确**（`/cover.png` 等，对应 `public/` 下文件）。
- [ ] `npm run dev` **已逐页验证**图片 / 视频 / 音频**正常加载、无 404**。
- [ ] 至少 1 个 BGM、1 个封面图、1 个结尾图；Bug 课**至少 1 个 sfx 音效**。

### 性能
- [ ] 单页代码块 ≤ 15 行（超出就拆页）。
- [ ] 单页文字 ≤ 4 行（超出就拆页或换 `two-cols`）。
- [ ] 图片 / 视频 / GIF 单个 ≤ 5 MB（避免课堂卡顿）。
- [ ] **总动画用时占比** ≤ 20%。

---

## 🔗 与其他模板的协作

```
T-01 知识点草稿（按"5 步"准备每个知识点）
   ↓
T-02 教材草稿（按 x.0~x.4 五模块整合成完整 2 小时课）
   ↓
T-05 Slidev课件设计 脚本安排设计（按页拆分课件，锁定每页的版式 / 动画 / 用时 / 旁白）
   ↓
T-06 Slidev课件设计 演示文件设计（你在这里 —— 写真正能被 Slidev 渲染的 slides.md）
   ↓
《第 X 课：标题（知识点）》正式 Slidev 课件
```

### 015 → 016 的字段对应关系（与 015 末尾表一致）

| 015 脚本表的字段 | 016 slides.md 的对应位置                         |
| :--------------- | :----------------------------------------------- |
| 页号 + 版式      | frontmatter 的 `layout:` 指令                    |
| 模块（A~H）      | `---` 分隔符分段                                 |
| 核心内容         | 页面主标题 + 正文 markdown                       |
| 关键元素         | 列表、表格、`<MTrixBadge/>` 等组件               |
| 动画             | `v-click` / `v-clicks` / `v-mark` / 行高亮 `{1,3 | all}` |
| 旁白             | 老师口播用，**不进** slides.md，**仅**015 自留   |
| 用时             | 016 的 HTML 注释 `<!-- 60s -->`                  |
| 备注             | 备品 / 互动形式 / 板书要求                       |
