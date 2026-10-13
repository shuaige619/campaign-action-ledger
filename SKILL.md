---
name: campaign-action-ledger
description: Use when 自媒体或电商活动的多场会议笔记、转写或旧台账需要整理活动承诺、改期、取消和待确认事项，尤其是同一任务分散在不同记录时。
---

# 活动跨会议行动台账

读取用户给出的全部会议文件、旧台账、活动范围与输出目录。用户未指定目录时，使用当前工作目录下 `outputs/campaign-action-ledger/` 并在完成时说明；若已有同名结果，则用其中的时间戳子目录保留旧版。其他缺口标待确认，继续整理已有材料。只处理本地资料，不发消息、提醒、发布内容或修改店铺。

按原有段落 ID 建立来源；没有 ID 时用“文件名:行号”。逐条区分明确承诺、建议、未决问题和取消记录。保留商品核对、素材交付、上线检查等活动事项的原话边界；“最好有人看看”不等于已指派。“收到”表示接受任务，不证明已开始或已完成。

按活动、交付物和责任语义匹配同一任务，沿用稳定 ID；相似名称无法确定时分列待确认。只有明确更改才更新负责人、期限或状态，保存前后值与各自来源；仅因记录较新不能覆盖矛盾。相对日期有会议日期和时区才换算，否则保留原话并置空。取消任务保留历史但不列为当前待办；未决定事项单列，不能补造负责人、冻结日期或检查内容。

在指定目录真实写入 UTF-8 `report.md`、`result.json`、`actions.csv`。报告先列当前承诺，再列变更、取消、冲突与待确认。JSON 顶层为 `skill`（本技能名）、`source_ids`（全部来源 ID 数组）、`actions`、`changes`、`pending`；后三者为数组。每条 action 必含 `id/task/owner/deadline/status/source_ids`，未知值用 null；status 为 open、in_progress、done、cancelled 或 needs_confirmation。changes 含 action_id、field、before、after、source_ids；pending 含 question、source_ids。CSV 列为 id,task,owner,deadline,status,source_ids，来源数组写 JSON 字符串。

写后回读文件，验证 JSON 可解析、三个文件行动项一致、引用均能定位、无重复 ID、无凭空状态；空数组也输出。完成回复给出真实文件路径与尚待确认事项。
