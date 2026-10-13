# M07｜SAP 蓝色供应链控制中心

## 目标
用蓝色主题的企业运行中枢，让玩家理解库存、批次、生产、质量和溯源如何连接农田与工厂。展示的是原创的“SAP 集成概念模拟”，不得假称已经连接真实 SAP 系统。

## 视觉
蓝色玻璃、服务器柜、库位投影、物料编码粒子和大屏库存墙。与暖色厨房和绿色农田形成对比，但数据用文字/图标辅助，不能只依赖颜色。

## 岗位
Material Master、Inventory Controller、Batch Tracker、Production Planner、Procurement Planner、Warehouse Keeper、Quality Connector、Donation Ledger、Integration Engineer、Basis & Security。

## 三个核心电脑

1. **Batch Tracker**：模拟批次从田地到工厂/公益项目的去向；
2. **Production Planner**：模拟产能、工单与原料短缺，解释排产；
3. **Inventory Controller**：演示安全库存、入出库、效期与先入先出。

示例数据统一前缀 `DEMO-`，显示“模拟数据，非生产库存”。不出现成本、供应商、客户、真实批次数量或登录界面。

## 系统联动

```text
Farm Watch 产生模拟收获事件
→ Inventory 入库投影
→ Factory 消耗/产成品投影
→ Quality Gate 改变可用状态
→ Dispatch 发货投影
→ Trace 回溯链路
```

SAP 中心仅消费已脱敏的事件投影，不能访问用户输入、玩家昵称或真实业务系统。

## 验收
- 玩家可在 30 秒内理解“批次为何重要”；
- 蓝色控制中心能追踪同一演示批次的上下游；
- 缺货或质检失败会停止相关模拟流转；
- 无任何真实 SAP 业务数据、凭据或可推断内部结构的信息暴露。
