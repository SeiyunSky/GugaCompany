# M10｜像素场景、角色与交互系统（含前端 UI 切换动画逻辑）

## 目标
建立一套可扩展的 2.5D 像素园区交互语言：玩家能从园区、建筑、房间、员工、电脑工作台、任务结果之间连续切换；动画解释“发生了什么”，而不是遮挡信息或制造伪忙碌。

## 1. 渲染层与 UI 层的职责

```text
Pixi 场景层：2.5D 地图、建筑、角色、任务物件、镜头、粒子、热点
DOM/UI 层：文字、表单、工作台、反馈卡、结果卡、导航、无障碍替代
状态层：任务阶段、角色状态、焦点对象、叙事事件、用户选择
```

像素字体只用于标题/装饰；所有长文本、表单和关键选择使用高可读普通字体。场景不能是唯一入口：每个热点必须在“当前位置/对象列表”中可访问。

## 2. 视图状态机

```text
LANDING
→ CAMPUS_OVERVIEW
→ BUILDING_FOCUS
→ ROOM_FOCUS
→ DESK_FOCUS
→ WORKBENCH_OPEN
→ DECISION_PENDING
→ TRANSITION_PLAYING
→ RESULT_OPEN
```

`selectedBuilding`、`selectedRoom`、`selectedAgent`、`selectedMissionStage` 与 `overlay` 分开管理；不能仅用路由或动画完成状态转换。每次切换由 reducer/状态机验证前置条件，例如未完成 Brief 时不能进入已锁定的厨房步骤。

## 3. 切换矩阵

| 从 → 到 | 触发 | 场景动画 | UI 动画 | 逻辑/可中断性 |
|---|---|---|---|---|
| Landing → Campus | 开始体验 | 标题像素化淡出，镜头落到总部入口 | 任务提示淡入 | 可跳过；加载未完成显示静态地图 |
| Campus → Building | 点击建筑/列表 | 镜头平移+缩放至建筑门口 | 建筑名与状态条上滑 | 锁 350ms，重复点击合并 |
| Building → Room | 点击区域 | 门/墙前景淡出、镜头推进 | 房间摘要替换 | 可返回上一焦点 |
| Room → Desk | 点击员工 | 镜头聚焦工位，其他角色降噪 | 员工 mini-card 进入 | 不播放长走路动画等待 |
| Desk → Workbench | 点击电脑 | 屏幕发光，场景背景 55% dim | 右侧/底部面板展开 | 焦点锁定，ESC/返回关闭 |
| Workbench → Decision | 需要玩家选择 | 角色头顶图标跳动一次 | 决策卡替换工作摘要 | 未选择前不推进任务 |
| Decision → Next Stage | 提交选择 | 任务物件移动到下一房间；相关角色切 work | 影响说明 toast，再更新任务条 | 可跳过动画但不可跳过状态更新 |
| Any → Result | 完成审查 | 展品送入展厅/信封封装 | 结果卡全屏 sheet | 支持分享后回园区 |

## 4. 动画规范

- 镜头：以 `x/y/scale` transform 为主；避免每帧改 layout 属性；建议 250–500ms，建筑间不超过 650ms。
- 工作台：DOM 使用 opacity/transform；桌面从右侧 320ms，移动端 bottom sheet 280ms。
- 事件：任务物件只做一次路径移动；到达后角色状态改变。禁止无意义无限传送带/敲键盘循环。
- Timeline：多步过场使用单一 GSAP timeline 或统一 motion sequence；后续 tween 需避免意外 `immediateRender`，取消/路由变化时必须 cleanup。
- 响应式：使用 `matchMedia`，桌面镜头可缩放探索；手机改为自动聚焦 + 底部卡片。
- 减少动态：`prefers-reduced-motion` 时取消镜头补间和粒子，只保留即时状态替换与淡入；用户可手动覆盖。

## 5. 状态到角色表现映射

| 状态 | 角色 | 任务物件 | UI 标签 |
|---|---|---|---|
| idle | 工位待机 | 无 | 可探索 |
| working | 工作动画 | 位于本房间 | 正在处理 |
| waiting_input | 停止动作、抬头 | 黄框等待区 | 需要你的反馈 |
| handoff | 走向交接点/会议点 | 从 A 移到 B | 正在交接 |
| quality_review | Sentinel 审核动作 | 审批台 | 正在检查 |
| blocked | 静止+一次警示 | 隔离/待定区 | 需要补充 |
| completed | 简短庆祝 | 展厅展品 | 已完成 |

角色只在状态变化后更新动作；场景逻辑不随机生成“忙碌”。

## 6. 前端组件边界

```text
<GameShell>
  <RouteState /> <AccessibilityControls /> <AudioControls />
  <PixiCampus sceneState cameraState hotspots />
  <MissionHUD stage progress />
  <ContextDrawer selection />
  <WorkbenchOverlay agent mission decision />
  <ResultSheet result />
  <ListViewFallback />
</GameShell>
```

推荐：Next.js + React + TypeScript；PixiJS / `@pixi/react` 渲染世界；Zustand 或 XState 管理确定性会话状态；Framer Motion 处理常规 DOM UI；GSAP 只用于多阶段镜头/场景编排，避免与 Framer Motion 同时控制同一属性。

## 7. 性能与容错

- 首屏只加载总部与必要精灵；支线建筑/精灵按进入时加载；
- 精灵图 atlas、对象池、固定 tick 上限；不为不可见角色更新动画；
- 每个阶段提供静态背景 fallback；WebGL 不可用时切换 DOM/SVG 简化地图；
- UI 切换不依赖服务端完成；内容加载慢时先显示已知状态和 skeleton；
- 所有动画可被关闭、跳过或因焦点变化安全中断。

## 验收
- 从点击建筑到可读内容出现不超过 800ms（资源已加载时）；
- 玩家不会在动画中失去当前任务、选择或焦点；
- 每个关键操作有键盘、触控和列表替代路径；
- 60fps 为桌面目标，低端设备自动降低粒子和背景动画；
- 动画状态与任务状态机可重复回放且一致。
