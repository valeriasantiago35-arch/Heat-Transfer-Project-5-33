# Adapted Problem 5-33: Composite Wall FD Analysis

Everything is per unit area (1 m²), in SI units, with temperatures in °C. Node 0 is the steam-side surface, node 6 is the interface, and node 8 is the air-side surface, as in Fig. P5-33.

## 1. Physical reasoning for the modifications

Heat flows in series from the steam to the air. Raising k₂ from 3 to 12 W/m·K cuts the nonmetal's conduction resistance by 4×. Raising h₂ from 50 to 150 W/m²·K cuts the air-side convection resistance by 3×. Lowering a series resistance with the same ΔT_total = 240 °C raises q″. A larger q″ means a larger drop across the steam-side film and a smaller drop to the air, so every internal temperature moves toward the air temperature and drops.

## 2. Thermal resistances affected

| Resistance | Expression | Original (m²·K/W) | Adapted (m²·K/W) | Changed? |
|---|---|---|---|---|
| R_conv,1 (steam) | 1/h₁ = 1/300 | 0.003333 | 0.003333 | No |
| R_SS | L₁/k₁ = 0.030/13 | 0.002308 | 0.002308 | No |
| R_NM | L₂/k₂ = 0.010/k₂ | 0.003333 (k₂=3) | 0.000833 (k₂=12) | Yes |
| R_conv,2 (air) | 1/h₂ | 0.020000 (h₂=50) | 0.006667 (h₂=150) | Yes |
| **R_total** | sum | **0.028974** | **0.013141** | |

In the original wall, the air film accounted for 69% of the total resistance. In the adapted wall it accounts for 51%, and it is still the largest.

## 3. Governing equation and assumptions

Steady 1D conduction without generation is d/dx(k dT/dx) = 0, with k constant within each layer.

Assumptions:
- Steady state.
- 1D heat flow, with no lateral effects.
- Constant properties.
- Perfect thermal contact at the interface, with no contact resistance.
- Bolts are ignored, so this is a plain two-layer wall.
- No radiation.
- Uniform T∞ and h on each side.

## 4–5. Finite difference equations (Δx = 0.005 m)

Nodes 0 and 8 are half-cells at the boundaries, and node 6 is a half-cell on each side of the interface.

Define Bi₁ = h₁Δx/k₁ = 300(0.005)/13 = 0.115385 and Bi₂ = h₂Δx/k₂ = 150(0.005)/12 = 0.0625.

- **Node 0 (steam convection):** h₁(T∞₁ − T₀) + k₁(T₁ − T₀)/Δx = 0, so (1 + Bi₁)T₀ − T₁ = Bi₁T∞₁, giving 1.115385 T₀ − T₁ = 30.000.
- **Nodes 1–5 (interior SS):** T_{m−1} − 2T_m + T_{m+1} = 0.
- **Node 6 (interface):** k₁(T₅ − T₆)/Δx + k₂(T₇ − T₆)/Δx = 0, so 13T₅ − 25T₆ + 12T₇ = 0.
- **Node 7 (interior NM):** T₆ − 2T₇ + T₈ = 0.
- **Node 8 (air convection):** k₂(T₇ − T₈)/Δx + h₂(T∞₂ − T₈) = 0, so −T₇ + 1.0625 T₈ = 1.25.

For the original case, Bi₂ = 50(0.005)/3 = 0.08333 and the interface row is 13T₅ − 16T₆ + 3T₇ = 0.

## 6. Matrix system A·T = b

```
Row\Col    T0        T1   T2   T3   T4   T5   T6   T7   T8  |   b
  0      1.115385   -1                                       |  30.00
  1        -1        2   -1   ... (tridiagonal, 1 -2 1 form) |   0
  2             -1    2   -1                                 |   0
  3                  -1    2   -1                            |   0
  4                       -1    2   -1                       |   0
  5                            -1    2   -1                  |   0
  6                                 13  -25   12             |   0
  7                                       1   -2    1        |   0
  8                                           -1   1.0625    |   1.25
```

Rows 1–5 and 7 are written as −T_{m−1} + 2T_m − T_{m+1} = 0 if you want A symmetric in the interior (multiply by −1).

## 7. Solution (nodes 0–8)

Solving the system, for example by Thomas elimination, gives the temperatures below. The values are also confirmed by the resistance method in item 9.

| Node | x (mm) | T (°C) |
|---|---|---|
| 0 | 0 | 199.12 |
| 1 | 5 | 192.10 |
| 2 | 10 | 185.07 |
| 3 | 15 | 178.05 |
| 4 | 20 | 171.02 |
| 5 | 25 | 164.00 |
| 6 (interface) | 30 | 156.98 |
| 7 | 35 | 149.37 |
| 8 | 40 | 141.76 |

The profile is piecewise linear, as expected for 1D steady conduction without generation. The slope is 7.024 °C per node in the stainless steel and 7.610 °C per node in the nonmetal.

## 8. Adapted heat flux

q″ = k₁(T₀ − T₁)/Δx = 13(199.12 − 192.10)/0.005 = **≈18,263 W/m²**

## 9. Independent verification by thermal resistances

R_total = 1/300 + 0.030/13 + 0.010/12 + 1/150 = 0.003333 + 0.002308 + 0.000833 + 0.006667 = 0.013141 m²·K/W

