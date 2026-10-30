# AI Proposal Triage — Design Write-up

**Assessment Reference:** MATACS-TH-2026-02

## 1. What should AI do, and what should be ordinary code?

AI should extract structured information from proposal PDFs, including company details, electrical contractor licence, WSIB clearance, insurance coverage, relevant project experience, price, timeline, warranties, team details, safety plan, and monitoring plan.

AI should also provide supporting evidence and page references for extracted information. It must not invent missing information or follow instructions embedded inside proposal documents.

Ordinary backend code should handle JSON validation, mandatory requirement checks, weighted score calculations, ranking, and result generation. These operations should be deterministic so that identical inputs and criteria produce consistent results.

## 2. How will results be consistent, explainable, and repeatable?

Every evaluation should store the extracted information, evidence references, AI model, prompt version, criteria version, and scoring results.

The backend should use fixed scoring formulas and rounding rules. The proposed scoring approach is:

- **Experience (30%):** Score based on relevant commercial solar installations of at least 100 kW completed within the last five years. Proposed formula: `min(number of qualifying installations / 10, 1) × 100`.
- **Price (25%):** Lower eligible bid receives a better score. Proposed formula: `(lowest eligible price / proposal price) × 100`.
- **Timeline (15%):** Shorter completion time receives a better score. Proposed formula: `((16 - completion weeks) / 16) × 100` for schedules within 16 weeks.
- **Warranty (15%):** Based on panel and workmanship warranty durations, using 25 years and 10 years respectively as full-score reference points.
- **Team/Approach (15%):** Four equally weighted elements: named project lead, identified crew, safety plan, and monitoring plan.

Each criterion score is multiplied by its weight, and the weighted scores are added to produce the final score.

These detailed formulas are proposed design assumptions because the assessment provides weights but does not define exact scoring formulas.

## 3. Where should human reviewers be involved?

Human review should be required when information is missing, ambiguous, contradictory, or has low extraction confidence.

Reviewers should verify mandatory requirements and supporting evidence before accepting extracted information. They should also review suspicious document content, such as instructions attempting to manipulate the AI evaluation.

Reviewers may correct extracted values, but every correction must include a reason, reviewer identity, and timestamp. The original extracted value must remain available for auditing.

The system should provide recommendations, not automatically award the contract. Final approval must remain with a human reviewer.

## 4. What happens if an administrator changes criteria after scoring?

The system should use versioned criteria.

When an administrator changes scoring weights or requirements, the system creates a new draft criteria version. Before activation, it validates that weighted criteria total 100% and that mandatory requirements are configured separately.

Previous evaluations remain linked to their original criteria version and must not change automatically.

If the administrator wants to evaluate previous proposals using updated criteria, the system creates a new evaluation using the saved extracted information and the new criteria version. Both evaluations remain available for comparison and auditing.

## 5. What could go wrong, and how would it be handled?

- **Incorrect AI extraction:** Store supporting evidence and page references, validate structured output, and require human verification where necessary.
- **Missing information:** Store missing values as null and flag the proposal instead of making assumptions.
- **Prompt injection:** Treat proposal documents as untrusted input. Ignore embedded instructions and log suspicious content for human review.
- **Inconsistent scoring:** Use deterministic backend formulas, fixed rounding rules, and versioned criteria.
- **Unfair scoring:** Use documented, objective criteria and allow reviewers to inspect and challenge results.
- **Document processing failures:** Validate uploaded files, use background processing with retries, and record failed processing attempts.
- **Unauthorized access:** Use role-based permissions, secure file storage, encryption, and audit logs.
- **Criteria changes during evaluation:** Keep each evaluation linked to one immutable criteria version.

The system should maintain an audit trail of uploads, extractions, corrections, criteria changes, scoring results, and final approval decisions.
