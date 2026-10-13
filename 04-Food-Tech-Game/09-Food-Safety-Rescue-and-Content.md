# M09｜食品安全、用户急救与内容传播

## 目标
在有趣的像素体验中提供必要的食品安全边界、失败补救与可信内容，避免“为了好玩”鼓励不安全烹饪或掩盖不确定性。

## 三个子区

### 食品安全与质量实验室
岗位：Food Safety Officer、Allergen Specialist、Nutrition Analyst、Quality Inspector、Shelf-life Auditor。负责检查主线模板与工厂模拟事件。对用户结果只输出可执行提醒：过敏原、变质食材不使用、生熟分开、肉蛋海鲜充分熟制；不提供医疗或个体化治疗建议。

### 用户急救中心
岗位：Kitchen Rescue、Substitution Specialist、Complaint-to-Insight Analyst、Family Meal Mediator、Recipe QA Editor。玩家可在结果页点击“我只有 10 分钟”“太咸/太辣怎么办”“没有某食材”“锅糊了”等预定义求助项，获得经审核的模板化补救建议。未知或高风险情况引导用户停止食用/寻求专业帮助，不虚构解决方案。

### 食堂电视台（与 M13 公关部共享）
把经审阅的研发、质量改进和匿名体验反馈转为节目/公告。内容不能覆盖原始事件或替代安全警示。

## 事件与升级

```text
safety.rule.matched → result.warning.attached
user.rescue.requested → rescue.template.selected
factory.quality.failed → batch.simulation.blocked
content.publication.requested → pr.review.required
```

## 验收
- 任何结果卡都有可见安全说明；
- 急救建议标注适用条件；
- 质量失败优先阻断，而不是用 PR 文案覆盖；
- 用户反馈先匿名汇总，再进入内容系统。
