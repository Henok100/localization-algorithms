# Algorithm 6: Route Smoothing Using a Kalman Filter (One Cycle)

**Purpose:** Smooth the estimated UAV route by filtering noisy position observations using a discrete Kalman filter. The algorithm performs one prediction–correction cycle using the latest accepted position estimate.

## Inputs

- **Observation:**

  $$
  z = (z_X, z_Y)
  $$

  after the plausibility filter

- **Time step:** $\Delta t$ since the last accepted observation
- **State vector:** $x$
- **Covariance matrix:** $P$
- **Process noise parameter:** $q$
- **Measurement noise parameter:** $r$
- **Initial variances:**
  - Position variance: $p_0$
  - Velocity variance: $v_0$

## Output
- Smoothed position:
  \[
  (X,Y)
  \]
- Updated state vector and covariance matrix

---

1.  if filter is not initialized then

2.    x ← (zX, zY, 0, 0)

3.    P ← diag(p0, p0, v0, v0)
        // Initialization using bootstrap median

4.    return (zX, zY)

5.  end if


6.  Construct F and Q from Δt
    // State transition and process noise matrices
    // According to Equation (4.8)


7.  x ← F x

8.  P ← FPFᵀ + Q
    // Prediction step


9.  y ← z − Hx
    // Innovation


10. S ← HPHᵀ + R


11. K ← PHᵀS⁻¹
    // Kalman gain


12. x ← x + Ky

13. P ← (I − KH)P
    // Correction step


14. return (X, Y) ← (x₁, x₂)
