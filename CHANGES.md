# YF-1 渐进式教学改造记录

基线：`7422556`（首访欢迎层）
备份：`index.bak.html`（动手前 `cp`，保留为未跟踪文件，未纳入提交）

硬约束执行情况：

- 只改 `index.html`，未引入外部依赖、构建步骤或后端。
- `modelOf`（功率平衡、τE、β、Q）及 `updateExpert`、`updateScan`、`bForMargin`、`pForMargin`、`pForQ`、`pForTemp` 与备份逐字一致。
- 每完成一个任务点单独提交，未 push。

## 1. 七步导览文案体检

### 锚点

- 文案表：`const TOUR_TITLES / TOUR_SAYS / TOUR_READS`（当前约 L2188-L2216）
- 分步渲染：`function renderTour()`（约 L2422）
- 未完成提示：`function tourMissing()`（约 L2241）
- 完成页：`function renderTourFinish()`（约 L2526）
- 对比实验结论：`function finishDemo()`（约 L2555）
- 名词解释：`#yf-gloss dl`（约 L688-L702）
- 首次可见的“任务”口径：`.yf-guide-goal`（约 L669）
- 进阶密度说明：`.yf-note`（约 L778）
- 事件日志“点火”：`logEvent("ignite", ...)`（约 L2809）

### 改了什么、为什么

| 位置 | 改动 | 为什么 |
| --- | --- | --- |
| 第 1 步“认识这台机器” | 只保留一个新概念“磁约束”；用“看不见的笼子”解释磁场，用“会发光的带电气体”复述等离子体；线圈、真空室只用大白话描述；“螺旋形”放到完成反馈里用“拧成麻花”解释 | 原文案同时抛出等离子体、螺旋磁场、真空室、线圈，零基础用户第一屏就超载 |
| 第 2 步“亲手点亮它” | 解释加热功率 = 每秒送进多少热、像灶台火力；补充“点亮 = 等离子体稳稳亮着”；解释单位 MW | 原步骤直接要求拖到 5–7 MW，却没有说明功率和“点亮”是什么 |
| 第 3 步“看懂三个数” | 拆成 3 张单概念卡，进度显示“第 3 / 7 步 · 第 1 / 3 张”；温度解释 keV 与 °C 换算；Q 用“开店回本”解释并补“聚变 = 轻原子核撞在一起放出的能量”；β 用“气球吹太鼓”解释并定义破裂 | 原来一步同时讲温度、Q、β，违反“一步最多一个新概念”；拆分后每个子卡只讲一个词 |
| 第 4 步“做个对比实验” | 明确“只加功率/只加磁场”两个对照；结论改成“离红线更近 / 热量漏得更慢”，避免“约束变好”这类行话；第一次出现 τE 时补“热量能被磁场关住多久，像保温杯能保温多久” | 对照实验的物理含义必须用现象描述，不能用术语解释术语 |
| 第 5 步“亲手翻一次车” | 开头先定义破裂：β 超 6.8% → 磁场兜不住 → 瞬间散掉、熄灭；破裂后文案再次点明“这就是破裂”；诊断区改成“会写清现象 → 原因 → 动作” | 破裂是高风险概念，必须先在触发前解释，再在触发后复述 |
| 第 6 步“自己达成 Q≥1” | 去掉裸写“B/P”，改成“磁场 B / 加热功率 P”；保留“先磁场、后功率”的顺序并加“先把锅沿加高，再开大火”类比 | Q、β 已解释，这一关不该再引入新名词，只训练操作顺序 |
| 第 7 步“往深里走” | 明确“每次只多一个控件或一个公式”；密度首次出现时解释“单位体积里装了多少燃料，像一锅汤里放多少料”；分解滑杆声明“只影响画面”；专家公式补 τE 解释 | 原来一步抛出密度、分解、公式；改为逐小步推进，并说明哪些只是视图、哪些是物理量 |
| 名词解释 `#yf-gloss` | 从 5 条扩到 15 条，并给每条加 `data-gloss-entry`：Q、β、τE、温度、磁场 B、加热功率 P、密度 n、等离子体、聚变、磁约束、点火/点亮、破裂、稳态、扭曲线圈、超导。所有类比与导览统一 | 保证“首次出现处”和 gloss 使用同一口径，也为读数提示提供唯一数据源 |
| `.yf-guide-goal` | 首次出现 Q、β、破裂时直接附生活化定义，不再等用户展开下面的“三个词” | 首屏任务卡是很多用户真正读的第一段 |
| `.yf-note`（进阶） | 密度 n 首次出现时补“一锅汤里放多少料” | 进阶入口也应先解释再让用户调 |
| 图例“超导” | 补“极低温下几乎不发热的导体” | 超导是图例里的第一个陌生材料词，至少要有一句大白话 |
| 事件日志“点火” | 改为“点火（加热到位，等离子体开始稳定亮着）” | “点火”首次出现在日志里，必须与 gloss 的“点亮”口径一致 |

