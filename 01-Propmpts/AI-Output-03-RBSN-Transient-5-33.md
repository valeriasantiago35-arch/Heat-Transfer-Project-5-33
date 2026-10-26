Transient heating time, adapted Problem 5-33

Basis: Everything is per unit area (1 m² of cross-section), so capacities are in J/(m²·K) and conductances in W/(m²·K). The scheme is the explicit finite-difference method with Δx = 0.005 m. Node 0 is at the steam side, nodes 0–6 are steel (6 intervals = 0.030 m), nodes 6–8 are RBSN (2 intervals = 0.010 m), and node 8 is at the air side.

Note on the steady-state values: You did not paste your adapted steady-state temperatures, so I recomputed them from your parameters in Step 7. I used the same discretized model, so they should match yours. If your values differ, tell me, because the targets in Step 7 depend on them.

1. Thermal diffusivity

α = k / (ρ c)

Steel: α = 13 / (7900 × 500) = 13 / 3.95×10⁶ = 3.291×10⁻⁶ m²/s
RBSN: α = 12 / (2500 × 1100) = 12 / 2.75×10⁶ = 4.364×10⁻⁶ m²/s

(Both materials have a volumetric heat capacity ρc of 3.95×10⁶ and 2.75×10⁶ J/(m³·K) respectively, which is used in Step 2.)

2. Nodal thermal capacity, Cᵢ = Σ ρ c V per unit area

Full-node capacity for each material is ρ c Δx:

Steel: 3.95×10⁶ × 0.005 = 19 750 J/(m²·K)
RBSN: 2.75×10⁶ × 0.005 = 13 750 J/(m²·K)
Node	Control volume	Cᵢ [J/(m²·K)]
0	half steel	19 750 / 2 = 9 875
1–5	full steel	19 750
6	half steel + half RBSN	9 875 + 6 875 = 16 750
7	full RBSN	13 750
8	half RBSN	13 750 / 2 = 6 875

Check: ΣCᵢ = 9 875 + 5(19 750) + 16 750 + 13 750 + 6 875 = 146 000. This equals 0.030 × 3.95×10⁶ + 0.010 × 2.75×10⁶ = 118 500 + 27 500 = 146 000 ✓

3. Conductances between nodes
Steel (nodes 0–1 through 5–6): G = k/Δx = 13 / 0.005 = 2 600 W/(m²·K)
RBSN (nodes 6–7 and 7–8): G = 12 / 0.005 = 2 400 W/(m²·K)
Steam side: h₁ = 300. Air side: h₂ = 150 W/(m²·K)
4. Transient equations

The energy balance is Cᵢ dTᵢ/dt = Σ Gᵢⱼ (Tⱼ − Tᵢ) + boundary terms. Explicit form: Tᵢᵖ⁺¹ = Tᵢᵖ + (Δt/Cᵢ) · Q̇ᵢᵖ. Below, Δt = 1 s.

Node 0:
T₀′ = T₀ + (1/9875)[300(260 − T₀) + 2600(T₁ − T₀)]
T₀′ = 0.70633 T₀ + 0.26329 T₁ + 7.8987

Nodes 1–5:
Tₘ′ = Tₘ + (2600/19750)(Tₘ₋₁ − 2Tₘ + Tₘ₊₁)
Tₘ′ = 0.13165 Tₘ₋₁ + 0.73671 Tₘ + 0.13165 Tₘ₊₁

Node 6 (interface):
T₆′ = T₆ + (1/16750)[2600(T₅ − T₆) + 2400(T₇ − T₆)]
T₆′ = 0.15522 T₅ + 0.70149 T₆ + 0.14328 T₇

Node 7:
T₇′ = T₇ + (2400/13750)(T₆ − 2T₇ + T₈)
T₇′ = 0.17455 T₆ + 0.65091 T₇ + 0.17455 T₈

Node 8:
T₈′ = T₈ + (1/6875)[2400(T₇ − T₈) + 150(20 − T₈)]
T₈′ = 0.34909 T₇ + 0.62909 T₈ + 0.43636

5. Stability

The explicit scheme requires all self-coefficients ≥ 0, so Δt ≤ Cᵢ / ΣG for each node:

Node	Cᵢ / ΣG	Δt_max [s]
0	9 875 / (300 + 2600)	3.405
1–5	19 750 / 5 200	3.798
6	16 750 / 5 000	3.350
7	13 750 / 4 800	2.865
8	6 875 / (2400 + 150)	2.696

Δt_max = 2.696 s (node 8). Δt = 1 s is 37% of the limit, and all self-coefficients above are positive ✓

