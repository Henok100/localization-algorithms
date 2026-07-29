## Algorithm 1: Intersection Area Centroid via Monte Carlo Integration

**Input**
- Anchors in range $\mathcal{O}=\{o_1,\dots,o_n\}$ ($n\geq 3$), coverage radius $R$

**Output**
- Estimated centroid $\hat{p}\in\mathbb{R}^2$, or `null` if there is no intersection

**Procedure**

1. $x_{\min}\leftarrow\max_i(o_i.x-R)$; $x_{\max}\leftarrow\min_i(o_i.x+R)$
2. $y_{\min}\leftarrow\max_i(o_i.y-R)$; $y_{\max}\leftarrow\min_i(o_i.y+R)$
3. **if** $x_{\min}\geq x_{\max}$ **or** $y_{\min}\geq y_{\max}$ **then**
   - **return** `null` *(the bounding boxes do not overlap)*
4. $S_x\leftarrow 0$; $S_y\leftarrow 0$; $hits\leftarrow 0$
5. **for** $k\leftarrow 1$ **to** $1000$ **do**
   1. $p_x\sim\mathcal{U}(x_{\min},x_{\max})$; $p_y\sim\mathcal{U}(y_{\min},y_{\max})$
   2. **if** $\forall\,o_i\in\mathcal{O}:\;(p_x-o_i.x)^2+(p_y-o_i.y)^2\leq R^2$ **then**
      - $S_x\leftarrow S_x+p_x$; $S_y\leftarrow S_y+p_y$; $hits\leftarrow hits+1$
6. **if** $hits=0$ **then**
   - **return** `null` *(degenerate geometry)*
7. **return** $\hat{p}=\left(S_x/hits,\;S_y/hits\right)$
