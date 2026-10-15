# Problem 5-33: Finite Difference Solution (Steady, 1-D, Per Unit Area)

## 1. Assumptions and governing equation

- Steady state, one-dimensional conduction along x (through the thickness), with no internal heat generation.
- Constant k in each layer: k₁ = 13 W/m·K (steel), k₂ = 3 W/m·K (nonmetal).
- Perfect thermal contact at the interface. No contact resistance is given, so none is used.
- The bolts are ignored in the heat flow model. No bolt data is given, and treating them as fins or extra conduction paths would require data I don't have.
- Convection coefficients and fluid temperatures are uniform and constant.
- Equations are written for 1 m² of area, so the area cancels.

Governing equation in each layer: d²T/dx² = 0. The finite-difference form for interior nodes is Tᵢ₋₁ − 2Tᵢ + Tᵢ₊₁ = 0.

## 2. Nodes (Δx = 5 mm)

| Node | x (mm) | Location |
|---|---|---|
| 0 | 0 | Steel bottom surface (steam side) |
| 1–5 | 5–25 | Steel interior |
| 6 | 30 | Interface, steel/nonmetal |
| 7 | 35 | Nonmetal interior |
| 8 | 40 | Nonmetal top surface (air side) |

This gives 9 nodes. Useful groups: k₁/Δx = 13/0.005 = 2600 W/m²·K, k₂/Δx = 3/0.005 = 600 W/m²·K, h₁ = 300, h₂ = 50.

## 3. Finite difference equations (energy balance on each node's control volume)

**Node 0 (half-volume, convection from steam):**
h₁(T∞₁ − T₀) + (k₁/Δx)(T₁ − T₀) = 0
(300 + 2600)T₀ − 2600T₁ = 300(260)
**2900 T₀ − 2600 T₁ = 78 000**

**Steel interior, nodes 1–5:**
**Tᵢ₋₁ − 2Tᵢ + Tᵢ₊₁ = 0**

**Node 6 (interface, half-volume of each material):**
(k₁/Δx)(T₅ − T₆) + (k₂/Δx)(T₇ − T₆) = 0
**2600 T₅ − 3200 T₆ + 600 T₇ = 0**

**Node 7 (nonmetal interior):**
**T₆ − 2T₇ + T₈ = 0**

**Node 8 (half-volume, convection to air):**
(k₂/Δx)(T₇ − T₈) + h₂(T∞₂ − T₈) = 0
(600 + 50)T₈ − 600T₇ = 50(20)
**−600 T₇ + 650 T₈ = 1000**

## 4. Matrix system A·T = b

| Row | T₀ | T₁ | T₂ | T₃ | T₄ | T₅ | T₆ | T₇ | T₈ | b |
|---|---|---|---|---|---|---|---|---|---|---|
| 0 | 2900 | −2600 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 78 000 |
| 1 | 1 | −2 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| 2 | 0 | 1 | −2 | 1 | 0 | 0 | 0 | 0 | 0 | 0 |
| 3 | 0 | 0 | 1 | −2 | 1 | 0 | 0 | 0 | 0 | 0 |
| 4 | 0 | 0 | 0 | 1 | −2 | 1 | 0 | 0 | 0 | 0 |
| 5 | 0 | 0 | 0 | 0 | 1 | −2 | 1 | 0 | 0 | 0 |
| 6 | 0 | 0 | 0 | 0 | 0 | 2600 | −3200 | 600 | 0 | 0 |
| 7 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | −2 | 1 | 0 |
| 8 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | −600 | 650 | 1000 |

## 5. Solution

Because every interior equation forces a linear profile within each layer, the system reduces to two unknown gradients. Solving the system (equivalently, by back-substitution) gives:

| Node | x (mm) | T (°C) |
|---|---|---|
| 0 | 0 | **232.4** |
| 1 | 5 | 229.2 |
| 2 | 10 | 226.0 |
| 3 | 15 | 222.8 |
| 4 | 20 | 219.6 |
| 5 | 25 | 216.5 |
| 6 | 30 | **213.3** |
| 7 | 35 | 199.5 |
| 8 | 40 | **185.7** |

