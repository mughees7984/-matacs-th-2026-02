# MATACS Technical Assessment AI Proposal Triage

**Full Name:** Muhammad Mughees Arif  
**Assessment Reference:** MATACS-TH-2026-02  
**Actual Time Spent:** Approximately 2 hours

## Project Overview

This repository contains my proposed design for an AI-assisted proposal triage system for the Commercial Solar Installation scenario.

The system extracts information from submitted proposals, validates mandatory requirements, calculates weighted scores, and generates a ranked list for human review.

## Repository Contents

- `diagram/` — Process flow diagram showing the complete workflow.
- `WRITEUP.md` — Answers to the five assessment questions.
- `PROMPTS.md` — AI prompts used for proposal extraction and explanation.
- `ranking.json` — Evaluation results for proposals A, B, C, D, and E.

## AI Usage

I used ChatGPT to help structure the system design, identify potential edge cases, draft AI prompts, and improve the clarity of the documentation.

I reviewed the suggestions against the assessment requirements and made decisions about which responsibilities should belong to AI, backend code, and human reviewers.

**Example of a correction:** An initial suggestion could treat a licensed subcontractor as equivalent to the bidder holding the required electrical contractor licence. I did not accept that assumption because the assessment explicitly requires the bidder itself to hold and state its Ontario electrical contractor licence. Therefore, Proposal B is flagged for clarification rather than automatically marked as eligible.

## Scoring Assumptions

The assessment provides the weighted criteria but does not specify exact scoring formulas. The proposed scoring formulas and assumptions are documented in `WRITEUP.md` and `ranking.json`.

Flagged proposals are not assigned a final rank or score until their outstanding review requirements are resolved.

## Important Design Principles

- AI extracts information but does not make the final award decision.
- Backend code handles mandatory checks and deterministic scoring.
- Missing or uncertain information is not invented.
- Human reviewers can verify and correct extracted information.
- Criteria and evaluation results are versioned for auditability.
- Final award decisions remain under human control.
