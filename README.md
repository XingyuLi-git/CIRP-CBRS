\# CBRS Battery Remanufacturing Scheduling - Variable Reference



This repository contains the implementation of a \*\*Combinatorial Battery Remanufacturing Scheduling (CBRS)\*\* model, including heuristic and CPLEX-based approaches.  

The table below maps the \*\*mathematical symbols\*\* used in the model to their \*\*descriptions\*\* and the \*\*Python code variables\*\*.



\---



\## Table of Symbols, Descriptions, and Code Variables



| Symbol / Variable | Description | Code Variable / Implementation |

|------------------|-------------|--------------------------------|

| \*\*Sets\*\* | | |

| `J` | Set of returned batteries (jobs) | `data\["J"]` |

| `\\mathcal{R}` | Set of remanufacturing routes | `data\["route\_ops"].keys()` |

| `\\mathcal{O}\_r` | Set of DRR operations for route `r` | `data\["route\_ops"]\[r]` |

| `M` | Set of workstations | Union of all `M\_o.values()` |

| `M\_o \\subset M` | Set of workstations capable of performing operation `o` | `data\["M\_o"]\[o]` |

| `J\_A, J\_B, J\_C` | Subsets of jobs by battery grade A, B, and C | `J\_A, J\_B, J\_C` in heuristic |

| \*\*Parameters\*\* | | |

| `N\_m` | Total number of modules in a battery | `N\_m` |

| `\\tilde{b}\_j` | Random variable representing number of functional modules in job `j` | `data\["GOOD\_MODULES"]\[j]` (heuristic) |

| `b\_j, a\_j` | Lower and upper bounds on functional modules for job `j` | `data\["b\_underline\_jrk"]`, `data\["a\_jrk"]` |

| `c\_p` | Unit time to assemble/disassemble one module | `data\["p\_jorm"]` or `BASE\_OP\_TIMES\[o]` |

| `v\_{j,r}` | Recovered value of battery `j` under route `r` | `ROUTE\_VALUE\_BY\_GRADE\[grade]\[r]` |

| `v\_0` | Value of a single recovered module used for energy storage | Used via `Z` in heuristic |

| `a\_{j,r}` | Number of modules required for job `j` on route `r` | `data\["a\_jrk"]\[(j,r,k)]` |

| `b\_{j,r}` | Number of modules produced by job `j` on route `r` | `data\["b\_underline\_jrk"]\[(j,r,k)]` |

| `\\delta\_{o,r}` | Binary indicator if operation `o` is included in route `r` | `route\_ops\[r]` implicitly |

| `H` | Large constant for sequencing constraints (big-M) | `BIG\_M = 100\_000` |

| `\\alpha` | Confidence level for chance constraints in DSO | Used in DSO reformulation |

| `Q\_j(\\alpha)` | \\alpha-quantile of \\tilde{b}\_j | Used in DSO reformulation |

| \*\*Decision Variables\*\* | | |

| `t\_{j,r} \\in {0,1}` | 1 if job `j` follows route `r`, 0 otherwise | `route\_assignments\[j]` |

| `x\_{o,j,m} \\in {0,1}` | 1 if operation `o` of job `j` is assigned to machine `m`, 0 otherwise | `x\_o\_j\_m\[(o,j,m)]` |

| `s\_{o,j}` | Start time of operation `o` for job `j` | `s\_o\_j\[(o,j)]` |

| `C\_{o,j}` | Completion time of operation `o` for job `j` | `c\_o\_j\[(o,j)]` |

| `C\_{\\max}` | Makespan of the schedule | `C\_max` |

| `z\_{j,j',k} \\ge 0` | Number of modules transferred from job `j` to job `j'` | `z\[(j,jp,k)]` |

| `y\_{(o,j),(o',j')} \\in {0,1}` | 1 if operation `(o,j)` precedes `(o',j')` on the same machine | `implicitly via no-overlap / end\_before\_start constraints` |

| `w\_{j,j',k} \\in {0,1}` | 1 if material dependency exists from job `j` to job `j'` | `w\[(j,jp,k)]` |

| `r^\\ast` | Selected route for job `j` in upper-level model | `fixed\_route\_j\[j]` |

| `Z` | Current stock of available functional modules (heuristic SMR) | `Z` |

| `op\_enqueue\_order` | Track order of operation scheduling | `op\_enqueue\_order` |

| `machine\_available\[m]` | Next available time of machine `m` | `machine\_available\[m]` |

| `job\_last\_completion\[j]` | Last completion time of job `j` | `job\_last\_completion\[j]` |

| `Operations` | List of (operation, job) tuples based on selected routes | `Operations` |

| `op\_int\[(o,j)]` | CPLEX interval variable for operation | `op\_int\[(o,j)]` |

| `machine\_activities\[m]` | List of interval variables assigned to machine `m` | `machine\_activities\[m]` |

| `p\_{o,j,m}` | Processing time of operation `o` of job `j` on machine `m` | `p\_jorm\[(j,o,r,m)]` |



\---



This table allows you to \*\*quickly trace any mathematical variable or parameter in the CBRS model to its Python implementation\*\*.  



