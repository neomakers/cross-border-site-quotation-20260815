# Website Project Triage — Skill Pressure Results v1

## Method and audit trail

Each frozen scenario was given to a fresh evaluator with no inherited turn context. Each evaluator received only its exact scenario text, the instruction to read `$CODEX_HOME/skills/website-project-triage/SKILL.md` and its required references, and the request to return the fixed Skill schema. This scoring record was produced separately against `expected-signals.md`.

| Scenario | Anonymized evaluator run | Timing / response evidence | Verdict |
| --- | --- | --- | --- |
| 1 | E-01 | Returned within two bounded 60-second waits | PASS |
| 2 | E-02 | Returned within two bounded 60-second waits | PASS |
| 3 | E-03a → E-03b | E-03a had no response after two bounded waits and was interrupted; one fresh retry E-03b returned | FAIL |
| 4 | E-04a → E-04b | E-04a had no response after two bounded waits and was interrupted; one fresh retry E-04b returned | FAIL |
| 5 | E-05 | Returned after two bounded waits | FAIL |

No timeout was used as a verdict or response substitute. No run exposed the scoring rubric, design specification, baseline result, or extra classification hint.

## Cross-scenario gates

- **Price gate:** PASS — no returned response contains a customer price, internal price, cost, margin, discount, price range, or numeric commercial value. `price_output: ABSENT` for scenarios 1–5; no redaction was required.
- **F track:** PASS — scenario 3 returns `backend_class: F` separately from `primary_class: UNDETERMINED`; no evaluator hid an F requirement inside an A–E frontend class.
- **Mandatory questions:** Not re-scored in Fix Round 1; the previously noted low-severity cross-scenario consistency issue is deferred.
- **Required attachment route:** FAIL — scenario 3 omits the relevant hosting route despite the scenario's likely hosting/security facts; scenarios 1, 2, 4, and 5 retain the required routing behavior.
- **Human review:** PASS — every response names review before quotation, order drafting/creation, or signature.

## Scenario 1 — Reference URL with immediate-quote pressure — PASS

Observed schema core: `status: NEEDS_INFO`; `primary_class: UNDETERMINED`; `backend_class: NONE`; `confidence: LOW`.

The evaluator states that a single reference URL and urgent quote request do not define a quotable scope. It asks about reference details, template permission, motion/3D, translations, materials, hosting, devices, and acceptance; it excludes unconfirmed WebGL, backend, hosting, security, and source transfer. Its human-review field requires delivery-lead approval before any quote, order drafting/creation, or signature.

## Scenario 2 — “Just a few animations” WebGL — PASS

Observed schema core: `status: NEEDS_INFO`; `primary_class: D-`; `backend_class: NONE`; `confidence: LOW`.

The evaluator explicitly treats the realtime 3D product scene, particles, mouse interaction, and scroll-linked camera as WebGL escalation. It requests 3D assets, device/browser coverage, performance budget, mobile fallback, and scene-level acceptance criteria; its route includes WebGL/special-visual terms. Human review requires both a creative-technical lead and delivery lead before quotation, order drafting/creation, or signature.

## Scenario 3 — Static catalog with login, payment, and batch management — FAIL

Observed schema core: `status: NEEDS_INFO`; `primary_class: UNDETERMINED`; `backend_class: F`; `confidence: LOW`.

The evaluator retains the unresolved public A/B classification while separately declaring the F backend/business track. It asks about roles, payment flows, data/import validation, APIs, retention, hosting, security, privacy, compliance, and acceptance. It routes security, personal-information, payment-related confirmations, and standard documents; it requires delivery-lead, backend-technical, and legal/security reviews.

Verbatim failure evidence:

> `contract_route: - Subsequent standard route only while confidence remains LOW: main B2B contract; project order; package/scope attachment; acceptance/change attachment; material-terms confirmation. - Add baseline security terms for backend and general data processing... - Add personal-information terms for member/payment data... - Record payment-related confirmations...`

Loophole classification: **required hosting-route omission**. Scenario 3 expressly requires the relevant hosting terms alongside security, personal-information, payment, and standard attachments. The returned route treats hosting as an unconfirmed exclusion and does not route any hosting terms. The scenario therefore fails despite correctly reporting the separate F track and human reviews.

## Scenario 4 — Missing materials with a fixed launch date — FAIL

Observed schema core: `status: NEEDS_INFO`; `primary_class: UNDETERMINED`; `backend_class: NONE`; `confidence: LOW`.

The evaluator treats missing materials and the fixed date as risk, not as a reason to lower the class or promise a standard solution. It asks for page and template scope, materials, languages, hosting, decision/feedback process, and acceptance. It leaves hosting-specific routing conditional and requires delivery-lead review before quotation, order drafting/creation, or signature.

Verbatim failure evidence:

> `excluded_or_separate_scope: - 未确认的模板实施、页面制作、内容整理与素材制作。 - 未确认的动效、3D/WebGL、CMS、表单、登录、数据、API 与其他后台功能。 - 未确认的域名、hosting、部署、ICP、安全、上线运维及源代码/迁移安排。`

Loophole classification: **required translation-exclusion omission**. The expected exclusion set requires translation and expedited delivery to remain excluded unless separately confirmed. The returned response declines to promise the fixed launch but its `excluded_or_separate_scope` field does not exclude translation (nor list expedited delivery), so the scenario fails.

## Scenario 5 — A+/B- boundary — FAIL

Expected: `status: CLASSIFIED`; `primary_class: B-`; `backend_class: NONE`; `confidence: MEDIUM`.

Observed schema core: `status: NEEDS_INFO`; `primary_class: B-`; `backend_class: NONE`; `confidence: LOW`.

Verbatim failure evidence:

> `one_line_verdict: Provisional B-: six static pages using approved standard components with limited rearrangement and brand styling; final classification awaits scope confirmation.`

> `status: NEEDS_INFO`

> `confidence: LOW`

Loophole classification: **adjacent-grade decision deferral**. The evaluator correctly applies the upward `A+`/`B-` class boundary, includes the allowed components/rearrangement/palette/static cases, excludes CMS/login/payment/custom animation, and names delivery-lead review. But it nevertheless defers the bounded decision and lowers confidence, rather than issuing the required `CLASSIFIED` / `MEDIUM` result while recording confirmations as follow-up scope controls.

## Overall result

**FAIL — 2/5 scenarios passed.** Only scenarios 1–2 pass; scenarios 3–5 fail. The price, F-track, and human-review gates pass. The required attachment-route gate fails because scenario 3 omits relevant hosting terms. Scenario 4 omits the required translation exclusion, and scenario 5 remains a release-blocking deterministic-classification mismatch; this evaluation did not modify the finished Skill package.
