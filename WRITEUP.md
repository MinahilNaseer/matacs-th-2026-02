# WRITEUP — AI Proposal Triage Design

# 1. AI vs. rules
Use AI for document understanding: extract facts from varied proposal PDFs into a fixed schema, preserve short supporting quotations and page references where available, and mark missing or ambiguous values as `unknown`. Use ordinary code for file validation, mandatory pass/fail checks, arithmetic, weighted totals, sorting, and status transitions. Code is more suitable for deterministic compliance checks and arithmetic; AI is useful for interpreting varied natural-language descriptions. Treat all proposal text as untrusted input, never as system instructions.

# 2. Consistency and explainability
Use a versioned category configuration and a documented scoring rubric. Store the extracted fields, evidence snippets, extraction-model/prompt version, criteria version, component scores, total score, and run timestamp. Apply deterministic code to the same structured facts and rubric, and use stable tie-breaking (higher experience score, then lower price, then alphabetical proposal letter). AI-written explanations must be grounded in stored evidence and must not change scores. A reviewer sees each criterion's score, the evidence used, missing information, mandatory checks, total, and a plain-language reason. PDF extraction itself may vary, so repeatability means deterministic scoring for the same confirmed extracted facts; uncertain extraction is sent for review.

# Proposed scoring rubric
The brief provides criterion weights but not exact scoring formulas, so this design proposes the following explicit example for administrator approval:
- Experience (30%): `min(100, relevant qualifying installations / 15 × 100)`.
- Price (25%): among proposals that pass mandatory checks and have a confirmed feasible timeline, `lowest eligible price / proposal price × 100`. All prices are within the CAD $260,000 ceiling; a price above the ceiling would fail the price criterion and be reviewed under a published policy.
- Timeline (15%): `max(0, (16 − completion weeks) / 16 × 100)`. Missing or over-16-week timelines are not assigned an invented score.
- Warranty (15%): average of panel warranty score (`years / 25 × 100`, capped at 100) and workmanship warranty score (`years / 12 × 100`, capped at 100).
- Team and approach (15%): 25 points each for an explicitly named lead, named/defined crew, safety plan, and monitoring plan. Evidence not stated is not assumed.
- Total: `0.30E + 0.25P + 0.15T + 0.15W + 0.15A`.
The exact numerical caps and baselines should be approved by the admin before use; these are illustrative, not criteria supplied by MATACS.

# 3. Human in the loop and audit
A reviewer confirms extracted mandatory evidence and all low-confidence, missing, conflicting, or suspicious fields before ranking. A reviewer must resolve Proposal B's bidder-licence uncertainty and Proposal D's unknown schedule. Reviewers may override a result only with a reason. Log proposal identifier and file hash, extracted facts and evidence, model/prompt version, criteria version, component scores and formula version, flags, reviewer identity/action, override reason, timestamps, and final decision. The tool recommends; a human makes the award decision.

# 4. Changing criteria
Every scoring run references an immutable criteria version. An admin edit creates a new version and does not silently alter prior results. The reviewer can rerun all proposals against the new version; the UI should clearly label old and new results and preserve both audit records. If a change affects a live evaluation, require confirmation before recalculation.

# 5. Risks and safeguards
- **Incorrect PDF extraction/OCR:** validate file type and size; retain evidence snippets/page references; use confidence thresholds; route uncertain fields to human review.
- **Made-up facts:** require schema-constrained output; allow only extracted evidence; use `unknown` when absent; validate values and never infer licences or dates.
- **Prompt injection/manipulation:** treat documents as untrusted data; isolate proposal content from system instructions; ignore embedded requests to change ranking; flag suspicious text. Proposal C contains such an instruction and it must not affect the score.
- **Bias/inconsistent scoring:** use admin-approved criteria, fixed formulas, versioning, and periodic audits; do not use irrelevant personal or company characteristics.
- **Missing/ambiguous eligibility evidence:** do not silently pass it; flag for human confirmation. Proposal B has no confirmed licence held by the bidder in the supplied text; its subcontractor is “to be confirmed.” Proposal D has no completion schedule.
- **Cost and latency:** extract once and cache by file hash; use smaller/cheaper models for extraction where adequate; batch requests and set token/time limits; do not call AI for arithmetic.
- **Security/privacy:** restrict access, encrypt stored proposals, define retention/deletion rules, and never log secrets or expose proposal contents publicly.
- **False confidence in rankings:** show evidence and uncertainty, explain that scores are decision support, and require a human award decision.
