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

---

# 第 3 轮：分拍叙事 + 图上就近标注 + 任务屏联动 + 浮动导览卡（2026-10-19）

基线：`3c3f393`（故事模式四屏、七步导览、入门层渐进披露完成态）
备份：`index.bak3.html`（动手前 `cp`，未加入 git；`.gitignore` 的 `index.bak*.html` 已覆盖）

## 16. 故事 2–4 屏改为分拍叙事

### 锚点

- 分拍 DOM：`[data-story-panel="1..3"] [data-story-beat]`、`[data-beat-body]`、`[data-story-beat-next]`
- 分拍样式：`.yf-story-beats` / `.yf-story-beat` / `.yf-story-body` / `.yf-story-beat-nav`（新增 CSS 块约 L803–L865）
- 拍状态机：`storyPanelBeats()` / `activeStoryBeat()` / `setStoryBeat()` / `storyNext()` / `storyPrev()`（约 L3604–L4180）
- 屏级入口：`setStoryScreen()`（约 L4118）；滚轮约 L4302；键盘约 L4322
- 视觉高亮：`.yf-story-visual .it.is-story-beat`（约 L821）；线圈序列 `lightNextCoilGroup()`（约 L3942）

### 拍拆分与文字量

| 屏 | 拍 | 拍标题 | 正文（≤25 字） | 图上被讲对象 |
| --- | --- | --- | --- | --- |
| 2 难题 | 1 / 2 | 装不住的火 | 上亿度，一碰就化（8） | 等离子体弧 |
| 2 难题 | 2 / 2 | 磁场当容器 | 唯一能兜住（5） | 六组扭曲线圈 |
| 3 机器 | 1 / 3 | 环形真空室 | 把火悬在中间（6） | 真空室 |
| 3 机器 | 2 / 3 | 扭曲线圈 | 六组拧螺旋（5） | 扭曲线圈 |
| 3 机器 | 3 / 3 | 橙色等离子体 | 带电会发光（5） | 等离子体弧 |
| 4 任务 | 1 / 2 | 先抬磁场 | 撑开红线（4） | 磁场 B 滑杆 |
| 4 任务 | 2 / 2 | 再加功率 | 推 Q 过 1（4） | 加热 P 滑杆 |

- 旧版 2–4 屏副文（去空白）：19 + 24 + 22 = **65 字**；新版 7 拍正文合计 **37 字**，减少 **43.1%**（要求 ≥40%）。
- 左栏只保留拍标题、≤25 字单行正文、拍进度与动作；原先的整段解释不再出现。
- 每拍只高亮一个对象集合：等离子体弧 → 线圈 → 真空室 → 线圈 → 等离子体弧 → B 滑杆 → P 滑杆；相邻拍的高亮集合均发生变化。
- 线圈逐组点亮只在“扭曲线圈”拍启动；切到真空室/等离子体拍会 `clearCoilSequence()`，避免旧动画抢焦点。

### 翻拍交互

| 位置 | 改动 | 为什么 |
| --- | --- | --- |
| “下一拍 →”按钮 | 每个多拍屏在拍标题下新增 `[data-story-beat-next]`；最后一拍隐藏，末拍 CTA 出现 | 点击即可翻拍，不依赖找底部箭头 |
| 底部箭头 | 有下一拍时先翻拍；最后一拍才翻屏。上一箭头反向：先上一拍，再上一屏的最后一拍 | 保持单一翻页入口，但先拍后屏 |
| 滚轮 | 沿用“先滚动面板、到边界再翻”的规则；边界时先翻拍 | 不破坏长内容滚动，也符合“滚轮翻拍” |
| 键盘 | ←→/↑↓/PageUp/PageDown 逐拍；Home/End 跳屏并回到第 1 拍 | 与原有键盘路径兼容 |
| 进度点 | 点任意屏重置该屏第 1 拍 | 避免从第 2 拍进入新屏的错位 |
| 可访问性 | 非当前拍 `hidden + inert`；`aria-live` 播报“第 X 屏，拍 Y / N”；翻拍后聚焦当前拍标题 | 读屏与键盘不会落到隐藏拍 |

