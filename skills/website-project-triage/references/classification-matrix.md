# 网站项目分级矩阵

本矩阵是 `website-project-triage` 的确定性分级依据。每次只选一个前端/体验主类 `A`–`E`，再独立判断 backend/business track `F`。分级依据是最难的必需子系统，不是参考站看起来像什么。

## 通用判定规则

1. 先确认页面/内容、backend、motion/graphics、hosting 和素材。未知项若仍可能改变主类、F track、实时图形边界，或使 hosting 责任、素材/语言准备度与交付范围无法明确限定，返回 `NEEDS_INFO`，不得猜最终等级。若这些维度已被事实或明确排除项充分限定到唯一等级，剩余确认问题不会改变等级，则返回 `CLASSIFIED`，通常使用 `MEDIUM` confidence，并保留问题与风险；不得机械地把“存在未知项”等同于 `NEEDS_INFO`。
2. 已知事实只能确定硬性下限、仍可能升级或改变等级时，可报告 provisional 下限并使用 `LOW` confidence；其余无法判断主类的情况用 `primary_class: UNDETERMINED`。
3. 两个相邻等级都合理时向上取级：`A+`/`B-` 取 `B-`，`B+`/`C-` 取 `C-`，`C+`/`D-` 取 `D-`，`D+`/`E-` 取 `E-`。
4. 素材缺失或混乱只增加 workload/risk，不会把项目降级。
5. 没有 backend 时输出 `backend_class: NONE`，不得用 `F-` 表示“无 backend”。
6. WebGL、Three.js、WebGPU、3D、particles、Shader、AI navigation 和 multi-user interaction 都是升级/澄清信号，必须单独确认。只有客户已接受为交付范围的实时 WebGL/Three.js/WebGPU（或等价 runtime graphics）scene 才形成 D 的硬性下限；只出现 3D、particles 或 Shader 等词而实现方式/是否实时未知时，不得直接定 D。

## A — Standard template site

选择一个现有整站 template，仅替换品牌、文字、图片、联系信息和有限产品卡片；navigation、组件结构、layout、移动端逻辑和 form logic 保持不变。交付为静态前端，不含登录或 CMS。

| 等级 | 确定性条件 |
| --- | --- |
| `A-` | 一至三个简单页面、section 很少、素材无特殊处理，且整站 template 结构完全不变。 |
| `A` | 标准 template 范围；常规页面和内容替换，不改变组件或交互逻辑。 |
| `A+` | 页面/内容/素材接近 template 范围上限，或仅有少量组件顺序调整、较多内容整理；仍不形成自定义信息结构。 |

边界测试：只要需要从多个标准组件重新组装页面、形成自定义信息结构，或改变移动端/组件逻辑，就不再是 A；相邻不确定时取 `B-`。

## B — Light custom brand site

使用获准的标准组件重新组合，而不是使用一个完整整站 template；采用一个品牌方向和自定义信息结构。产品、news、case 页面为静态内容，news 不实时；客户可管理后台必须另加 F。

| 等级 | 确定性条件 |
| --- | --- |
| `B-` | 主要由标准组件构成，仅有限重排和品牌样式调整；是 A/B 边界的向上结果。 |
| `B` | 多种页面类型、明确的品牌组合和自定义信息结构。 |
| `B+` | 页面/内容接近本类上限，含较多自定义组件、前端 filtering/search，或显著 responsive refinement；仍无高级原创 motion 系统。 |

边界测试：品牌配色或静态 case 页面本身不升到 C；出现原创视觉方向、定制交互、成组 motion/transition 或 custom cursor 时，从 `C-` 评估。若 filtering/search 依赖数据库或客户管理的数据，前端仍按 B/C 判定，同时另加 F。

## C — Advanced visual and motion site

包含原创视觉方向、custom interaction、motion sequences、transition、custom cursor 或 video integration。WebGL/实时 3D 不自动包含在 C；客户可管理后台仍须另加 F。

| 等级 | 确定性条件 |
| --- | --- |
| `C-` | 高级视觉设计，motion 数量和状态有限。 |
| `C` | 多套协调 motion systems 和定制 page transitions。 |
| `C+` | 复杂 scroll narrative、大量 animation states、严格移动端降级或专项 performance work，但没有主要 WebGL scene。 |

边界测试：仅 video、CSS/DOM/Canvas 动效，或只出现 3D、particles、Shader 等词，不能据此认定 D。若这些词对范围有实质影响但 implementation/runtime detail 未确认，返回 `NEEDS_INFO`，确认后按实际实现判 C 或 D。只有客户已接受为交付/验收范围的实时 WebGL/Three.js/WebGPU（或等价 runtime graphics）scene，才至少按 provisional `D-`，不得留在 `C+`。

