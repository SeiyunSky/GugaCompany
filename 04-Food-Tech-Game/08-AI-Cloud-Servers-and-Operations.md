# M08｜AI 云、服务器与运维

## 目标
让玩家以可理解的方式看到“是谁把用户任务送到正确团队、如何使用菜谱知识和如何保持系统安全”，而不是把机房变成神秘魔法或暴露内部实现。

## 场景
可步入服务器机房，机柜贴有功能标签：`TASK-ROUTER`、`RECIPE-CORE`、`SIMULATION-STATE`、`CONTENT-CMS`、`SAFETY-GUARD`、`OBSERVABILITY`。不同颜色数据流来自总部、工厂、农田和 SAP 模拟中心。

## 岗位
AI Orchestrator、Recipe Knowledge Engineer、Data Steward、Model Ops Engineer、Security Guardian、Observability Operator、Simulation Engineer。

## 玩家可见内容

| 热点 | 解释 |
|---|---|
| TASK-ROUTER | 根据时间、忌口和目标，将任务分发给菜单/厨房/研发模板 |
| RECIPE-CORE | 保存经审核的食材替代、步骤和安全规则，不读取私人冰箱 |
| SIMULATION-STATE | 保存当前会话和已选分支，离开后可清除 |
| SAFETY-GUARD | 阻止高风险、虚假或不适用的结果 |
| OBSERVABILITY | 展示匿名活动指标：完成率、卡点、性能，不展示个人行为回放 |

## 数据原则
活动默认本地会话；服务端只接收匿名埋点与经用户允许的分享结果。若启用一次受限 AI 个性化，后端必须做输入长度/敏感内容/JSON 格式校验，失败时回退模板结果；不让模型自由控制任务流程。

## 验收
- 玩家知道 AI 是受规则、内容和安全边界约束的系统；
- 不展示系统提示词、密钥、内部网络或真实监控数据；
- 任意外部服务不可用时，预设模板主线仍可完成；
- 机房信息可作为可选探索，不阻塞主线。
