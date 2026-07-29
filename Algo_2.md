## Algorithm 2: Sliding-Window PDR Estimation and Distance Conversion

**Input**

- Active anchors $i$ with reception timestamp queue $T_i$
- Current time $t$
- Window length $W = 10s$
- Broadcast period $\tau = 250ms$
- Loss model coefficients $A$ and $B$

**Output**

- Estimated distance $\hat{d}_i$ for each active anchor

1. Compute

$$
c_{\max} = \frac{W}{\tau}
$$

2. For each active anchor $i$:

   1. While $T_i \neq \emptyset$ and $t-\text{first}(T_i)>W$:

      - Remove $\text{first}(T_i)$ from $T_i$.

   2. Compute

$$
\widehat{PDR}_i =
\min\left(1,\max\left(0,\frac{|T_i|}{c_{\max}}\right)\right)
$$

   3. Compute

$$
\hat{p}_{loss,i}=1-\widehat{PDR}_i
$$

   4. Compute

$$
\hat{d}_i=
\frac{-B+\sqrt{B^2+4A\hat{p}_{loss,i}}}{2A}
$$

3. Return $\{\hat{d}_i\}$.