## 17. 图上就近标注：标注点 + 引导线 + 短语标签

### 锚点

- 标注层：`<div class="yf-story-notes" data-story-notes aria-hidden="true">`（约 L882）
- 动态元素：`ensureStoryNote()` 创建 `.yf-story-leader`（`<path>` + `<circle>`）与 `.yf-story-note`
- 定位：`storyNoteAnchor()` / `positionStoryNote()` / `scheduleStoryNote()`（约 L3604–L3740）
- 样式：`.yf-story-note` / `.yf-story-leader` / `.is-corner`（约 L825–L840）

### 改了什么、为什么

| 位置 | 改动 | 为什么 |
| --- | --- | --- |
| 标注点 | 在代表部件 `getBoundingClientRect()` 的边缘画 3.4px 圆点，带 2.4s 呼吸 | 让“讲的是哪一个部件”有明确落点 |
| 引导线 | 全屏 SVG 三层折线（水平—垂直—水平 / 垂直—水平—垂直），虚线 26px 间距 | 标签与部件之间形成可追踪的视觉链路 |
| 短语标签 | 从 `data-beat-label` 读取，7 个标签为 等离子体/扭曲线圈/真空室/扭曲线圈/等离子体/磁场 B/加热 P，最长 4 字（要求 ≤8） | 图上直接给词，不要求用户回左栏找 |
| 代表部件选择 | 多部件目标（线圈/真空室）按“离视觉焦点近 + 面积惩罚 + 是否在视口”挑一个代表；可用 `data-beat-anchor` 收窄 | 六组线圈有 102 个 `.it`，不能把标签放在包围盒外 |
| 避让 | 标签避开视口四边、左栏 copy 区、其他滑杆/读数卡；冲突时尝试左/右/下/上 | 不遮挡被讲控件，避免本轮反馈中的“空间距离远/互相覆盖” |
| 窄屏 ≤760px | 标签退化为右上角标：`right:12px`、`top:max(48px, safe-area + 42px)`，隐藏引导线与圆点 | 窄屏没有横向空间；角标固定、单条不重叠 |
| 跟随 | 屏切换、visual transform `transitionend`、窗口 resize、面板滚动时重排 | 故事图有 1.1s 位移过渡；任务屏内容可滚动，标注必须跟着走 |

## 18. 任务屏：滑杆贴读数 + 影响连线 + 脉冲

### 锚点

- 任务屏精简控件：`[data-story-panel="3"] .yf-story-play`（约 L935–L967）
- 读数：`[data-story-read="temp|q|beta"]`；滑杆：`#yf-story-b` / `#yf-story-p`
- 连线层：`[data-story-links]` + `drawStoryLinks()`（约 L3742–L3810）
- 通道状态：`pulseStoryChannel()` / `endStoryChannelSoon()`（约 L3812–L3836）
- 样式：`.yf-story-links` / `.yf-story-link` / `.is-impact`（约 L841–L849）

### 改了什么、为什么

| 位置 | 改动 | 为什么 |
| --- | --- | --- |
| 布局顺序 | 三张读数卡在上，滑杆紧接其下 16px；B/P 两杆并排放在 560px 控件区内 | 读数与滑杆同屏、同区，打破“讲解在左、控制在远” |
| 磁场连线 | B 滑杆拇指 → 温度卡底边；B 滑杆拇指 → β 卡底边 | 对应“磁场杆 → β/温度” |
| 功率连线 | P 滑杆拇指 → Q 卡底边；P 滑杆拇指 → 温度卡底边 | 对应“功率杆 → Q/温度” |
| 拖动高亮 | `input`/`pointerdown` 设 `data-story-channel="b|p"`，点亮同色连线、压暗另一通道；`pointerup` 后 380ms 清除 | 让“正在动哪根杆、影响哪些数”即时可见 |
| 读数脉冲 | 受影响读数加 `.is-impact`，0.5s × 2 的 box-shadow 脉冲；B 为琥珀、P 为青绿 | 读数不只是数字变化，也有注意提示 |
| 初始/换拍 | B 拍预点亮 B 通道、P 拍预点亮 P 通道；换拍/换屏清理活动通道与脉冲 | 讲解与控件状态同步，不残留上一拍 |
| 可访问性 | `aria-label` 保留完整量程说明；连线 SVG `aria-hidden`；读数变化同时写入 `aria-live` 目标行 | 视觉连线不替代读屏信息 |

