# Chapter 12 Cheat Sheet: Exam-Day Time Management

## Time Budget Math
| Exam length | Questions | Avg time/question | First-pass target |
|---|---|---|---|
| 120 min | 50 | 2.4 min | ~100 sec |
| 120 min | 55 | 2.18 min | ~90 sec |
| 120 min | 60 | 2.0 min | ~80 sec |

Rule: answer every question on first pass (never blank), flag uncertain ones, bank the difference between your fast-pass time and the average for a final review buffer.

## Domain Coverage (All Domains, no single dominant weight)
Google's official exam guide does not publish exact per-domain percentages for this cert — treat all five domains as roughly equally likely to appear:
- Configuring access within a cloud solution environment
- Configuring network security
- Ensuring data protection
- Managing operations within a cloud solution environment
- Ensuring compliance

Don't over-invest study time assuming one domain dominates — a full-length simulation should sample all five.

## Keyword Decision Table
| Keyword | What it signals | Action |
|---|---|---|
| BEST | Multiple valid options exist | Check which satisfies ALL stated constraints |
| MOST | Ranking a single property upward | Find the highest-degree match |
| LEAST | Ranking a single property downward | Find the lowest-degree match (e.g., least privilege, least cost) |
| ONLY / NEVER | Absolute constraint | Eliminate anything with exceptions |

## Elimination Fast-Path
1. Cut deprecated/legacy services immediately.
2. Cut options that violate a stated hard constraint (cost cap, compliance requirement, managed-service-only).
3. Cut options requiring manual/unenforced processes when an automated control exists.
4. Between the remaining 2, re-read the requirement sentence for the differentiator.

## Flagging Strategy
- Flag if: narrowed to 2 plausible options, or answer depends on a specific memorized number/limit you're unsure of.
- Never leave a flagged question blank — submit your best guess, flag, move on.
- Target: flag no more than 15-20% of questions to keep review pass manageable.

## Final Review Pass (last 10-15 min)
1. Fill any remaining blanks first.
2. Revisit ONLY flagged questions — don't re-open confidently answered ones.
3. Re-verify keyword (BEST/MOST/LEAST) only on flagged items.
4. Stop reviewing once time runs low rather than rushing new changes — first instinct is usually correct.

## Common Traps in Full Simulation
- Multi-part scenario questions: read the FULL scenario before jumping to the last sentence — earlier constraints often eliminate 2 answers.
- Distractors that are "technically possible but manual" (e.g., manual key rotation, manual IP removal) vs. the automated/managed answer.
- Confusing always-on logs (Admin Activity) with opt-in logs (Data Access).