## 2. 入门层渐进披露

### 锚点

- 入门层：`<section class="yf-level" data-level="1">`（约 L705）
- 折叠容器：`<details class="yf-more" id="yf-more-controls">`（约 L720）
- 样式：`.yf-more` 系列（约 L561-L572）
- 首次回填与记忆：`const moreControls = ...` 到 `function syncMoreControlsForTour()`（约 L1693-L1719）
- 导览接入：`renderTour()` 内 `syncMoreControlsForTour()`（约 L2425）；`renderTourFinish()` 内 `expandMoreControls(true)`（约 L2527）
- 三个核心读数副标签：`.yf-now` 三张卡（约 L874-L886）；动态文案 `updateNow()`（约 L3022）

### 改了什么、为什么

| 位置 | 改动 | 为什么 |
| --- | --- | --- |
| `.yf-level[data-level="1"]` | 两根滑杆仍是直接子内容；预设工况、诊断、三关任务整体移入 `<details id="yf-more-controls">`，默认无 `open` | 首屏先给两个可操作变量，降低选择过载；没有删除任何功能 |
| `details` 结构 | 原生 `<summary>`：`更多控件 · 预设工况 · 诊断 · 三关任务`；说明“点开后下次会自动展开” | 原生 details 自带键盘与展开语义，不引入自定义状态机 |
| localStorage | 键 `yf-more-controls-open`；手动展开时写入 `"1"`；刷新时回填 `open` | 用户主动点开过，就不应每次回来再收起来 |
| 导览联动 | 第 5 步破裂后需要诊断区时自动展开，但不立即写入；完成导览时 `expandMoreControls(true)` 展开并持久化 | 导览过程中不能让用户找不着诊断按钮；完成导览后才算“走完了”，此时应记住 |
| 三个核心读数 | 温度副标签“有多热：…”；Q 副标签“回本线 Q=1 · …”；β 副标签“离 6.8% 红线 · …”；破裂态改为“已熄灭 · …” | 让主数字下方始终有一行不依赖术语的解释 |
| 进阶/专家层 | 未改动 `.yf-level[data-level="2"/3]`、`setDepth()`、`data-depth` 或显隐逻辑 | 遵守“进阶/专家层与深度切换逻辑不受影响” |

## 3. 读数工具提示

### 锚点

- 读数卡：`.yf-now[data-gloss-key]`（约 L874-L886）
- “这是什么”按钮：`.yf-now-why`（同上）
- 提示样式：`.yf-gloss-tip` 系列（约 L309-L314）
- 提示元素：`#yf-gloss-tip[role="tooltip"]`（约 L904）
- 交互与读取 gloss：`const glossTip = ...` 到 `window.addEventListener("resize", ...)`（约 L1741-L1822）

### 改了什么、为什么

| 位置 | 改动 | 为什么 |
| --- | --- | --- |
| 三张核心读数卡 | 各加一个“这是什么”按钮，带 `aria-label` 和 `aria-describedby="yf-gloss-tip"` | 让零基础用户不用先知道“名词解释”在哪 |
| 悬停/聚焦 | 悬停读卡 180ms 后出现提示；键盘聚焦按钮立即出现；移出/失焦/Esc/滚动/缩放时收起 | 符合“hover/focus”的要求，同时避免提示挡视线 |
| 提示内容 | 直接读取 `#yf-gloss [data-gloss-entry="temp|q|beta"]` 的 `<dt>/<dd>` | 不复制第二份解释，确保与 gloss 一字不差；后续改 gloss 不会和提示漂移 |
| 点击按钮 | 复用 `setGloss(true)` 打开已有名词解释面板，并高亮对应条目；移动端同时上拉侧栏 | “优先复用已有 gloss 机制”，点击后仍有完整上下文 |
| 可访问性 | 提示用 `role="tooltip"`，按钮可键盘聚焦，Esc 可关闭 | 不把解释只做成鼠标彩蛋 |

