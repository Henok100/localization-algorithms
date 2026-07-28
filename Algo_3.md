# Algorithm_3 — Least-Squares Multilateration

**Purpose:** Estimate the node position using least-squares multilateration based on PDR-derived distances, with a projection step to ensure the solution remains within the feasible communication region.

## Inputs
- **Active anchor nodes:** \((x_i, y_i)\), \(i = 1, \ldots, n\)
- **Packet counts:** \(N_i\)
- **Maximum expected packet count:** \(N_{\max}\)
- **Maximum communication range:** \(\hat{d}_{\max}\)
- **Backup centroid estimate:** \(\hat{p}_0\) (from Algorithm 4.1)

## Output
- Estimated position \((X, Y)\) consistent with the feasibility region

---
1.  for each anchor i = 1 to n do

2.      PDRi ← min(1, max(0, Ni / Nmax))

3.      d̂i ← distance obtained from PDRi
        // Using Equation (4.3)

4.  end for

5.  gi ← xi² + yi² − d̂i²        (for all i)

6.  Compute:
        x̄ ← mean({xi})
        ȳ ← mean({yi})
        ḡ ← mean({gi})

7.  Saa ← 0
    Sab ← 0
    Sbb ← 0
    Sac ← 0
    Sbc ← 0

8.  for each anchor i = 1 to n do

9.      a ← 2(xi − x̄)
        b ← 2(yi − ȳ)
        c ← gi − ḡ

10.     Saa ← Saa + a²
        Sab ← Sab + a·b
        Sbb ← Sbb + b²

11.     Sac ← Sac + a·c
        Sbc ← Sbc + b·c

12. end for

13. Δ ← Saa·Sbb − Sab²
    // Determinant of the normal equations

14. if |Δ| < ε then
15.     return p̂0
        // Collinear or degenerate geometry
16. end if

17. X ← (Sbb·Sac − Sab·Sbc) / Δ
    Y ← (Saa·Sbc − Sab·Sac) / Δ

18. if (X, Y) is within d̂max of all anchor nodes then
19.     return (X, Y)
        // Solution is already feasible
20. end if

21. lo ← 0
    hi ← 1
    // Projection onto the feasibility region

22. for k ← 1 to 30 do

23.     t ← (lo + hi) / 2

24.     q ← p̂0 + t((X, Y) − p̂0)

25.     if q is within d̂max of all anchor nodes then
26.         lo ← t
27.     else
28.         hi ← t
29.     end if

30. end for

31. return p̂0 + lo((X, Y) − p̂0)