The gradients are 3.19 °C per node in the steel and 13.80 °C per node in the nonmetal. The ratio is k₂/k₁ inverted (13/3 = 4.33), as required for continuous heat flux at the interface.

## 6. Temperature vs. position

## 7. Steady-state heat flux

From the node 8 convection boundary:
q″ = h₂(T₈ − T∞₂) = 50(185.7 − 20) = **≈ 8.28 kW/m²**

Cross-checks from other nodes:
- Steam side: h₁(260 − 232.4) = 300(27.6) ≈ 8.28 kW/m² ✓
- Steel conduction: k₁(T₀ − T₁)/Δx = 2600(3.19) ≈ 8.28 kW/m² ✓
- Nonmetal conduction: k₂(T₆ − T₇)/Δx = 600(13.8) ≈ 8.28 kW/m² ✓

Flux is constant through every layer, as required for steady 1-D conduction.

## 8. Independent verification with thermal resistances

| Resistance | Expression | R″ (m²·K/W) |
|---|---|---|
| Steam convection | 1/h₁ = 1/300 | 0.003333 |
| Steel conduction | L₁/k₁ = 0.030/13 | 0.002308 |
| Nonmetal conduction | L₂/k₂ = 0.010/3 | 0.003333 |
| Air convection | 1/h₂ = 1/50 | 0.020000 |
| **Total** | | **0.028974** |

q″ = (260 − 20)/0.028974 = **8283 W/m²** ✓

Back-calculated temperatures:
- T₀ = 260 − 8283(0.003333) = 232.4 °C ✓
- T₆ = 232.4 − 8283(0.002308) = 213.3 °C ✓
- T₈ = 20 + 8283(0.02) = 185.7 °C ✓

The FD and resistance results agree. The match is exact because the true solution is piecewise linear, which the FD scheme reproduces with no truncation error.

## 9. Compliance evaluation

**Stainless steel (limit 260 °C):** The maximum steel temperature is T₀ = 232.4 °C, which is 27.6 °C below the limit. The plate complies. It cannot reach the steam temperature because of the 1/h₁ convection resistance.

**Bolts (limit 204 °C):** The figure shows the bolts passing through both plates. Using the plate temperatures as the local bolt temperature:
- The bolt shank in the steel sees 213.3 to 232.4 °C, which exceeds 204 °C by 9 to 28 °C.
- The 204 °C isotherm lies in the nonmetal, about 3.4 mm above the interface (x ≈ 33.4 mm), from linear interpolation between 213.3 °C and 199.5 °C.
- Only the upper part of the bolt (x > 33.4 mm, head at about 186 °C) is below the limit.

**Conclusion:** The use of ASTM B211 6061 bolts is **not in compliance** with the 204 °C limit under this model. The steel plate itself is compliant.

## 10. Limitations and inconsistencies to flag

1. **Bolt temperature is assumed, not computed.** The 1-D model ignores the bolts. Real bolts have a different k (6061 aluminum is roughly 160 W/m·K, but that value is not given in the problem, so I did not use it). They would act as a high-conductivity path and pull local temperatures toward each other, so the true bolt temperatures would differ somewhat. The conclusion is conservative but needs a 2-D/3-D check for a firm verdict.
2. **No contact resistance** at the interface was given, so it is zero. A real bolted joint has contact resistance, which would lower T₆⁺ on the nonmetal side and raise the steel side.
3. **Interface node:** I used the standard half-volume energy balance. No heat is generated there.
4. **Where the code limit applies:** Whether ASME requires the metal temperature or the fluid temperature for the check is not stated. I used local metal temperature for the bolts and the steel plate. By the fluid-temperature interpretation, the 260 °C steam would equal the steel limit exactly, which would be marginal.
5. **Thickness mismatch with the figure** is not an issue here. The figure's nodes 0 to 8 match the given 30 mm + 10 mm at Δx = 5 mm.
