# Fuel-Efficient Free-Final-Time Guidance for Terrain-Avoided Precision Soft Landing

> **Abhishek Barad and Satadal Ghosh.**
> "A Fuel-Efficient Free-Final-Time Guidance for Terrain-Avoided Precision Soft Landing of Spacecraft."
> *2025 IEEE 64th Conference on Decision and Control (CDC).* IEEE, 2025.
> DOI: [10.1109/CDC57313.2025.11313021](https://doi.org/10.1109/CDC57313.2025.11313021)

---

## 📌 Problem Statement

Achieving a **safe, fuel-efficient, and precise soft landing** on a planetary surface containing hazardous terrain is one of the most critical challenges in space exploration. The spacecraft must autonomously reach its designated landing site with high precision (typically within 3 m), with near-zero touchdown velocity, while simultaneously:

- **Avoiding hazardous terrain** that surrounds the landing zone
- **Satisfying thrust constraints** — thrusters must remain active once ignited and cannot operate below a minimum thrust level (i.e., T_min > 0)
- **Minimizing fuel consumption** throughout the descent

The governing dynamics of the powered descent are:

```
ṙ = v,    v̇ = g + a,    ṁ = −‖T‖ / (Isp · ge)
```

where **r** = [x, y, z]ᵀ is position, **v** is velocity, **g** is Martian gravity, **a** is the guidance acceleration command, and **T = ma** is the thrust vector. The thrust magnitude is bounded as:

```
T_min ≤ ‖T(t)‖ ≤ T_max,    T_min > 0
```

### Why is this hard?

Traditional guidance approaches fall short in the following ways:

| Limitation | Effect |
|---|---|
| **Fixed final time (too short)** | Requires aggressive thrusting → excess fuel burn |
| **Fixed final time (too long)** | Unnecessarily prolonged descent → excess fuel burn |
| **Convex optimization methods** | Computationally expensive for real-time onboard use |
| **Most terrain-aware methods** | Assume fixed final time; not fuel-optimal |

There is thus a need for a guidance strategy that **jointly handles terrain avoidance, thrust constraints, and free-final-time fuel optimization**, while remaining **computationally lightweight** for onboard real-time implementation.

---

## 💡 Proposed Solution

This paper presents a **two-stage guidance strategy**:

**Stage 1 — Fixed-Final-Time Guidance:**
Hazardous terrain is modeled using a step-shaped piecewise linear approximation. A virtual **barrier function** is constructed above this terrain with horizontal (ρᵢ) and vertical (ρ_z) components. A closed-form sub-optimal guidance law is derived using optimal control principles, with an **exponential penalty** on the distance to the barrier embedded in the cost function — strongly discouraging near-barrier trajectories and enforcing strict terrain avoidance.

**Stage 2 — Free-Final-Time Optimization:**
The fixed-final-time guidance is embedded into an outer-loop optimization that searches for the **fuel-optimal final time** using the **Golden Section Line Search (GSLS)** algorithm — a fast, derivative-free, one-dimensional search. This eliminates the need to pre-specify descent duration, enabling near-fuel-optimal trajectories across varied initial conditions.

---

## 🚀 Interactive 3D Barrier Playground

Open [`3D_barrier_generation_playground.html`](./3D_barrier_generation_playground.html) directly in any browser to explore the barrier construction interactively — no installation required.

**Features:**
- 4 construction stages: terrain sketch → step approximation → apex shifting → 3D barrier surface
- Live readout of lateral & vertical clearances with safe/unsafe status
- Drag to orbit, scroll to zoom
- Keyboard shortcuts: `1`–`4` stages · `WASD/QE` lander · `R` reset · `?` help

**Or visit the live hosted version:**
[🔗 Open Playground in Browser](https://raj-abhishek-barad.github.io/fuel-efficient-guidance-terrain-avoided-landing/3D_barrier_generation_playground.html)

---

## 🎬 Simulation Animation

![ISRO Project Animation](isro_proj_animation.gif)

The animation shows **three spacecraft landing trajectories** (Cases 1–3) in the X-Z and Y-Z planes, each starting from different initial positions and velocities, all successfully avoiding hazardous terrain and achieving precision soft landing under the proposed free-final-time guidance law.

| Case | Initial Position r₀ (m) | Initial Velocity v₀ (m/s) | Final Time (s) | Fuel Spent (kg) |
|------|--------------------------|---------------------------|----------------|-----------------|
| 1 | [−1539.4, −619.6, 2083.3] | [16.8, −14.1, −85.4] | 68.821 | 274.523 |
| 2 | [669.4, −819.6, 2883.3] | [10.0, −28.5, −105.0] | 83.516 | 153.804 |
| 3 | [900.0, 800.0, 2200.0] | [15.0, 25.0, −80.0] | 33.335 | 187.460 |

---

## 📊 Key Results

- ✅ Successful terrain avoidance across **500 Monte Carlo trials** with randomized initial conditions and thrust perturbations
- ✅ Precision landing within **0.5 m** of target with near-zero terminal velocity
- ✅ **Lower fuel consumption** than OTALG [Basar & Ghosh, 2023] across all fixed-final-time settings tested (25 s, 50 s, 75 s, 100 s, 125 s)
- ✅ Optimal final time adapts between **35.3 s and 94.8 s** (median 78.5 s) across varied initial conditions

---

## 📄 Citation

```bibtex
@inproceedings{barad2025fuel,
  title     = {A Fuel-Efficient Free-Final-Time Guidance for Terrain-Avoided Precision Soft Landing of Spacecraft},
  author    = {Barad, Abhishek and Ghosh, Satadal},
  booktitle = {2025 IEEE 64th Conference on Decision and Control (CDC)},
  year      = {2025},
  publisher = {IEEE},
  doi       = {10.1109/CDC57313.2025.11313021},
  url       = {https://doi.org/10.1109/CDC57313.2025.11313021}
}
```

---

*Aerospace Engineering Department, IIT Madras*
*ae23s022@smail.iitm.ac.in · satadal@iitm.ac.in*