## 4. 欢迎层对齐

### 锚点

- `#yf-welcome .yf-welcome-card`（约 L3184-L3197）
- 首访脚本：`<script>` 中的 `yf-welcome-done`（约 L3199-L3212，逻辑未改）

### 改了什么、为什么

| 位置 | 改动 | 为什么 |
| --- | --- | --- |
| 标题 | 从“你要做的事只有一个”改为“先玩两根滑杆，再看懂三个数” | 直接对应新的渐进披露顺序，不再一上来讲完整任务 |
| 第 1 段 | 解释仿星器、磁场（看不见的力）、等离子体（会发光的带电气体）、扭曲线圈 | 把首访最先出现的三个名词讲清 |
| 第 2 段 | 加热功率/磁场与两个滑杆对应；补 MW、T 单位；Q 用“聚变（太阳发光发热的那种反应）放出的能量 ÷ 投入的加热能量”解释，过 1 回本 | 与 gloss、导览第 2–3 步同一口径；首次出现的单位也有解释 |
| 第 3 段 | β 用“往外顶的劲儿 ÷ 磁场兜得住的劲儿”解释；6.8% 上限；破裂 = 瞬间散掉、熄灭；点明温度/Q/β 三个读数 | 保留任务感，但把所有后果先讲成现象 |
| 第 4 段 | 明确“入门层只直接摆两根滑杆和三个读数”；预设、诊断、三关在“更多控件”；走完导览自动展开并记住 | 让欢迎层与新默认布局、localStorage 行为一致 |
| 按钮副文案 | “零基础，跟着做就行”“随时点「开始分步导览」重新开始” | 与导览入口文案统一，不再依赖“左侧”这种布局假设 |

## 5. 自验修正

在四个任务提交之后，又补了一次一致性/鲁棒性修正（提交 `69d811c`）：

- 第 2 步补“点亮”的首次解释。
- 导览、gloss、对比实验结果统一补“τE 像保温杯能保温多久”。
- 欢迎层补 MW、T 的单位解释。
- `details` 的 `toggle` 在关闭时也复位自动展开标记，避免极端的“自动展开后立刻关闭”竞态。
- 提示按钮的 `closest` 加可选链，避免非 Element 事件目标报错。
- “这是什么”字号从 8.3px 调到 8.8px、颜色加深，保证可读性。

## 6. 自验结果

### 脚本语法

从 `index.html` 中提取全部 `<script>`（共 2 段：主 module + 欢迎层普通脚本），分别执行：

```text
node --check script1.mjs   -> 0
node --check script2.js    -> 0
```

两段均通过，无语法错误。

### 关键选择器/结构

用 BeautifulSoup 解析最终 HTML：

```text
.yf-welcome                  True
.yf-tour                     True
[data-depth]                 3
input[type=range]            4
[data-level]                 ['1', '2', '3']
#yf-more-controls 默认折叠   True
#yf-gloss [data-gloss-entry] 15
.yf-now [data-gloss-open]    3
```

另外确认：

- `.yf-presets`、`.yf-diagnosis`、`.yf-missions` 都在 `#yf-more-controls` 内。
- 入门层直接子级的 range 仍是 `yf-b`、`yf-p` 两根。
- 全页 ID 无重复。
- 与备份对比：未删除任何 ID；未删除任何预设、任务、带做按钮、深度按钮、层级、导览入口或 range；新增 ID 仅 `yf-more-controls`、`yf-gloss-tip`；新增按钮 3 个（三个“这是什么”）；新增 details 1 个。

### 物理代码不变

与 `index.bak.html` 逐字比对通过：

```text
modelOf       482 chars   unchanged
updateExpert 1130 chars   unchanged
updateScan   1738 chars   unchanged
bForMargin    206 chars   unchanged
pForMargin    200 chars   unchanged
pForQ         246 chars   unchanged
pForTemp      258 chars   unchanged
```

### 浏览器自动化说明

