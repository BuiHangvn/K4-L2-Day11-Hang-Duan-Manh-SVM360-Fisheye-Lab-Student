# L / R / M

L/R/M là thứ tự box in-scope theo XML của từng frame.

## adasind_069450.jpg
- L3+R4+M4: LRM (center)
- L4+R5+M6: LRM (center)
- L5+R1: LR_noM (edge)
- L1+R2: LR_noM (mid)
- L2+R3+M3: LRM (center)
- M5: M_only (edge)
- M7: M_only (mid)
## adasind_082170.jpg
- L2+R1: LR_noM (mid)
- L7+R8+M3: LRM (mid)
- L3+R7: LR_noM (mid)
- L4+R3+M1: LRM (center)
- L5+R2: LR_noM (center)
- L1+R4: LR_noM (edge)
- L6+R6+M10: LRM (edge)
- R5: R_only (edge)
- M2: M_only (mid)
- M5: M_only (mid)
- M6: M_only (center)
- M7: M_only (center)
- M9: M_only (mid)
## adasind_102750.jpg
- L2+R1+M1: LRM (mid)
- L1+R3: LR_noM (mid)
- L5+R4: LR_noM (edge)
- L3: L_only (mid)
- L4: L_only (edge)
- L6: L_only (center)
- R2+M2: RM_noL (edge)
- R5+M8: RM_noL (center)
- M3: M_only (mid)
- M4: M_only (mid)
- M5: M_only (mid)
- M6: M_only (center)
- M7: M_only (mid)

## Zone × cell
| zone | LRM | LR_noM | LM_noR | L_only | RM_noL | R_only | M_only |
|---|---|---|---|---|---|---|---|
| center | 4 | 1 | 0 | 1 | 1 | 0 | 3 |
| mid | 2 | 4 | 0 | 1 | 0 | 0 | 8 |
| edge | 1 | 3 | 0 | 1 | 1 | 1 | 1 |
