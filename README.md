# CBRS Battery Remanufacturing Scheduling - Variable Reference

This repository implements the **Combinatorial Battery Remanufacturing Scheduling (CBRS)** model.  
The table maps **mathematical symbols** to **descriptions** and **Python code variables**.

---

## Symbols, Descriptions, and Code Variables

| Symbol / Variable | Description | Code Variable |
|------------------|-------------|---------------|
| **Sets** | | |
| $J$ | Set of returned batteries (jobs) | `data["J"]` |
| $\mathcal{R}$ | Set of remanufacturing routes | `data["route_ops"].keys()` |
| $\mathcal{O}_r$ | Set of DRR operations for route $r$ | `data["route_ops"][r]` |
| $M$ | Set of workstations | Union of all `M_o.values()` |
| $M_o \subset M$ | Workstations capable of performing operation $o$ | `data["M_o"][o]` |
| $J_A, J_B, J_C$ | Subsets of jobs by battery grade | `J_A, J_B, J_C` |
| **Parameters** | | |
| $N_m$ | Total number of modules in a battery | `N_m` |
| $\widetilde{b}_j$ | Random variable: number of functional modules in job $j$ | `data["GOOD_MODULES"][j]` |
| $a_{j,r}$ | Modules required for job $j$ on route $r$ | `data["a_jrk"][(j,r,k)]` |
| $b_{j,r}$ | Modules produced by job $j$ on route $r$ | `data["b_underline_jrk"][(j,r,k)]` |
| $c_p$ | Unit time to assemble/disassemble one module | `data["p_jorm"]` or `BASE_OP_TIMES[o]` |
| $v_{j,r}$ | Recovered value of battery $j$ under route $r$ | `ROUTE_VALUE_BY_GRADE[grade][r]` |
| $v_0$ | Value of a single recovered module for energy storage | Used via `Z` in heuristic |
| $\delta_{o,r}$ | Indicator if operation $o$ is in route $r$ | `route_ops[r]` implicitly |
| $H$ | Large constant for sequencing constraints (big-M) | `BIG_M = 100_000` |
| $\alpha$ | Confidence level for chance constraints (DSO) | Used in DSO reformulation |
| $Q_j(\alpha)$ | $\alpha$-quantile of $\widetilde{b}_j$ | Used in DSO reformulation |
| **Decision Variables** | | |
| $t_{j,r} \in \{0,1\}$ | 1 if job $j$ follows route $r$ | `route_assignments[j]` |
| $x_{o,j,m} \in \{0,1\}$ | 1 if operation $o$ of job $j$ is assigned to machine $m$ | `x_o_j_m[(o,j,m)]` |
| $s_{o,j}$ | Start time of operation $o$ for job $j$ | `s_o_j[(o,j)]` |
| $C_{o,j}$ | Completion time of operation $o$ for job $j$ | `c_o_j[(o,j)]` |
| $C_{\mathrm{max}}$ | Makespan of the schedule | `C_max` |
| $z_{j,j',k} \ge 0$ | Modules transferred from job $j$ to job $j'$ | `z[(j,jp,k)]` |
| $y_{(o,j),(o',j')} \in \{0,1\}$ | 1 if $(o,j)$ precedes $(o',j')$ on the same machine | `implicitly via no-overlap / end_before_start` |
| $w_{j,j',k} \in \{0,1\}$ | 1 if material dependency exists from $j$ to $j'$ | `w[(j,jp,k)]` |
| $r^*$ | Selected route for job $j$ in upper-level model | `fixed_route_j[j]` |
| $Z$ | Current stock of available modules (SMR heuristic) | `Z` |
| $p_{o,j,m}$ | Processing time of operation $o$ of job $j$ on machine $m$ | `p_jorm[(j,o,r,m)]` |
