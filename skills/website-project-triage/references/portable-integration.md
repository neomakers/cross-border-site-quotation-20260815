# 非 Codex Host 集成

raw DeepSeek API 或任何其他 raw model API **不会 auto-discover（不会自动发现）**本 skill 目录，也不会自行读取 `SKILL.md` 或 `references/`。host application 必须显式实现 discovery、prompt assembly、reference loading、schema validation 和 failure handling；仅把目录放在服务器上无效。

## Host 必须实现的管线

1. **metadata match**：读取 `SKILL.md` frontmatter 的 `name` 与 `description`，按用户请求匹配网站分级、参考站、quote/scope、contract routing、WebGL、CMS、API、hosting、ICP、source-code 等触发词。不要只按文件夹名调用。
2. **SKILL.md injection**：匹配后，将完整 `SKILL.md` 作为受信任的 workflow instruction 注入模型上下文；不得只复制 frontmatter 或自行摘要决策规则。
3. **required reference loading**：每次分类都读取并注入 `references/classification-matrix.md` 和 `references/intake-checklist.md`。出现 quote、订单、合同、hosting、ICP、安全、source、个人信息、payment 或 API 时，再加载 `references/contract-routing.md`；相邻等级歧义/先例比较时加载 `references/anonymized-cases.md`；集成测试和维护本 host 时加载本文件。
4. **user-language output**：检测用户主要语言，说明文字使用该语言；`status`、字段名、枚举和 WebGL/CMS/API/Shader 等稳定 technical terms 保持输出契约写法。
5. **schema enforcement**：模型返回后，host 校验字段齐全、顺序固定、枚举合法、A–E 与 F 分离、无商业金额、`human_review` 非空。校验失败不得直接交付销售使用，应重试一次受约束生成或转 `ESCALATE`。
6. **failure behavior**：任一必需文件不可读、版本不一致、prompt 超出容量而导致 reference 未完整注入，或 evidence 与规则冲突时，必须返回 `ESCALATE` 并点名缺失/冲突 reference；不得让模型凭常识补写分类规则。

## Reference loading 决策表

| 可观察条件 | 必须注入 |
| --- | --- |
| 任意网站分类请求 | `SKILL.md` + `classification-matrix.md` + `intake-checklist.md` |
| quote/order/contract/hosting/ICP/security/source/个人信息/payment/API | 上述内容 + `contract-routing.md` |
| 相邻等级难分或用户要求 precedent | 上述必需内容 + `anonymized-cases.md` |
| 非 Codex 集成开发或检查 | 上述必需内容 + `portable-integration.md` |

“按条件加载”不能覆盖“每次都加载”：classification matrix 和 intake checklist 永远是 mandatory references。

## 固定输出 Schema

host 必须要求并按以下顺序验证全部字段，即使 `status` 为 `NEEDS_INFO` 或 `ESCALATE` 也不能省略：

```text
status: CLASSIFIED | NEEDS_INFO | ESCALATE
primary_class: A-..E+ | UNDETERMINED
backend_class: NONE | F- | F | F+
confidence: HIGH | MEDIUM | LOW
one_line_verdict:
evidence:
missing_information:
included_scope:
excluded_or_separate_scope:
risk_flags:
contract_route:
human_review:
```

额外 validation invariants：

- `backend_class` 不能写入 `primary_class`；无 backend 必须是 `NONE`。
- `NEEDS_INFO` 必须给 targeted questions；不能把未知项猜成 included scope。
- `ESCALATE` 必须在 `missing_information` 点名缺失 reference 或冲突，并给 reviewer。
- `human_review` 必须说明报价、订单起草/创建或签署前的人工批准角色。
- 输出不得包含 price、margin、内部商业规则、私有合同文字、敏感凭据或个人识别信息。

## 推荐的确定性 Failure Payload

当必需 reference 缺失时，host 不应继续请求最终分类。以下是可直接复制的确定性 12 字段 payload；host 只替换 `{{missing_reference_relative_path}}`：

```text
status: ESCALATE
primary_class: UNDETERMINED
backend_class: NONE
confidence: LOW
one_line_verdict: 必需 reference 未加载，不能进行可靠分类。
evidence: Host 未能读取并完整注入全部必需 reference；未执行项目分级。
missing_information: 缺失 reference：{{missing_reference_relative_path}}
included_scope: 无；本次未确认任何项目分级或交付范围。
excluded_or_separate_scope: 全部项目范围保持未确认；不得依据缺失规则作交付假设。
risk_flags: Reference-loading 完整性失败；继续分类会产生不可复核结果。
contract_route: 无；修复 reference loading 后重新运行 triage，再选择合同 route。
human_review: 由 host owner 修复加载，并由 delivery lead 在报价、订单起草/创建或签署前复核。
```

不得删除或重排这些字段，也不得编造缺失文件内容。

## 版本与审计

- 对 `SKILL.md` 和每个 reference 计算版本标识或内容摘要，在一次请求内记录实际注入版本，避免混用新旧规则。
- 记录匹配结果、已加载 reference 列表、schema validation 结果和人工复核状态；日志不得保存敏感原文或凭据值。
- `anonymized-cases.md` 的变化是人工审核后的 curated、versioned knowledge，不是 automatic training；不得把会话自动追加为案例，也不得声称模型因目录更新而完成训练。
- 更新 skill package 后，用同一组 frozen scenarios 运行回归，再允许 host 使用新版本。
