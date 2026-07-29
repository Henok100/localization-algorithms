## Algorithm 4: Sliding-Window PDR Estimation and Distance Conversion

**Input**
- Active anchors $i$ with reception timestamp queue $T_i$; current timestamp $t$; window length $W=10\text{ s}$; broadcast period $\tau=250\text{ ms}$; coefficients $A,B$ of the loss model

**Output**
- Estimated distance $\hat{d}_i$ for each active anchor

**Procedure**

1. $c_{\max} \leftarrow W / \tau$ *(expected packets in the window ($40$))*
2. **for** each active anchor $i$ **do**
   1. **while** $T_i \neq \emptyset$ **and** $t - \mathrm{first}(T_i) > W$ **do**
      - discard $\mathrm{first}(T_i)$ *(sliding-window pruning)*
   2. $\widehat{\mathrm{PDR}}_i
   \leftarrow
   \min\left(
   1,
   \max\left(
   0,
   \frac{|T_i|}{c_{\max}}
   \right)
   \right)$

3. $\hat{p}_{\mathrm{loss},i}
   \leftarrow
   1-\widehat{\mathrm{PDR}}_i$

4. $\hat{d}_i
   \leftarrow
   \frac{-B+\sqrt{B^2+4A\hat{p}_{\mathrm{loss},i}}}{2A}$
3. **return** $\{\hat{d}_i\}$