## 19. 七步导览卡：从固定角落改为浮动贴靠

### 锚点

- 卡片：`#yf-tour`（DOM 约 L1220）；样式 `.yf-tour.is-floating`（约 L851–L861）
- 定位：`resetTourCardPosition()` / `tourCardAnchor()` / `positionTourCard()` / `scheduleTourPosition()`（约 L2749–L2862）
- 目标渲染：`renderTour()` 中 `applyTourTargets()` / `tourFocusEl` / `scheduleTourPosition()`（约 L2981）
- 滚动避让：`tourScroll()`（约 L2685）；滚动监听约 L2865

### 改了什么、为什么

| 位置 | 改动 | 为什么 |
| --- | --- | --- |
| 桌面 ≥1100px | 卡片 `position:fixed`，由 JS 选左/右/上/下贴靠当前 focus/target；不再固定右下角 | 讲解词与被讲控件同视野 |
| 安全区 | 左界 = 侧栏右缘 + 12px；上界 64px（让出模式切换）；右/下各 12px；卡片宽 348、最大高 66vh | 智能避让侧栏与视口边缘 |
| 目标选择 | 优先 focus 元素；若 focus 是整个 stage/nowbar 等超大元素，则在 `.is-tour-target` 中选离主视区焦点近、面积较小的代表 | 第 1 步等目标是一整张图时，仍能贴到具体线圈 |
| 防遮挡 | 候选位置打分：覆盖当前 target 罚 1000，越界/位移越小越优；目标在侧栏时卡片落在主区 | 既不挡控件，也不压侧栏 |
| 跟随 | 主区/侧栏 scroll、resize、每步渲染后 rAF + 120/520ms 二次定位；`tourScroll` 只在卡片与 target 横向重叠时才限制纵向滚动 | 滚动后卡片仍跟着；浮动卡不再把 target 强行顶到卡片上方 |
| 窄屏 | ≤1099px 保持原有底部导览卡/抽屉逻辑；JS 不添加 `is-floating` | 手机上浮动卡无横向空间，保留可用性 |
| 完成/退出 | `renderTourFinish()` / `exitTour()` 清空 `tourFocusEl`、复位 floating 样式与 `body.yf-tour-floating` | 不把上一轮的定位状态带到下一次 |

## 20. 自验

### 20.1 脚本语法

最终文件共 2 段真正执行的 `<script>`（防闪 classic 328 字符 + 主 module 143312 字符；第三处 `<script` 出现在导出用的模板字符串里，不是标签）：

```text
node --check script1.js  -> 0
node --check script2.js  -> 0
```

### 20.2 关键结构

```text
.yf-story                      1
data-story-screen             存在（4 屏）
[data-depth]                  3
input[type=range]             6（模拟器 4 + 故事 2）
#yf-more-controls             1（默认折叠）
.yf-tour                      1
data-story-beat               7
data-story-notes / links      存在
静态重复 id                   0
```

### 20.3 新增文本/几何/联动断言（host 侧 dsh-verify + Playwright）

浏览器验收：`dsh-verify` spec 20/20 通过；页内 harness 79/79 项通过。断言包括：

```text
每拍正文去空白长度        8,5,6,5,5,4,4；均 ≤25；合计 37（旧 65，-43.1%）
7 个标签长度              等离子体4 / 扭曲线圈4 / 真空室3 / 扭曲线圈4 / 等离子体4 / 磁场 B4 / 加热 P4；均 ≤8
7 拍标注                  note 与高亮部件均在同一 iframe 视口；标签边距 26px，<300px
6 次拍推进                高亮部件签名全部变化（弧→线圈→真空室→线圈→弧→B→P）
B 输入                    温度/β 文本变化；temp+beta 获 .is-impact；B 通道 2 条连线有 d
P 输入                    Q/温度文本变化；q+temp 获 .is-impact；P 通道 2 条连线有 d
拖杆 120ms                 .is-impact 仍存在；data-story-channel=b/p
导览第 1/2 步             卡片 is-floating=true；left=392 ≥ sidebar.right+12=392；不覆盖 P 控件；P 控件在视口内
≤760px                    标签 is-corner=true；无引导线；与右上模式切换不重叠；在视口内
页面异常/控制台错误       0
```