q″ = (260 − 20)/0.013141 = **18,263 W/m²** ✔

- T₀ = 260 − 18,263(0.003333) = 260 − 60.88 = 199.12 °C ✔
- T₆ = 199.12 − 18,263(0.002308) = 199.12 − 42.15 = 156.98 °C ✔
- T₈ = 156.98 − 18,263(0.000833) = 156.98 − 15.22 = 141.76 °C ✔
- Cross-check from the air side: T₈ = 20 + 18,263/150 = 141.76 °C ✔

**Energy balances (W/m²):**

| Location | Heat in | Heat out |
|---|---|---|
| Steam surface | 300(260 − 199.12) = 18,263 | 13(7.024)/0.005 = 18,263 |
| Interface | 13(7.024)/0.005 = 18,263 | 12(7.610)/0.005 = 18,263 |
| Air surface | 12(7.610)/0.005 = 18,263 | 150(141.76 − 20) = 18,263 |

Matrix row check: row 0 gives 1.115385(199.122) − 192.098 = 30.000, and row 6 gives 13(164.000) − 25(156.976) + 12(149.366) ≈ 0.

**Rounding:** The FD solution is exact for this problem, because the temperature profile is linear in each layer. Any differences of about 0.001 °C or 1 W/m² between the two methods come only from carrying 4–6 significant figures.

## 10. Original vs. adapted temperatures

Original: R_total = 0.028974, so q″ = 240/0.028974 = 8,283 W/m².

| Node | x (mm) | T original (°C) | T adapted (°C) | ΔT (°C) |
|---|---|---|---|---|
| 0 | 0 | 232.39 | 199.12 | −33.27 |
| 1 | 5 | 229.20 | 192.10 | −37.11 |
| 2 | 10 | 226.02 | 185.07 | −40.95 |
| 3 | 15 | 222.83 | 178.05 | −44.78 |
| 4 | 20 | 219.65 | 171.02 | −48.62 |
| 5 | 25 | 216.46 | 164.00 | −52.46 |
| 6 | 30 | 213.27 | 156.98 | −56.30 |
| 7 | 35 | 199.47 | 149.37 | −50.10 |
| 8 | 40 | 185.66 | 141.76 | −43.91 |

## 11. Temperature distributions

The slope change at x = 30 mm marks the interface. The adapted curve is lower everywhere and has a milder slope in the nonmetal relative to the stainless steel (7.6 vs. 7.0 °C per node, compared with 13.8 vs. 3.2 originally).

## 12. Percentage increase in heat flux

Δq″/q″_orig = (18,263 − 8,283)/8,283 = **+120.5%**. The flux is about 2.2 times the original.

Contribution of each change on its own:

| Case | R_total | q″ (W/m²) | Increase |
|---|---|---|---|
| Only k₂ → 12 | 0.026474 | 9,066 | +9.4% |
| Only h₂ → 150 | 0.015641 | 15,344 | +85.2% |
| Both | 0.013141 | 18,263 | +120.5% |

The h₂ change does most of the work, because the air film was the dominant resistance.

## 13. Evaluation against the stated limits (simplified model only)

| Quantity | Original | Adapted | Limit |
|---|---|---|---|
| Max modeled temperature in the nonmetal plate (interface, node 6) | 213.3 °C | 157.0 °C | 204 °C (bolt) |
| Max modeled temperature in the stainless steel (node 0) | 232.4 °C | 199.1 °C | 260 °C |

- **Bolt limit:** The modeled wall temperature at the interface is 213.3 °C originally, which is above 204 °C. In the adapted model it is 157.0 °C, which is 47 °C below 204 °C.
- **Stainless steel limit:** The modeled maximum is 199.1 °C, which is 60.9 °C below 260 °C. This limit can't be exceeded in this model anyway, because the profile falls monotonically from steam to air and the steam is at 260 °C.
- **What this does not show:** These are modeled wall temperatures, not bolt temperatures. The bolts pass through both plates and conduct heat as fins. They also have heads and nuts exposed to the air, and contact resistances at the interface. None of that is in this 1D model, so the actual bolt temperature may differ from the wall temperature at the same location.
- **Compliance:** ASME compliance also depends on the code provisions, such as allowable stress at temperature, pressure, and design conditions. This analysis supports only a temperature comparison against the limits you supplied, not a compliance claim.

## 14. Feasibility and limitations

**Feasibility:**
- Raising h₂ to 150 W/m²·K generally requires forced airflow (fans or ducted flow), and the achieved value depends on geometry and velocity. It should be confirmed with a correlation or measurement rather than assumed.
- k₂ = 12 W/m·K is four times the original value, so a different nonmetal is needed. A candidate would have to be checked against supplier data for conductivity, temperature rating, and compatibility with the bolts and the steel plate.
- The adapted wall passes 2.2 times more heat. If the design goal includes limiting energy loss from the steam, this is a trade-off, since lowering temperatures here comes from increasing losses.

**Limitations:**
- Contact resistance at the interface is neglected, and real bolted joints have it. It would raise the temperature drop across the interface and the steam-side temperatures.
- Heat conduction through the bolts and any 2D/3D effects are neglected.
- Properties are assumed constant with temperature.
- Radiation is neglected.
- Results are steady state only. No transient cooling time or heat-up time can be inferred from them.
- The Δx = 5 mm grid adds no discretization error here, because the exact solution is linear within each layer.
