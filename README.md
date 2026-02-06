# Best Backpacker: Mathematical Modeling and Shapley Value Analysis with Python

## Overview
This project studies the **Best Backpacker problem**, a constrained resource allocation task.
The goal is to select and evaluate items under weight, volume, and budget constraints,
and to fairly assess the contribution of each item using the **Shapley Value**.

## Method Summary
- The backpacking problem is formulated as an optimization problem with multiple constraints
- Item utility depends on scenario variables such as duration, altitude, and temperature
- Cooperative game theory is applied, using **Shapley Value** to measure marginal contributions
- Numerical evaluation is performed using **Python**

## My Role
This project was completed as a team project.  
I primarily focused on constructing the **mathematical modeling logic**,
including defining assumptions, constraints, and objective formulations.
I also worked on understanding and following Python code for utility
and Shapley Value computation, and assisted in data analysis and result interpretation.

---

## Files
- **items.csv**  
  Master list of items with weights, volumes, prices, and utility parameters

- **synergies.csv**  
  Pairwise synergy or substitution interactions and their conditions

- **scenarios.csv**  
  Example scenarios including duration, capacity constraints, budget, and context profiles

- **divisible_bounds.csv**  
  Minimum, maximum, and step sizes for divisible items (e.g., water, food, sunscreen, electrolytes)

---

## Utility Model
For an item *i*, baseline stand-alone utility under trip duration **T** (hours),
altitude **H** (meters), and average temperature **tempC**:

```text
u_i(T, H, tempC) = alpha
                   + beta_T_per_hour * T
                   + beta_altitude_per_1000m * (H / 1000)
                   + beta_temp_cold_per_c * max(0, 12 - tempC)
                   + beta_temp_hot_per_c  * max(0, tempC - 20)
                   + 1(T >= t_threshold_hours) * gamma_after_threshold
                   + 1(nights >= nights_req) * 0.5
```

- **Divisible items** (`divisible == 1`) can be split into fractional units  
  - Utility is multiplied by the chosen number of units  
  - Weight, volume, and price scale linearly

- **Diminishing returns**  
  - If quantity exceeds `diminish_after_units`, marginal utility may be scaled down  
  - Example: marginal utility × 0.5 beyond the threshold

---

## Coalition Adjustments
Synergies and substitutions are applied **after** summing stand-alone utilities:

- `mode == "additive"`  
  - Add `delta_value` if the condition holds

- `mode == "multiplier"`  
  - Multiply the coalition value by `delta_value` if the condition holds

---

## Constraints
- Scenario files provide typical limits:
  - Maximum weight (`C_weight_kg`)
  - Maximum volume (`C_volume_l`)
  - Budget constraint (`B_budget_usd`)
- These constraints can be directly used as **knapsack-style constraints**

---

## Notes
- Prices are rough placeholders in USD and can be replaced with local currency
- Conditions in `synergies.csv` are stored as strings and can be parsed or evaluated in code
- These CSV files are designed to integrate with:
  - Shapley Value analysis
  - Knapsack optimization
  - Monte Carlo simulation pipelines
