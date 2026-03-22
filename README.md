# CBRS Battery Remanufacturing Scheduling - Variable Reference

This repository implements the **Combinatorial Battery Remanufacturing Scheduling (CBRS)** model.  
The table maps **mathematical symbols** to **descriptions** and **Python code variables**.

---

## Symbols, Descriptions, and Code Variables

| Symbol / Variable | Description | Code Variable |
|------------------|-------------|---------------|
| **Sets** | | |
| J | Set of returned batteries (jobs) | `data["J"]` |
| R | Set of remanufacturing routes | `data["route_ops"].keys()` |
| O_r | Set of DRR operations for route r | `data["route_ops"][r]` |
| M | Set of workstations | Union of all `M_o.values()` |
| M_o ⊆ M | Workstations capable of performing operation o | `data["M_o"][o]` |
| J_A, J_B, J_C | Subsets of jobs by battery grade | `J_A, J_B, J_C` |
| **Parameters** | | |
| N_m | Total number of modules in a battery | `N_m` |
| b~_j | Random variable: number of functional modules in job j | `data["GOOD_MODULES"][j]` |
| a_jr | Modules required for job j on route r | `data["a_jrk"][(j,r,k)]` |
| b_jr | Modules produced by job j on route r | `data["b_underline_jrk"][(j,r,k)]` |
| c_p | Unit time to assemble/disassemble one module | `data["p_jorm"]` or `BASE_OP_TIMES[o]` |
| v_jr | Recovered value of battery j under route r | `ROUTE_VALUE_BY_GRADE[grade][r]` |
| v_0 | Value of a single recovered module for energy storage | Used via `Z` in heuristic |
| δ_or | Indicator if operation o is in route r | `route_ops[r]` implicitly |
| H | Large constant for sequencing constraints (big-M) | `BIG_M = 100_000` |
| α | Confidence level for chance constraints (DSO) | Used in DSO reformulation |
| Q_j(α) | α-quantile of b~_j | Used in DSO reformulation |
| **Decision Variables** | | |
| t_jr ∈ {0,1} | 1 if job j follows route r | `route_assignments[j]` |
| x_ojm ∈ {0,1} | 1 if operation o of job j is assigned to machine m | `x_o_j_m[(o,j,m)]` |
| s_oj | Start time of operation o for job j | `s_o_j[(o,j)]` |
| C_oj | Completion time of operation o for job j | `c_o_j[(o,j)]` |
| C_max | Makespan of the schedule | `C_max` |
| z_jj'k ≥ 0 | Modules transferred from job j to job j' | `z[(j,jp,k)]` |
| y_(oj,o'j') ∈ {0,1} | 1 if (o,j) precedes (o',j') on the same machine | `implicitly via no-overlap / end_before_start` |
| w_jj'k ∈ {0,1} | 1 if material dependency exists from j to j' | `w[(j,jp,k)]` |
| r* | Selected route for job j in upper-level model | `fixed_route_j[j]` |
| Z | Current stock of available modules (SMR heuristic) | `Z` |
| op_enqueue_order | Operation scheduling order | `op_enqueue_order` |
| machine_available[m] | Next available time of machine m | `machine_available[m]` |
| job_last_completion[j] | Last completion time of job j | `job_last_completion[j]` |
| Operations | List of (operation, job) tuples | `Operations` |
| op_int[(o,j)] | CPLEX interval variable for operation | `op_int[(o,j)]` |
| machine_activities[m] | Interval variables assigned to machine m | `machine_activities[m]` |
| p_ojm | Processing time of operation o of job j on machine m | `p_jorm[(j,o,r,m)]` |