## D — Immersive WebGL interactive site

客户已接受的交付范围至少包含一个实时 WebGL/Three.js/WebGPU 或等价 runtime graphics scene；该 scene 可涉及 3D assets、particles、Shader、camera path 或与 scroll/mouse 联动。必须单列设备覆盖、performance budget、降级方案和 scene-based acceptance criteria。

| 等级 | 确定性条件 |
| --- | --- |
| `D-` | 一个简单 WebGL hero 或一个边界清晰的 contained interactive scene。单个页面 scene 即使含 particles、mouse/scroll camera interaction，在 scene 数、资产复杂度、协调 transition 和移动端降级范围未确认时，也只形成 provisional `D-` 下限。 |
| `D` | 一个标准 immersive scene，并有已确认的协调 transition 和移动端降级范围；不得仅凭“移动端可用”要求或待确认 fallback 从 `D-` 上调。 |
| `D+` | 多个但仍边界清晰的 scene、advanced Shader/particles、大量原创 3D assets，或严格设备/performance 目标。 |

边界测试：若实时体验扩展为 site-wide runtime，或多个实时 scene 构成全站核心叙事并需要跨学科旗舰制作能力，从 `E-` 评估；`D+`/`E-` 证据冲突时取 `E-`。

## E — Flagship immersive digital experience

包含多个实时 scene 或 site-wide interactive runtime；可含 advanced particles、Shader、physics、AI navigation、multi-user interaction 或 custom creative tooling。通常需要 creative direction、UI、motion、3D、creative frontend、graphics engineering、QA、performance engineering 和项目管理协同。

| 等级 | 确定性条件 |
| --- | --- |
| `E-` | 旗舰呈现，已有多 scene/全站 runtime 证据，但系统复杂度有限。 |
| `E` | 完整 multi-scene experience，实时系统贯穿主要体验。 |
| `E+` | real-time multi-user、AI navigation、复杂 physics、custom engine/tooling，或异常广泛的设备/SLA 要求。 |

边界测试：单个高质量实时 scene 不足以认定 E；必须有多个实时 scene、全站 runtime 或 `E+` 系统信号。证据不足时保留已知 D 下限并返回 `NEEDS_INFO`。

## F — Backend and business-system track

F 与 A–E 正交，可附加到任何主类。只要客户登录、管理数据、处理支付、保存结构化数据或连接业务系统，就必须单列 F。

| 等级 | 确定性条件 |
| --- | --- |
| `F-` | fixed-schema 内容管理、form storage、简单 file upload，或一个简单第三方 integration。 |
| `F` | 登录/member area、database workflow、standard payment、多个 API、roles，或 structured content management。 |
| `F+` | ERP/CRM integration、复杂 permissions/approval workflow、高风险 payment/data flow、multi-tenant、real-time sync，或 custom business platform。 |

边界测试：静态 news 不加 F；客户需要发布、批量编辑或保存结构化内容时至少 `F-`，登录、支付或标准角色/数据库流程通常为 `F`，业务系统集成、复杂审批或高风险数据流为 `F+`。多个条件并存时按最难的 backend 子系统定级，不把 F 隐藏到主类中。

## 升级与停止信号

| 事实信号 | 必须动作 |
| --- | --- |
| 只有参考 URL，未说明复制视觉、layout、motion、3D 还是功能 | `NEEDS_INFO`；不得把参考站全部范围视为已包含。 |
| 已接受的交付范围含实时 WebGL/Three.js/WebGPU（或等价 runtime graphics）scene，却被称为“普通动画” | 至少记录 D 下限，并要求图形技术评审。 |
| 只提到 3D、particles、Shader 或 camera interaction，未确认 implementation/runtime detail 或是否实时 | 若对范围有实质影响，返回 `NEEDS_INFO`；补问后按实际实现判 C 或 D，不得先记 D 下限。 |
| 登录、CMS、支付、database、API、ERP/CRM 被包装成“静态站” | 单列 F，并补问数据、安全、合规和验收。 |
| 页面、backend、图形、hosting 或素材的未知仍可能改变等级/track，或无法限定交付边界 | `NEEDS_INFO`；提出目标问题。 |
| 等级、F、图形及交付责任已充分限定，剩余未知可排除且不会改变等级 | `CLASSIFIED` + 适当 `MEDIUM`；保留确认问题、排除项与风险。 |
| 分类矩阵或 intake checklist 无法加载 | `ESCALATE`；写明缺失 reference，不得临时编造。 |
| 证据互相冲突、出现特殊许可/异常责任或超出矩阵的系统 | `ESCALATE`，交由相应 technical/legal/security reviewer。 |

任何等级都不是报价或合同批准。输出必须保留 `human_review`，并在报价、订单起草/创建或签署前取得指定人工批准。