说明：shell 直接拉起 Chrome 仍会被 crashpad/命名管道拒绝；本轮真实浏览器验收改由 host 侧 `dsh-verify` 执行（Chromium/Playwright），因此不再沿用上一轮“浏览器无法启动”的结论。截图保存在未跟踪的 `dsh-verify-out/`：

```text
story-beats/s2-beat1-plasma.png、s2-beat2-coils.png
story-beats/s3-beat1-vessel.png、s3-beat2-coils.png、s3-beat3-plasma.png
story-beats/s4-beat1-b.png、s4-beat2-p.png
harness-tour/tour-step2.png
harness-narrow/narrow-s2.png
```

### 20.4 物理计算逻辑不变

动手前后用 brace-aware 提取并对以下函数做 SHA-1，逐项一致（12/12）：

```text
modelOf       85b56267569ff905939484c176c6ab3b4e32d7c3
updateExpert  bff1976a949aae3389431fe26e14c1ce10093606
updateScan    9dcd187f4388896d4f4c5f06887b8abf434db9a8
bForMargin    2a339b8147bae6334a48952fdac243d3e4e8fc18
pForMargin    498f687502500478584b117f29e6fc174793a2a2
pForQ         6dacd1468960a7e1fb3b6596b3602d1538478dde
pForTemp      104f73d85fac51227f956d82beddb043563f4a42
updateNow     4c11a3df6a334908008a5be90689896937cfee95
diagnosisOf   637ad8abfe946126cff0db2f70b1e9441aa8ead8
tick          0a991b6b83067423aa32f38c73dc466361568479
spring        38a19e490d0c944cb9926971ec6aee0708d46d1b
readoutOf     21b5964194330972eb06622d887b8f5122628285
```

### 20.5 单文件、零依赖

```text
<script src>      0
<link>            0
@import           0
http 引用         仅 SVG 命名空间 http://www.w3.org/2000/svg
新增依赖          无
```

## 21. 本轮提交

单提交（不 push）：`分拍叙事 + 图上标注 + 任务屏联动 + 浮动导览卡`

`index.bak3.html` 为动手前备份，保留为未跟踪文件；`CHANGES.md` 与本轮最终 `index.html` 一起提交。

## 22. Lead 验收修复：末拍死路与翻屏拍态残留

备份：`index.bak4.html`（动手前 `cp`，未跟踪）。本轮只改 `index.html` 的拍导航锚点与一条 CSS 规则，未改 HTML 结构、未新增依赖；上一节 §16 的“最后一拍隐藏下一拍按钮”由本节明确取代。

### 22.1 复现与根因

| Bug | 真实浏览器复现 | 根因 |
| --- | --- | --- |
| BUG1 末拍死路 | 清 localStorage 全新加载 → 封面 CTA 进第 2 屏 → 点“下一拍”到拍 2/2 → 再点“下一拍”，`page.click` 超时，日志显示按钮带 `hidden`；屏幕上只有底部箭头能离开 | `setStoryBeat()` 在最后一拍执行 `nextBeat.hidden = lastBeat`，把“下一拍”按钮设为 `display:none`。末拍根本点不到该按钮，`storyNext()` 的翻屏分支永远不会被它触发；CTA 与底部箭头成了唯二出口 |
| BUG2 翻屏拍态残留 + 双按钮 | 第 2 屏拍 2/2 时点底部箭头进入第 3 屏；全文档 `[data-story-beat-kicker]` 首个匹配仍是 `02 / 04 · 难题 · 拍 2 / 2`，`[data-beat-body]` 仍是旧正文；同时有 2 个 `.yf-story-beat-next` 的 `offsetParent` 非空 | `setStoryScreen()` 只切换 `aria-hidden` / `inert` 与 track transform，三块 panel 仍然参与布局。非当前屏的拍导航按钮没有 `hidden`，`offsetParent` 仍非空（第 3 屏进入后是第 3、4 屏两个按钮）；`data-story-beat-kicker` / `data-beat-body` 又写在每个 panel/beat 上，全文档查询自然先命中第 2 屏旧节点 |