尝试用 Chrome headless 做交互回归时，当前沙箱拒绝 Chrome 的 crashpad/mojo 命名管道（`OpenProcess` / `platform_channel` 拒绝访问），浏览器无法启动；因此没有把浏览器自动化作为验收依据。已用静态 DOM 解析、源码比对和 `node --check` 覆盖本次改动的结构、语法与物理代码不变性。

## 7. 提交记录

```text
6b7b0d0  导览文案：逐步解释物理名词，统一 gloss 口径
cab09ef  入门层渐进披露：更多控件默认折叠并记忆展开
c5290fc  读数提示：三个核心读数支持悬停/聚焦查看大白话解释
eb52501  欢迎层文案：与新手指引及术语口径对齐
69d811c  自验修正：补齐点亮与 τE 口径，巩固展开记忆和提示边界
```

`index.bak.html` 为动手前备份，未加入 git；`CHANGES.md` 随最终提交加入。

---

# 第 2 轮：故事模式（2026-10-18）

基线：`b9ae07a`（欢迎层 + 七步导览完成态）
备份：`index.bak2.html`（动手前 `cp`，已用 SHA-256 与 `b9ae07a:index.html` 逐字节确认一致；保留为未跟踪文件）

## 8. 故事模式壳与四屏叙事

### 锚点

- 故事样式：`/* ---- story mode shell v1 ---- */`（约 L651-L800）
- 模式切换：`.yf-mode-switch`（约 L654；DOM 约 L815-L819）
- 故事容器：`#yf-story`（约 L820-L885）
- 四屏：`[data-story-panel="0..3"]`（约 L827-L871）
- 任务精简控件：`#yf-story-b` / `#yf-story-p` 与 `[data-story-read]`（约 L859-L867）
- 翻页点与箭头：`[data-story-dots]` / `[data-story-prev]` / `[data-story-next]`（约 L875-L884）
- 翻页逻辑：`function setStoryScreen()`（约 L3482）；滚轮约 L3613；方向键约 L3649
- 故事内容见 `CHANGES.md` 下方的文字量自验

### 改了什么、为什么

| 位置 | 改动 | 为什么 |
| --- | --- | --- |
| 首屏封面 | 标题「我们在地球上造一颗星星」；副句「岩超聚能 YF-1 仿星器 · 5屏带你亲手点亮它」；只留一个主按钮「开始 · 进入故事」 | 用一句主张 + 一个动作替代欢迎层的四段解释，先建立情绪与方向 |
| 第 2 屏难题 | 标题「上亿度的火，装不住」；副句「任何材料都装不住，唯一的容器，是磁场。」 | 只讲一个反直觉事实，把“为什么需要磁场”留成下一屏的悬念 |
| 第 3 屏机器 | 标题「看不见的笼子」；副句「六组扭曲线圈，把环形真空室里的等离子体兜在中间。」 | 用一个比喻承接上一屏，不引入新术语；六组与现有图注、导览口径一致 |
| 第 4 屏任务 | 标题「你来开火」；副句「磁场兜住，功率加热；把 Q 推过 1，别碰 β 红线。」；屏内两根滑杆 + 温度/Q/β 三读数 | 把“先读再玩”压缩成“边玩边懂”，任务目标在操作区内可见 |
| 翻页 | 滚轮、←→/↑↓/PageUp/PageDown/Home/End、左右箭头按钮、4 个可点击进度点 | 满足多输入方式；滚轮在可滚动任务屏内先滚内容，到边界才翻屏；760ms 冷却防连跳 |
| 一屏一念 | 每屏面板中只有 1 个内容按钮；标题/副文见下方硬断言 | 控制注意力，避免上一轮欢迎层式的多目标竞争 |

关于「5屏」：封面副句按需求保留原文案；实际结构是 **4 章故事 + 1 次进入模拟器亲手开火**，封面细字「4 章故事 + 1 次亲手开火」用于消歧，不新增第 5 个故事章节。

## 9. 视觉复用：iso 解剖图的运行时克隆

### 锚点

- 克隆函数：`function buildStoryVisual()`（约 L3364-L3430）
- 视觉容器：`[data-story-visual]`（L822）
- 屏级高亮 CSS：`[data-story-screen="cover|problem|machine|task"]`（约 L699-L712）
- 线圈分组点亮：`storyCoilGroups` / `startCoilSequence()`（约 L3348、L3408-L3436）

### 改了什么、为什么

