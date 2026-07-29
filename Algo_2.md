# Algorithm 2 — Sliding-Window PDR Estimation and Distance Conversion

**Purpose:** Estimate the distance to each active anchor node by computing the Packet Delivery Ratio (PDR) over a sliding time window and converting the packet loss probability into distance.

## Inputs
- **Active anchor nodes:** $i$, each with a reception timestamp queue $T_i$
- **Current time:** $t$
- **Sliding-window length:** $W = 10\text{ s}$
- **Broadcast period:** $\tau = 250\text{ ms}$
- **Loss model coefficients:** $A, B$

## Output
- Estimated distance $\hat{d}_i$ for each active anchor node

---

```text
1.  c_max ← W / τ
    // Expected number of packets within the sliding window (40)

2.  for each active anchor i do

3.      while T_i ≠ ∅ and (t − first(T_i)) > W do
4.          discard first(T_i)
            // Remove expired timestamps
5.      end while

6.      PDR_i ← min(1, max(0, |T_i| / c_max))

7.      p_loss_i ← 1 − PDR_i

8.      d_hat_i ← (−B + √(B² + 4A * p_loss_i)) / (2A)
        // Inverse of Equation (4.3)

9.  end for

10. return {d_hat_i}
