# 售前 Intake Checklist

用于销售对话的紧凑问题清单。先记录客户已明确的事实，再问缺口；不要把推测写成 evidence。

## 最少证据门槛

形成 confident classification 前，至少要有以下五组事实：

1. 页面与内容：page count、page types、navigation、产品/news/case 数量，news 是否静态。
2. 技术边界：template/组件是否可用，motion/3D/WebGL 是否需要，backend/business functions 是否明确为有或无。
3. 交付环境：domain/hosting 责任、地区/ICP 需求、验收 devices/browser。
4. 素材与语言：文案、图片、video、3D assets、语言包和 approved translation 的提供方与准备状态。
5. 治理：决策人、反馈节奏、目标日期、验收方式和 source/deployment ownership。

把缺口分为两类：若缺口仍可能改变主类、F track、实时图形边界，或使 hosting 责任、素材/语言准备度与交付范围无法被明确限定，它是分级阻断项，输出 `NEEDS_INFO`。若主类、F、图形、hosting 责任与素材准备风险已充分限定到唯一等级，剩余问题可明确排除且不会改变等级，则可输出 `CLASSIFIED`/`MEDIUM`，同时保留 targeted questions、排除项与风险。只有必需 reference 缺失或证据冲突无法按矩阵处理时才输出 `ESCALATE`。

## 销售问题

### 参考与视觉

- 参考站具体要复刻什么：visual style、layout、motion、3D 还是 functionality？
- 可以直接使用一个 whole-site template，还是只能使用获准 components？哪些结构允许变化？
- 是否要 custom interaction、transition、custom cursor、video、Canvas、WebGL、Three.js、WebGPU、particles、Shader、AI navigation 或 multi-user？
- 若有实时 scene：scene 数、3D assets 来源、camera path、交互、目标设备、browser、performance budget、mobile fallback 和 scene acceptance 各是什么？

### 页面、内容与语言

- 有多少页面、多少 page types？navigation 和每类页面的数量是什么？
- 产品、news、case 有多少条；是静态交付还是客户持续管理？
- 有几个 language packs？谁提供并批准翻译？
- 文案、图片、video、产品资料、字体/品牌许可和 3D assets 是否齐备？缺失项由谁在何时确认？

### Backend、数据与集成

- 是否有 CMS、登录/member area、roles、batch editing/upload、database、search/filter、file upload 或 scheduled tasks？
- 是否有 payment/invoice flow？provider、币种/地区、退款或异常流程、验收路径是什么？
- 需要几个 API？数据方向、频率、失败处理、接口文档、测试环境和责任方是什么？
- 是否连接 ERP/CRM，含 approval workflow、real-time sync、multi-tenant 或复杂 permissions？
- 会收集哪些 form/member/analytics/cookie/log 数据？保存位置、期限、访问角色及跨境情况是什么？

### Domain、hosting、security 与 source

- 谁购买、持有、配置 domain/hosting？部署地区、预期 traffic、storage、bandwidth、video/large-file 需求是什么？
- 是否使用中国大陆节点或需要 ICP filing assistance？主体和时点是否已确认？
- 只需要 baseline security，还是明确要求 WAF/DDoS、audit、monitoring、渗透测试、compliance 或 SLA 等 advanced security？
- 谁负责上线后的 monitoring、更新、备份、incident handling？
- 是否要求 source、repository、迁移、self-hosting、部署接管或认证凭据交接？交付边界和验收方式是什么？

### 进度与验收

- 目标日期是偏好还是不可移动约束？依赖哪些素材、审批、备案或第三方接口？
- 谁是最终 decision maker？反馈人有几位，多久完成一轮反馈？
- 验收页面、功能、devices/browser、性能指标和 defect/change 规则是什么？

## Evidence 记录格式

每条 evidence 使用“已确认事实 + 来源角色 + 待确认边界”，不复制敏感原文。例如：

```text
已确认：需要一个实时 3D hero，包含 mouse interaction 和 scroll-linked camera。
来源：销售访谈中的明确需求。
待确认：3D assets、目标设备、performance budget、mobile fallback、scene acceptance。
```

不要把参考站默认功能、未获确认的 hosting/security/source、占位素材或模糊的“和它一样”写入 `included_scope`。

## Confidence 与动作

| 证据状态 | 输出动作 |
| --- | --- |
| 五组事实完整，边界清晰，无冲突 | 可 `CLASSIFIED`；根据证据用 `HIGH` 或 `MEDIUM`。 |
| 已确定唯一等级，主类/F/图形与交付责任已限定；仅剩不会改变等级的确认问题 | `CLASSIFIED`、通常 `MEDIUM`；继续列问题、排除项与风险。 |
| 有已知下限，但一个或多个关键边界未确认 | `NEEDS_INFO`、`LOW`，可写 provisional 下限。 |
| 无法判断主类，或参考 URL/固定日期替代了范围 | `NEEDS_INFO`、`primary_class: UNDETERMINED`、`LOW`。 |
| 必需 reference 缺失或冲突/异常责任超出矩阵 | `ESCALATE`，写明缺失项和 reviewer。 |

无论结果如何，都要列 `excluded_or_separate_scope`、`risk_flags`、`contract_route` 和 `human_review`。每个 `human_review` 必须保留指定 reviewer，并用用户语言明确表达“人工审批完成前，不得报价、起草/创建订单或签署合同”的完整含义。对当前请求适用但未明确 included/confirmed 的责任必须镜像到 `excluded_or_separate_scope`，不能只留在问题或风险中：实时/定制视觉且素材责任未定时列 original/custom asset production，并检查 undefined additional scenes、hosting/security/source transfer；文案、图片或产品资料缺失/创建责任未定时列 content creation/copywriting 与适用的 asset production；source/repository/self-hosting/部署接管/batch import/upload/既有系统或数据转移相关且迁移责任未定时列 migration；backend/payment/data/integration 上下文还要检查 ERP/CRM、复杂 permissions、integrations、security/hosting operations 与 source transfer；多语言责任未定时列 translation production/approval；固定或加速日期可行性未确认时列 expedited delivery。无对应上下文时不机械添加。输出前验证每个适用未确认项真实出现在排除字段。`contract_route` 先列基础附件，再独立计算 hosting route 谓词：只有原始请求/事实明确引入 domain/hosting ownership、provider、资源、配置、部署、self-hosting 或 migration 责任，供应方提供/配置 hosting，或 backend/payment/data/batch-import/integration 业务系统实质需要 hosting 责任时，才增加域名/hosting 条款；责任未定时写为待责任确认的条件路由。纯 A–E 项目按本 checklist 标准补问、报风险或列排除，不会单独触发 hosting route。
