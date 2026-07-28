# Algorithm 4 — Leader Navigation and Waypoint Arrival Detection

**Purpose:** Navigate the leader UAV toward a target waypoint while robustly detecting waypoint arrival using smoothed position estimates and handling prolonged loss of localization.

## Inputs
- **Target waypoint:** \(w\)
- **Nominal segment origin:** \(s\)
- **Navigation speed:** \(v\)
- **Minimum active flight time:** \(t_{\min}\) (0.9 of the nominal duration)
- **Maximum active flight time:** \(t_{\max}\) (1.2 of the nominal duration)
- **Proximity threshold:** \(\rho = 250\,\text{m}\)
- **Confirmation samples:** \(N_a = 6\)
- **EMA smoothing factor:** \(\alpha = 0.30\)
- **Control period:** \(\Delta t\)
- **Emergency timeout threshold:** \(T_{em} = 70\,\text{s}\)

## Output
- Leader reaches the waypoint, or aborts due to prolonged absence of valid position estimates

---
1.  û ← (w − s) / ||w − s||
    // Planned segment direction

2.  near ← 0
    tact ← 0
    started ← false

3.  while true do

4.      p ← last published estimated position

5.      if p = null then

6.          if time without estimation ≥ Tem then
7.              return abort
                // Emergency stop
8.          end if

9.          wait Δt
10.         continue

11.     end if

12.     if not started then
13.         p̃ ← p
14.         started ← true
15.     else
16.         p̃ ← p̃ + α(p − p̃)
            // Exponential Moving Average (EMA)
17.     end if

18.     progress ← (p̃ − s) · û
19.     remaining ← ||w − s|| − progress

20.     if tact ≥ tmax then
21.         return arrival
            // Maximum flight time reached
22.     end if

23.     if tact ≥ tmin and remaining ≤ ρ then
24.         near ← near + 1

25.         if near ≥ Na then
26.             return arrival
                // Waypoint arrival confirmed
27.         end if

28.     else
29.         near ← 0
30.     end if

31.     Navigate toward w at speed v
        using the raw estimate p

32.     tact ← tact + Δt
        // Advance active flight time

33.     wait Δt

34. end while