| 位置 | 改动 | 为什么 |
| --- | --- | --- |
| 故事视觉 | 运行时 `cloneNode(true)` 现有 `.yf-stage svg`，不新增任何手绘 SVG | 严格复用现有 iso 解剖图；故事与模拟器始终同一张图 |
| 克隆清理 | 去掉 `is-exploded`/`show-labels`/`show-field`/`has-tour-parts`，清掉 `.it` 的 inline transform，重命名所有 `id` 并重写辉光 filter | 避免把模拟器当前的爆炸/标注/排序状态带进故事；避免重复 ID 与 `#yfBlur` 被隐藏原图抢引用 |
| 屏级动画 | 封面整体微漂移；难题屏只留等离子体并呼吸发光；机器屏六组线圈逐组点亮；任务屏压暗机器并保留等离子体 | 用同一张图做“背景/高亮”，每屏只强调一个对象 |
| 线圈逐组 | 按 `data-name` 的 `@0/@60/.../@300` 分成 6 组，`setTimeout` 依次加 `.is-story-coil-lit`；减少动态时直接全部点亮 | 对应“逐组点亮线圈”；尊重 `prefers-reduced-motion` |
| 任务屏视觉联动 | 复用 `modelOf` 结果更新 `--yf-plasma-op`、粗细与辉光；离开任务屏 `resetStoryVisual()` | 让读数与视觉同源，且返回难题屏时不会带着“破裂变暗”状态 |

## 10. 模式系统：记忆、深链与持久切换

### 锚点

- 首屏防闪脚本：`<script>` 中的 `var mode="story"`（约 L803-L813）
- 右上切换：`#yf-mode-switch`（L815-L819）
- 模式应用：`function applyMode()`（约 L3519-L3569）
- 记忆读取：`readStoredStoryMode()`（L3352）；hash 解析：`hashStoryMode()`（L3359）
- hashchange：约 L3668-L3672；启动：约 L3674-L3676
- 模拟器顶部让位：`html[data-yf-mode="sim"] .yf-main-scroll`（L672、L770）

### 改了什么、为什么

| 位置 | 改动 | 为什么 |
| --- | --- | --- |
| localStorage | 键 `yf-mode`，值 `story` / `sim`；用户点切换、深链进入或故事交棒时写入 | 记住用户选择；首访无值时不写，保持“首访默认故事” |
| URL hash | 支持 `#story` / `#sim`（兼容 `#/story`）；切换用 `history.replaceState` 写入，失败回退 `location.hash` | 可分享/可深链；不把每次模式切换塞满浏览器历史 |
| 首访/无 hash | `hash > 记忆 > story` 三级决议；无 hash 不主动补 hash | 严格满足“无 hash=遵循记忆/首访规则” |
| 右上切换 | 固定在 `z-index:300`，两种模式及七步导览期间都可见；故事/模拟器各自有按下态 | 用户随时可离开长叙事，符合“右上持久” |
| 模拟器布局 | 顶部给切换条预留 54px（含移动端安全区），桌面把原 `.yf-corner` 左移 128px | 避免持久切换条压住原有读数/导览入口 |
| 状态隔离 | 进入故事：`exitTour`、停 coach、清 idle、取消 rAF、`setGloss(false)`、隐藏 tooltip；模拟器 `inert`；故事面板 `inert` | 故事中不该有后台模拟器继续跑、演示或自动漂流；读屏也不会读到隐藏层 |

## 11. 交棒：末屏进入模拟器并自动启动七步导览

### 锚点

- 末屏主按钮：`[data-story-handoff]`（L869）
- 交棒函数：`function storyHandoff()`（约 L3572-L3580）
- 现有导览入口：`[data-tour-start]`（L902）
- 现有导览：`function startTour()`（约 L2870）

### 改了什么、为什么

| 位置 | 改动 | 为什么 |
| --- | --- | --- |
| 末屏按钮 | 「进入模拟器 · 跟着导览点亮」，全屏章节内唯一主按钮 | 把“读完故事”自然变成“动手” |
| 交棒 | `applyMode("sim")` 后 `setTimeout(function(){ document.querySelector("[data-tour-start]").click(); }, 60)` | 沿用旧欢迎层已验证的 `setTimeout` 衔接；不用 rAF（后台标签页不可靠） |
| 竞态 | 60ms 回调前检查 `activeMode === "sim"`，并用 `storyHandoffTimer` 防连点 | 用户若在 60ms 内切回故事，不再偷偷启动导览 |
| 导览 | 未改 `startTour()` / 七步文案 / 完成态 | 复用现有教学流程，不引入第二套导览 |