### 22.2 锚点与最小改动

`git diff --stat`：`index.html` 23 insertions(+), 4 deletions(-)，无标记结构改动。

| 位置 | 改动 | 为什么 |
| --- | --- | --- |
| `.yf-story-beat-nav[hidden]`（L814） | 在既有 `[hidden]` 规则里补上拍导航容器 | 作者样式 `.yf-story-beat-nav{display:flex}` 会覆盖 UA 的 `[hidden]`；补 `!important` 后隐藏整个进度 + 按钮行 |
| `setStoryBeat()`（L4024） | 末拍不再隐藏 `[data-story-beat-next]`；文案按状态改为“下一拍 → / 下一屏 → / 进入模拟器 →”；`data-beat-body` 标记只落在当前拍正文 | 末拍按钮必须可点且必须走 `storyNext()` 的翻屏/交棒分支；当前拍正文标记跟随拍索引，全文档查询不再命中旧拍 |
| `setStoryScreen()`（L4106） | 逐 panel 门控：仅 active panel 保留 `data-story-beat-kicker`、显示 `.yf-story-beat-nav`、取消按钮 `hidden`；非 active panel 的拍导航、拍按钮与 `[data-story-after-beat]` CTA 隐藏；翻屏仍传 `beat: 0` 或 `"last"` | 用 `data-story-screen` 对应的当前屏做唯一可见源；进入新屏固定第 1 拍，只有“上一屏”反向导航才落到上一屏最后一拍 |
| `storyNext()`（L4226） | 第 4 屏末拍落到函数末尾时调用 `storyHandoff()` | 末屏末拍的“下一拍”与 CTA 走同一条交棒路径，不再静默无效 |

没有新增 DOM、没有新增全局状态、没有改 `storyHandoff()` / `applyMode()` 的交棒时序。

### 22.3 自验断言（真实浏览器，`file:///C:/Users/92993/yf1-stellarator-sim/`）

`dsh_verify` 结果（均为 Chromium/Playwright，真实鼠标点击；页面无 console / network error）：

| spec | 结果 | 覆盖 |
| --- | --- | --- |
| `specs/story-nav-real.json` | PASS 46/46 | 逐屏点“下一拍”：第 2 屏末拍 → 第 3 屏，第 3 屏末拍 → 第 4 屏，第 4 屏末拍 → 模拟器交棒；全局 kicker / body；截图 7 张 |
| `specs/story-nav-bottom-arrow-file.json` | PASS 28/28 | 直接复现 BUG2 的底部箭头路径：第 2 屏拍 2/2 → 第 3 屏拍 1/3；旧屏按钮 `[hidden]`、新屏按钮 `:not([hidden])`、第 4 屏按钮 `[hidden]`；第 3 屏末拍再经底部箭头 → 第 4 屏 |
| `specs/story-nav-assert-file.json` | PASS 31/31 | 内嵌断言 harness：`PASS screens=4 lastBeats=3/3 buttons=116 kickers=3 keys=0` |
| `specs/story-nav-key-file.json` | PASS 24/24 | 方向键：第 2 屏末拍 ArrowRight → 第 3 屏；第 4 屏拍 1 ArrowRight → 拍 2 |

新增断言逐条对应：

