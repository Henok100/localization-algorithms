# Algorithm_5 — Plausibility Filter for Position Estimation

**Purpose:** Reject physically implausible position estimates caused by transient localization errors while maintaining the last valid estimate. The filter uses a kinematic constraint based on the leader velocity and applies a buffer-based recovery mechanism during prolonged estimation failures.

## Inputs
- **Candidate position estimate:** \(p\)
- **Current time:** \(t\)
- **Leader velocity:** \(v\)
- **State variables:**
  - Reference position \(p_{\text{ref}}\) (last accepted estimate)
  - Last acceptance time \(t_{\text{ref}}\)
  - Initialization buffer \(B\)
  - Rejection buffer \(R\)

## Output
- Published position estimate or maintained last valid estimate
- Updated filter state

---

1.  if p_ref = null then

2.      Add p to B
        // Reference initialization (calibration phase)

3.      if |B| < Nmin then
4.          return maintain without publishing
5.      end if

6.      p_ref ← median(B)
7.      t_ref ← t

8.      publish(p_ref)

9.      return

10. end if


11. Δt ← t − t_ref

12. maxJump ← E + m·v·Δt
    // Maximum physically plausible displacement


13. if ||p − p_ref|| ≤ maxJump then

14.     p_ref ← p
15.     t_ref ← t

16.     clear R

17.     publish(p)
        // Plausible position update

18.     return

19. end if


20. Add (t, p) to R

21. Prune R to retain only the last Tc ms

22. Maintain last valid estimate
    // Current cycle recorded as without valid estimation


23. if ((t − t_ref ≥ Tc and σR ≤ D) 
        or (t − t_ref ≥ Ttimeout)) then

24.     p_ref ← median(R)

25.     t_ref ← t

26.     clear R
        // Reference reset

27. end if
