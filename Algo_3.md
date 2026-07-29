## Algorithm 2: Least Squares Multilateration

**Input**
- Active anchors $(x_i,y_i)$ with packet count $N_i$ ($i=1,\dots,n$), maximum expected count $N_{\max}$, range $d_{\max}$, fallback centroid $\hat{p}_0$ (Algorithm 1)

**Output**
- Estimated position $(X,Y)$, consistent with the feasibility region

**Procedure**

1. **for** $i\leftarrow 1$ **to** $n$ **do**
   1. $\widehat{\mathrm{PDR}}_i \leftarrow \min\!\big(1,\,\max(0,\,N_i/N_{\max})\big)$
   2. $\hat{d}_i \leftarrow$ distance obtained from $\widehat{\mathrm{PDR}}_i$ according to Eq. (1)
2. $g_i \leftarrow x_i^2 + y_i^2 - \hat{d}_i^2$ *(for all $i$)*
3. $\bar{x},\bar{y},\bar{g} \leftarrow$ means of $\{x_i\},\{y_i\},\{g_i\}$
4. $S_{aa},S_{ab},S_{bb},S_{ac},S_{bc} \leftarrow 0$
5. **for** $i\leftarrow 1$ **to** $n$ **do**
   1. $a \leftarrow 2(x_i-\bar{x})$; $b \leftarrow 2(y_i-\bar{y})$; $c \leftarrow g_i-\bar{g}$
   2. $S_{aa}\leftarrow S_{aa}+a^2$; $S_{ab}\leftarrow S_{ab}+ab$; $S_{bb}\leftarrow S_{bb}+b^2$
   3. $S_{ac}\leftarrow S_{ac}+ac$; $S_{bc}\leftarrow S_{bc}+bc$
6. $\Delta \leftarrow S_{aa}S_{bb}-S_{ab}^2$ *(determinant of the normal equations)*
7. **if** $|\Delta| < \varepsilon$ **then**
   - **return** $\hat{p}_0$ *(collinear / degenerate geometry)*
8. $X \leftarrow (S_{bb}S_{ac}-S_{ab}S_{bc})/\Delta$; $Y \leftarrow (S_{aa}S_{bc}-S_{ab}S_{ac})/\Delta$
9. **if** $(X,Y)$ is at distance $\leq d_{\max}$ from all anchors **then**
   - **return** $(X,Y)$ *(already feasible)*
10. $lo \leftarrow 0$; $hi \leftarrow 1$ *(projection to the feasibility region)*
11. **for** $k\leftarrow 1$ **to** $30$ **do**
    1. $t \leftarrow (lo+hi)/2$; $q \leftarrow \hat{p}_0 + t\,\big((X,Y)-\hat{p}_0\big)$
    2. **if** $q$ is at distance $\leq d_{\max}$ from all anchors **then**
       - $lo \leftarrow t$
    3. **else**
       - $hi \leftarrow t$
12. **return** $\hat{p}_0 + lo\,\big((X,Y)-\hat{p}_0\big)$