## 12. 旧欢迎层退役

### 锚点

- 删除前：`index.bak2.html` 尾部 `.yf-welcome` 样式块 + `#yf-welcome` DOM + `yf-welcome-done` 脚本
- 删除后：`rg "yf-welcome" index.html` 为 0；DOM/CSS/JS 均已移除

### 改了什么、为什么

| 位置 | 改动 | 为什么 |
| --- | --- | --- |
| `.yf-welcome` 全块 | 删除样式、对话框 DOM、首访脚本 | 故事首屏接管欢迎职责；两套首访层会互相遮挡 |
| `yf-welcome-done` | 删除写入/读取逻辑 | 旧键退役；新模式使用 `yf-mode`，不混用两套状态 |
| 可回滚性 | 全部删除只在 git 工作树中；`b9ae07a` 可完整找回 | 满足“git 历史可回滚”，不在文件里留死代码 |

## 13. 移动端与可访问性

### 锚点

- 移动媒体查询：`@media (max-width:760px)`（约 L767-L790）
- 安全区：`env(safe-area-inset-top/bottom)`（L768-L770、L779、L786）
- 触控目标：`.yf-story-arrow`（42px，粗指针 44px）、`.yf-story-dots button`（24×30）、`.yf-story-cta`（44px）
- 焦点管理：`applyMode()` 中 `focusWasInPage` + 标题 focus；`setStoryScreen({focus:true})`
- 语义：`aria-current="step"`、`aria-pressed`、`aria-live` 屏间播报、`aria-live` 任务目标

### 改了什么、为什么

| 位置 | 改动 | 为什么 |
| --- | --- | --- |
| 窄屏面板 | 82px 顶部（含安全区）、20px 左右、底部安全区 + 60px；面板内部滚动 | 380px 以下标题换行、副文可读；任务屏内容可滚到按钮 |
| 控件 | 两根滑杆单列堆叠，range 高度 26px；三读数 2 列、β 读数整行 | 避免窄屏横向挤压与滑杆难点按 |
| 导航 | 底部安全区；箭头 42/44px；进度点扩大命中区并保留小圆点视觉 | 拇指可点，不破坏“点”的视觉 |
| 模式切换 | 顶部安全区让位；模拟器主内容顶部预留 54px + 安全区 | 持久切换条在刘海屏也不压内容 |
| 语义/焦点 | 屏切换标题聚焦；非活动屏 `inert` + `aria-hidden`；故事/模拟器互斥 `inert`；目标与屏播报 live | 键盘与读屏用户不会落到屏外按钮 |
| 动效 | `prefers-reduced-motion` 下关闭漂移/呼吸/滑动过渡；线圈直接全亮 | 降低眩晕与等待 |

## 14. 自验结果

### 14.1 脚本语法

从最终文件抽取全部 `<script>`（2 段：防闪 classic + 主 module），逐一执行：

```text
script1.js  328 chars   node --check -> 0
script2.mjs 118619 chars node --check -> 0
```

### 14.2 关键结构（BeautifulSoup，最终文件）

```text
.yf-tour                 1
[data-depth]             3
input[type=range]        6   （模拟器 4 + 故事 2）
[data-level]             ['1','2','3']
#yf-more-controls        True（无 open，默认折叠）
.yf-welcome              0
.yf-story-panel          4（每个面板 1 个 .yf-story-cta、1 个 button）
#yf-story / #yf-mode-switch 均存在
```

### 14.3 新增：每屏文字量硬断言

按可见 title / sub 文本逐字计数（含标点、空格）：

```text
屏 1  标题 11 字「我们在地球上造一颗星星」  副文 25 字「岩超聚能 YF-1 仿星器 · 5屏带你亲手点亮它」  CTA 1
屏 2  标题  9 字「上亿度的火，装不住」      副文 19 字「任何材料都装不住，唯一的容器，是磁场。」      CTA 1
屏 3  标题  6 字「看不见的笼子」            副文 24 字「六组扭曲线圈，把环形真空室里的等离子体兜在中间。」 CTA 1
屏 4  标题  4 字「你来开火」                副文 27 字「磁场兜住，功率加热；把 Q 推过 1，别碰 β 红线。」 CTA 1
```