1. **每屏末拍点“下一拍”必然翻屏**：第 2 屏末拍 → `data-story-screen="machine"`；第 3 屏末拍 → `"task"`；第 4 屏末拍 → `html[data-yf-mode="sim"]` 且自动启动导览。harness 记录 `lastBeats=3/3`。
2. **翻屏后 kicker 前缀与新屏编号一致**：第 2 屏 `02 / 04 · 难题 · 拍 1 / 2`；第 3 屏 `03 / 04 · 机器 · 拍 1 / 3`；第 4 屏 `04 / 04 · 任务 · 拍 1 / 2`。`[data-beat-body]` 同步为 `把火悬在中间` / `撑开红线`，不再命中旧屏。断言同时检查 kicker/body 节点位于 active panel。
3. **任意时刻可见的 `.yf-story-beat-next` 恰好 1 个**：harness 每 80ms + 每次屏切换/末拍点击后统计 `offsetParent !== null` 的按钮，共 116 次检查全部为 1（第 2–4 屏）；封面没有拍导航，计数为 0。这是“任意时刻”在封面这一唯一无拍屏的例外，已在断言里显式区分。

关键截图：

```text
dsh-verify-out/story-nav-real/02-s2-beat2.png          末拍“下一屏 →”可见，不再死路
dsh-verify-out/story-nav-real/03-s3-beat1.png          第 3 屏拍 1/3，单一“下一拍”
dsh-verify-out/story-nav-real/06-s4-beat2.png          第 4 屏末拍“进入模拟器 →”
dsh-verify-out/story-nav-real/07-handoff-sim.png       交棒后模拟器
dsh-verify-out/story-nav-bottom-arrow/bug2-fixed-screen3.png  BUG2 底部箭头路径
dsh-verify-out/story-nav-assert/harness-03-pass-handoff.png   断言 harness PASS 徽标
dsh-verify-out/story-nav-key/key-01-last-beat-to-screen3.png  方向键末拍翻屏
dsh-verify-out/narrow-beat2/narrow-s2-beat2.png               390×760 第 2 屏末拍：“下一屏 →” + CTA 不溢出
```

### 22.4 回归

| spec | 结果 |
| --- | --- |
| `specs/acceptance-harness.json` | PASS 20/20 spec；页内 harness **79/79 checks** |
| `specs/story-beats-shots.json` | PASS 24/24 |
| `specs/harness-narrow.json` | PASS 4/4（390×760 iframe） |
| `specs/harness-narrow-beat2.json` | PASS 4/4（390×760，第 2 屏拍 2/2 的“下一屏 →”与 CTA 不溢出） |
| `specs/harness-tour.json` | PASS 5/5 |

测试侧同步：`specs/harness-wide.html` 的两处 `[data-beat-body]` 选择器改为 `.yf-story-body`，因为 `data-beat-body` 现在是“当前拍”标记、不再是全量拍列表；这是测试选择器修正，不影响页面行为。

### 22.5 约束核对

- 物理计算 12 函数：`specs/check-physics.mjs` 对 `index.bak4.html` 与修复后 `index.html` 逐项 SHA-1，12/12 一致：

```text
modelOf       5ec6fda8d40c4802b0c64c4d12bd3cdeaee5ac6f
readoutOf     aa133e18aea03ea5bc066477911eaf3c85fc1b83
spring        38a19e490d0c944cb9926971ec6aee0708d46d1b
bForMargin    2a339b8147bae6334a48952fdac243d3e4e8fc18
pForMargin    498f687502500478584b117f29e6fc174793a2a2
pForQ         6dacd1468960a7e1fb3b6596b3602d1538478dde
pForTemp      104f73d85fac51227f956d82beddb043563f4a42
diagnosisOf   637ad8abfe946126cff0db2f70b1e9441aa8ead8
updateExpert  bff1976a949aae3389431fe26e14c1ce10093606
updateScan    9dcd187f4388896d4f4c5f06887b8abf434db9a8
updateNow     4c11a3df6a334908008a5be90689896937cfee95
tick          0a991b6b83067423aa32f38c73dc466361568479
```

- 单文件零依赖：`<script src>` 0、`<link>` 0、`@import` 0；`http://` 仅 SVG 命名空间字符串；未新增外部文件。
- 备份：`index.bak4.html` 为动手前版本；`specs/`、`dsh-verify-out/` 保留为未跟踪自验产物。
- 提交：单提交、不 push，提交信息 `修复故事末拍死路与翻屏拍态残留（lead 验收）`。
