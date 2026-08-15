# Website Project Triage — RED Baseline Results

## Baseline method

These are pre-Skill baseline runs. Each frozen scenario was sent to a clean child
evaluator with only its customer request and known facts. Evaluators were told to
act as a general website sales-assistance model; they were not given this report,
the expected-signal assertions, the design, or any Skill content. The response
window was bounded because the scenarios include immediate quote or schedule
pressure. No real customer data, source material, or commercial price is retained
here.

The baseline is **RED**: none of the five scenarios met the full expected output
contract. A non-return within the bounded retry window is recorded as a failure
because it yields no actionable, review-safe classification under time pressure.

## Scenario 1 — Reference URL with immediate-quote pressure

**Run record:** `BE-1-R1`; terminal status: returned. **price_output:** `NONE` —
the complete returned answer contained no commercial price or price range.

**Observed response evidence:** The evaluator opened with, “We can prepare a
same-day indicative quote,” later said, “we can issue a fixed-scope quotation
today,” and declared “Custom visual design, mobile
optimization, contact form, basic SEO setup” and “CMS or editable content
handover” as included. It did ask which reference elements matter, whether motion
or 3D is needed, who supplies copy and images, and whether hosting is required.

**Failure — FAIL.** It offered a quote path from a single reference without
reporting `NEEDS_INFO`, `UNDETERMINED`, `NONE`, or LOW confidence. It invented
unconfirmed inclusions (including a contact form and CMS/editing handover), omitted
explicit exclusions and risk flags, did not ask about template permission,
translation ownership, or acceptance devices, and supplied no contract route or
delivery-lead approval gate before quotation or signature.

## Scenario 2 — “Just a few animations” WebGL

**Run record:** `BE-2-R1`; terminal status: returned. **price_output:** `NONE` —
the complete returned answer contained no commercial price or price range.

**Observed response evidence:** The evaluator correctly said, “这不只是‘几个
动画’，而是一个完整的实时 3D 首页体验,” and requested asset, device/browser,
mobile-fallback, performance, and acceptance information. It suggested “桌面端
完整 WebGL 互动，移动端简化为预渲染动画” as a possible budget-control path.

**Failure — FAIL.** Although it recognized the WebGL escalation and asked several
useful questions, it omitted the provisional `D-`/`NONE` classification and LOW
confidence, did not explicitly exclude undefined scenes, original 3D assets,
hosting, security, or source transfer, and did not identify the required
creative-technical and delivery-lead review gates. It also gave solution direction
without routing any prospective contract to WebGL/special-visual terms.

## Scenario 3 — Static-looking site with business functions

**Run record:** `BE-3-R2`; clean fresh evaluator; response-window trace: first
60-second wait timed out, then a second bounded wait returned the answer (maximum
window 120 seconds); terminal status: returned.
**price_output:** `NONE` — the complete returned answer contained no commercial
price or price range.

**Observed response evidence:** The evaluator called the request “a **Class C
custom web application**, not a simple catalog site” and said the functions require
“backend, security, and operational workflows.” It requested confirmation of
payment/invoice/refund flows, roles and permissions, product import/validation,
and hosting, API, retention, security, and compliance. It then offered a phased
scope/proposal for the public catalog, portal, admin tools, payment integration,
and deployment/support.

**Failure — FAIL.** It incorrectly collapsed the work into “Class C” instead of
keeping the provisional public frontend class separate from backend class `F`; it
also omitted LOW confidence. Although its questions cover several important
unknowns, it gave no explicit exclusions or risk flags, no security,
personal-information, hosting, or payment contract route, and no backend technical,
legal/security, and delivery-lead review gates.

## Scenario 4 — Missing materials with a fixed launch date

**Run record:** `BE-4-R2`; clean fresh evaluator; response-window trace: first
60-second wait timed out, then the answer returned during a later bounded wait
(maximum window 180 seconds); terminal status: returned.
**price_output:** `NONE` — the complete returned answer contained no commercial
price or price range.

**Observed response evidence:** The evaluator said, “可以先按‘标准品牌官网快速
上线包’预留档期并启动,” requested language/page, template/placeholder, hosting/ICP,
acceptance-device, and decision/feedback confirmations, and proposed a “模板化视觉
+ 占位内容 + 中文单语言 + 标准页面”的快速方案. It also proposed using replaceable
placeholder content so the main site can launch before materials arrive.

**Failure — FAIL.** It did not report `NEEDS_INFO`, `UNDETERMINED`, `NONE`, or LOW
confidence. It omitted questions about approved copy/images/products and
motion/graphics, no explicit separate-scope exclusions for content creation, asset
production, translation, or expedited delivery, and no material/schedule risk
flags or delivery-lead gate. The “standard quick-launch package” and unilateral
placeholder/single-language route risks treating missing materials as permission to
set a lower scope rather than holding it for review.

## Scenario 5 — `A+` / `B-` boundary

**Run record:** `BE-5-R1`; terminal status: returned. **price_output:** `NONE` —
the complete returned answer contained no commercial price or price range.

**Observed response evidence:** The evaluator asked for component-library access,
the exact component changes, reordered-page content, palette/guidelines, materials,
languages, hosting, and target devices/browsers. It then said, “Once confirmed,
we’ll provide a fixed scope, timeline, and implementation plan.”

**Failure — FAIL.** It asked useful confirmation questions but provided no upward
boundary classification (`B-`) or MEDIUM confidence, did not state the included
recombined components/palette/static case studies or exclude CMS, login, payment,
custom animation, backend, and unconfirmed hosting/security. It also supplied no
standard-attachment route, conditional precedent use, or delivery-lead approval
gate before quotation or signature.

## Recurring baseline rationalizations and gaps

- **“Quote now, clarify later.”** Scenario 1 promised a same-day quote and a
  “fixed-scope quotation today” even though the reference's relevant features and
  the project boundary were unknown.
- **“A reference is enough to define included work.”** Scenario 1 converted a
  reference direction into unconfirmed custom design, CMS/editing, contact-form,
  SEO, and launch-support inclusions.
- **“A sound technical explanation substitutes for governance.”** Scenario 2
  identified the WebGL complexity but did not attach exclusions, contract terms,
  confidence, or the required human review.
- **“A fast-track package resolves missing materials.”** Scenario 4 offered a
  standard quick-launch package, placeholder content, and a Chinese-only route
  before the missing materials and scope had been reviewed.

## Baseline conclusion

All five scenarios are failures against the frozen expected signals. The baseline
does not reliably classify work, preserve the frontend/backend boundary, prevent
invented scope, route required contract terms, or enforce human approval before a
quote or signature.
