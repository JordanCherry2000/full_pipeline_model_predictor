# DECISIONS.md

## 2024-11-14 — Threshold selection at 0.35 rather than 0.50

**Context:** Default sklearn threshold of 0.50 produced precision of 0.79
and recall of 0.61 on the validation set.

**Options considered:**
- Keep 0.50 (simpler, conventional)
- Use Youden's J optimal threshold (0.31 — maximizes TPR - FPR)
- Use business-informed threshold based on precision/recall tradeoff

**Decision:** Set threshold at 0.35.

**Reasoning:** At 0.35, recall improves to 0.74 while precision drops to
0.71. In a credit default context the cost of a missed default (false
negative) substantially exceeds the cost of an unnecessary review (false
positive). The 0.31 Youden threshold was too aggressive — precision
dropped below 0.65 which would generate an unworkable volume of false
alarms in a real system.

**Consequences:** All downstream metrics and the fairness audit use this
threshold. If deployed in a context where false positive costs are higher
(e.g. automated rejection rather than manual review), the threshold should
be re-evaluated.

---
