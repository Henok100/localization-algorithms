## Algorithm 2: Sliding-Window PDR Estimation and Distance Conversion

**Input**

- Active anchors $i$ with reception timestamp queue $T_i$
- Current time $t$
- Window length $W = 10\,\mathrm{s}$
- Broadcast period $\tau = 250\,\mathrm{ms}$
- Loss-model coefficients $A$ and $B$

**Output**

- Estimated distance $\hat{d}_i$ for each active anchor

1. Compute

   $$
   c_{\max} \leftarrow \frac{W}{\tau}
   $$

2. For each active anchor $i$:

   1. While $T_i \neq \emptyset$ and $t-\operatorname{first}(T_i)>W$:

      - Remove $\operatorname{first}(T_i)$ from $T_i$.

   2. Compute

      $$
      \widehat{\mathrm{PDR}}_i
      \leftarrow
      \min\left(
      1,
      \max\left(
      0,
      \frac{|T_i|}{c_{\max}}
      \right)
      \right)
      $$

   3. Compute

      $$
      \hat{p}_{\mathrm{loss},i}
      \leftarrow
      1-\widehat{\mathrm{PDR}}_i
      $$

   4. Compute

      $$
      \hat{d}_i
      \leftarrow
      \frac{-B+\sqrt{B^2+4A\hat{p}_{\mathrm{loss},i}}}{2A}
      $$

3. Return $\{\hat{d}_i\}$.
