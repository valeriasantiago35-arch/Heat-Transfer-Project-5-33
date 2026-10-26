Act as an expert professor of Heat Transfer and Numerical Methods.

I need you to calculate ONLY the numerical transient heating time for the ADAPTED configuration of Problem 5-33 from my Heat Transfer project.

Do not rewrite my report. Do not generate GitHub documentation, report sections, or additional documents. I only need the complete calculation, clearly explained step by step, so I can understand it and reproduce the essential verification calculations by hand.

*Existing parameters — DO NOT MODIFY:*

Stainless Steel ASTM A240 904L:
- Thickness = 0.030 m
- Thermal conductivity = 13 W/(m·K)
- Density = 7,900 kg/m³
- Specific heat = 500 J/(kg·K)

Reaction-Bonded Silicon Nitride (RBSN):
- Thickness = 0.010 m
- Thermal conductivity = 12 W/(m·K)
- Density = 2,500 kg/m³
- Specific heat = 1,100 J/(kg·K)

Boundary conditions:
- Lower steam temperature = 260°C
- Lower convection coefficient = 300 W/(m²·K)
- Upper air temperature = 20°C
- Upper convection coefficient = 150 W/(m²·K)

Numerical model:
- Nine nodes, numbered 0–8
- Node spacing = 0.005 m
- Material interface = node 6
- Initial temperature of every node = 20°C
- Heating begins at t = 0
- Time criterion = all nine nodes have achieved at least 95% of their temperature change toward steady state

*Required calculations:*

1. Calculate thermal diffusivity for both materials, showing the formula, numerical substitution, units, and result.

2. Calculate the thermal capacity of each node, including the half-control-volumes at the boundaries and the combined capacity at the material interface.

3. Calculate the conductance between adjacent nodes.

4. Develop the transient finite difference equations for nodes 0, 1–5, 6, 7, and 8. Show numerical coefficients.

5. Determine the maximum stable time step and verify that Δt = 1 second is acceptable.

6. Demonstrate the complete FIRST time-step calculation for the relevant nodes, including numerical substitutions.

7. Calculate the 95% target temperature for each node using the existing adapted steady-state temperatures.

8. Numerically determine the time required for every node to reach its target. Identify the limiting node.

9. Present the final time in seconds and minutes.

10. Verify the numerical result using energy conservation, steady-state consistency, and time-step convergence.

*Important instructions:*

- Show all equations in readable mathematical notation, not programming code.
- Do not skip intermediate calculations.
- Distinguish calculations reproducible by hand from results requiring numerical iteration.
- Do not claim that hundreds of numerical iterations were calculated manually.
- Do not calculate the original Problem 5-33 or Problem 5-34.
- Do not change any existing steady-state temperature, thermal resistance, or heat flux.
- Independently calculate the time instead of assuming a predetermined answer.
- If a result differs from the previous calculation, explain the numerical reason.
- Do not create files, report text, GitHub documentation, or unrelated explanations.

*Your only deliverable is a clear, complete, step-by-step numerical calculation of the transient heating time for the adapted Problem 5-33, suitable for engineering manual verification.*
