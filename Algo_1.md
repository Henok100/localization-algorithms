# Algorithm_1 — Centroid of the Intersection Area Using Monte Carlo Integration

**Purpose:** Estimate the centroid of the common intersection region of multiple anchor coverage circles using Monte Carlo integration.

## Inputs
- **Anchor set:** \( O = \{o_1, o_2, \ldots, o_n\} \), where \( n \geq 3 \)
- **Coverage radius:** \( R \)

## Output
- Estimated centroid \( \hat{p} \in \mathbb{R}^2 \)
- `null` if no common intersection exists

---

```text
1.  xmin ← max_i (o_i.x − R)
2.  xmax ← min_i (o_i.x + R)
3.  ymin ← max_i (o_i.y − R)
4.  ymax ← min_i (o_i.y + R)

5.  if xmin ≥ xmax or ymin ≥ ymax then
6.      return null        // Bounding boxes do not overlap
7.  end if

8.  Sx ← 0
9.  Sy ← 0
10. hits ← 0

11. for k ← 1 to 1000 do

12.     px ← Uniform(xmin, xmax)
13.     py ← Uniform(ymin, ymax)

14.     if for every o_i ∈ O:
            (px − o_i.x)² + (py − o_i.y)² ≤ R²
        then

15.         Sx ← Sx + px
16.         Sy ← Sy + py
17.         hits ← hits + 1

18.     end if

19. end for

20. if hits = 0 then
21.     return null        // Degenerate geometry
22. end if

23. return p̂ ← (Sx / hits, Sy / hits)
```