四屏标题均 ≤20、副文均 ≤60；每个 `.yf-story-panel` 内恰好 1 个 `.yf-story-cta`。

### 14.4 hash 路由 / 记忆矩阵（JSDOM）

```text
无 hash + 无记忆             -> story（首访默认）
无 hash + 记忆 sim           -> sim
#story + 记忆 sim            -> story
#sim + 记忆 story            -> sim
#foo + 记忆 sim              -> sim（非法 hash 回落记忆）
#/sim + 无记忆               -> sim（兼容 #/ 前缀）

点击「模拟器」                 -> data-yf-mode=sim, hash=#sim, localStorage=sim
点击「故事」                   -> data-yf-mode=story, hash=#story, localStorage=story
hash 清空                      -> 回落到记忆中模式
hash 改为 #story               -> story 且记忆更新为 story
```

JSDOM 同时验证：

```text
滚轮 -> 第 2 屏；ArrowRight -> 第 3 屏；进度点 -> 第 4 屏
机器屏 800ms 时第一组线圈已亮（共 6 组，逐组进行）
故事任务：B 3.0 / P 12 -> Q 1.88、目标转绿；B 2.0 / P 12 -> 温度“已熄灭”、β“已归零”、目标转红
末屏 CTA -> sim + #sim + localStorage=sim，且 #yf-tour 显示、body.yf-tour-on、进度“第 1 / 7 步”
导览中点「故事」-> 导览退出；再点「模拟器」-> 不会自动重启导览
模拟器启动时故事 SVG 克隆不存在；首次切故事时才创建；克隆后全页无重复 id（39 个 id 唯一）
控制台/脚本错误：0
```

### 14.5 物理计算逻辑不变

与 `index.bak2.html` 逐字提取比对（brace-aware）：

```text
modelOf       480 chars  unchanged=True
updateExpert 1129 chars  unchanged=True
updateScan   1737 chars  unchanged=True
bForMargin    205 chars  unchanged=True
pForMargin    199 chars  unchanged=True
pForQ         245 chars  unchanged=True
pForTemp      257 chars  unchanged=True
updateNow     945 chars  unchanged=True
diagnosisOf  4780 chars  unchanged=True
tick         1369 chars  unchanged=True
spring        604 chars  unchanged=True
readoutOf      24 chars  unchanged=True
ALL_PHYSICS_UNCHANGED True
```

本轮只在 `draw()` 末尾增加 `updateStoryReadouts(m)` 调用，并在 `kick()` / 稳态 interval / 初始 idle 增加模式守卫；没有改任何公式、参数或模型返回值。

### 14.6 单文件、零依赖、旧层清理

```text
<link>            0
<script src>      0
@import           0
url(http...)      0
src="http..."     0
外部 http refs    仅 SVG 命名空间 http://www.w3.org/2000/svg
yf-welcome 引用   0
```

`index.bak2.html` 与 `b9ae07a:index.html` SHA-256 完全一致（备份有效，未纳入提交）。

### 14.7 浏览器自动化说明

本轮再次尝试 Chrome / Edge headless；仍被沙箱拒绝 crashpad 的 `OpenProcess` 与 mojo `platform_channel` 命名管道，浏览器无法启动（与上一轮相同）。因此移动端结论来自响应式 CSS 静态核对（断点、安全区、触控尺寸、堆叠规则），交互与路由结论来自 JSDOM 真实 DOM 事件回归，不把 headless 截图作为验收依据。

## 15. 提交记录（本轮）

```text
2354a60  故事模式壳：四屏叙事、iso 图复用与欢迎层退役
6c5248c  模式系统：故事/模拟器切换、记忆与 #story/#sim 深链
a863189  故事交棒：末屏进入模拟器并用 setTimeout 自动启动七步导览
e44a267  故事模式移动端适配：安全区、触控目标与焦点/视觉收尾
e3174df  自验修正：故事读数、交棒竞态与视觉状态收尾
```

本轮 `CHANGES.md`（含锚点与自验记录）随紧随其后的文档提交加入；代码提交 `e3174df` 已包含最终自验结论对应的代码。`index.bak2.html` 为本轮动手前备份，未加入 git；`index.bak.html` 为上轮备份，保留。均不 push。