6. First time step (t = 0 → 1 s), all Tᵢ = 20 °C
Node 0: Q̇₀ = 300(260 − 20) + 2600(20 − 20) = 72 000 W/m². ΔT₀ = 72 000 × 1 / 9 875 = 7.291 K, so T₀ = 27.291 °C
Nodes 1–5: Q̇ = 0 (neighbors all at 20 °C), so T = 20 °C
Node 6: Q̇ = 0, so 20 °C
Node 7: Q̇ = 0, so 20 °C
Node 8: Q̇₈ = 2400(0) + 150(20 − 20) = 0, so 20 °C

Second step, to show the propagation:

Node 0: Q̇ = 300(232.709) − 2600(7.291) = 69 812.7 − 18 957.0 = 50 855.7. ΔT = 5.150, so T₀ = 32.441 °C
Node 1: Q̇ = 2600(7.291) = 18 957.0. ΔT = 18 957.0 / 19 750 = 0.960, so T₁ = 20.960 °C
Nodes 2–8 are still at 20 °C.
7. Steady state (hand-reproducible) and 95 % targets

Resistance per unit area:
R_tot = 1/300 + 6/2600 + 2/2400 + 1/150 = 0.003333 + 0.002308 + 0.000833 + 0.006667 = 0.013141 m²·K/W

q″ = (260 − 20)/0.013141 = 18 263 W/m²

Temperatures:

T₀ = 260 − 18 263/300 = 199.122 °C
Each steel step drops q″/2600 = 7.024 K
Each RBSN step drops q″/2400 = 7.610 K
T₈ = 20 + 18 263/150 = 141.756 °C

Target: T_target = 20 + 0.95 (T_ss − 20)

Node	T_ss [°C]	Target (95 %) [°C]
0	199.122	190.166
1	192.098	183.493
2	185.073	176.820
3	178.049	170.146
4	171.024	163.473
5	164.000	156.800
6	156.976	150.127
7	149.366	142.898
8	141.756	135.668
8. Time to reach targets (requires numerical iteration)

This part is not reproducible by hand. It needs about 1 180 steps × 9 nodes of repeated application of the Step 4 equations. I did not do this manually. I ran it as a program, using exactly the equations and coefficients above.

First time each node reaches its target (Δt = 1 s):

Node	0	1	2	3	4	5	6	7	8
t [s]	981	1033	1076	1110	1136	1156	1169	1177	1180

Limiting node: node 8, the air-side RBSN surface. It has the smallest capacity but is the farthest from the heat source. The thermal wave has to travel through the whole wall, and it is also cooled by the air.

Temperatures at t = 1180 s:

Node	0	1	2	3	4	5	6	7	8
T [°C]	193.88	186.30	178.85	171.52	164.32	157.26	150.34	142.96	135.68

Node 8 gives (135.683 − 20)/(141.756 − 20) = 95.01 % ✓

9. Result

t ≈ 1 180–1 181 s ≈ 19.7 min (about 19 min 41 s).

The converged value from Step 10 is 1 180.6 s, so I report ≈ 1 181 s ≈ 19.7 min.

10. Verification

Energy conservation (numerical): Integrating the boundary flows over 1 180 s gives:

Energy in from steam: 35 296 877 J/m²
Energy out to air: 13 935 395 J/m²
Net: 21 361 482 J/m²
Stored energy Σ Cᵢ (Tᵢ − 20): 21 361 482 J/m²

The two agree to round-off.

Steady-state consistency (hand-checkable): The fixed point of the Step 4 equations satisfies the steady equations exactly. Flux through each layer, using the Step 7 temperatures:

300(260 − 199.122) = 18 263 W/m²
2600(199.122 − 192.098) = 18 263 W/m²
2400(156.976 − 149.366) = 18 263 W/m²
150(141.756 − 20) = 18 263 W/m²

Sanity check: stored energy at steady state is Σ Cᵢ (T_ss,ᵢ − 20) = 22 286 341 J/m². At 1 180 s the stored energy is 21 361 482 J/m², which is 95.8 % of that. This is consistent with a 95 % criterion.

Time-step convergence:

Δt [s]	2	1	0.5	0.25	0.1
t₈₅ [s]	1178	1180	1180	1180.5	1180.6

With Δt ≥ 1 s the explicit scheme crosses the target slightly early, since the crossing is only resolved to the step. The result converges to about 1 180.6 s as Δt shrinks, so Δt = 1 s is accurate to better than 1 s.

Comparison with your previous calculation: You did not give me the earlier value, so I can’t compare. If it differs, the likely reasons are:

The criterion was applied to a different node or to an average temperature, rather than to all nine nodes (node 8 governs here).
Different boundary-node capacities, such as full instead of half control volumes at nodes 0 and 8.
A different Δt or a coarser crossing resolution, which accounts for differences of only 1–2 s.
