# cellular-automata
Cellular automata: Game of Life and SIRS

# SIRS Model Simulation

This project simulates the **SIRS epidemic model** on a 2D square lattice with periodic boundary conditions using a **random sequential updating scheme**.

The model was implemented for coursework tasks involving simulation, phase diagrams, variance analysis, and immunity effects.

---

# Model

Each site on the lattice represents an agent in one of three states:

- **S** – susceptible  
- **I** – infected  
- **R** – recovered  

The transition rules are:

- **S → I** with probability `pSI` if at least one neighbour is infected
- **I → R** with probability `pIR`
- **R → S** with probability `pRS`

The lattice uses:

- **4-neighbour interactions**
- **periodic boundary conditions**
- **random sequential updating**

One **sweep** corresponds to `L × L` random updates.

---

# Program Modes

The program has four main modes:

| Mode | Description |
|-----|-------------|
| `animate` | Visualise the SIRS dynamics |
| `phase` | Generate phase diagram of average infected fraction |
| `varcut` | Variance of infected sites along a parameter cut |
| `immunity` | Study effect of permanent immunity |

---

# Arguments

The program uses command line arguments (via `argparse`).  
When running in **Spyder**, these are defined inside `main()` using:

```python
args = ap.parse_args([...])
```

## General Arguments

| Argument | Description | Default |
|--------|-------------|--------|
| `--mode` | Simulation mode (`animate`, `phase`, `varcut`, `immunity`) | `animate` |
| `--L` | Lattice size | `50` |
| `--seed` | Random seed | `None` |

---

## Initial Condition Arguments

These define the initial fractions of sites in each state.

| Argument | Description | Default |
|--------|-------------|--------|
| `--initS` | Initial fraction susceptible | `0.90` |
| `--initI` | Initial fraction infected | `0.05` |
| `--initR` | Initial fraction recovered | `0.05` |

---

## Animation Parameters

Used when `--mode animate`.

| Argument | Description | Default |
|--------|-------------|--------|
| `--pSI` | Probability of transition S → I | `0.4` |
| `--pIR` | Probability of transition I → R | `0.5` |
| `--pRS` | Probability of transition R → S | `0.5` |
| `--sweeps` | Number of simulation sweeps | `1000` |
| `--every` | Plot update frequency | `1` |
| `--fIm` | Fraction of permanently immune sites | `0.0` |

---

## Phase Diagram Parameters

Used when `--mode phase`.

| Argument | Description | Default |
|--------|-------------|--------|
| `--pIR_fixed` | Fixed value of pIR | `0.5` |
| `--pSImin` | Minimum value of pSI | `0.0` |
| `--pSImax` | Maximum value of pSI | `1.0` |
| `--pRSmin` | Minimum value of pRS | `0.0` |
| `--pRSmax` | Maximum value of pRS | `1.0` |
| `--step` | Parameter step size | `0.01` |
| `--equil` | Number of equilibration sweeps | `1000` |
| `--meas` | Number of measurement sweeps | `1000` |
| `--out` | Output plot filename | `out.png` |

---

## Variance Cut Parameters

Used when `--mode varcut`.

| Argument | Description | Default |
|--------|-------------|--------|
| `--pRS_fixed` | Fixed value of pRS | `0.5` |
| `--pSIa` | Minimum pSI value | `0.2` |
| `--pSIb` | Maximum pSI value | `0.5` |
| `--total` | Total measurement sweeps | `20000` |
| `--block` | Block size used for error bar calculation | `400` |

Error bars are estimated using block averaging.

---

## Immunity Scan Parameters

Used when `--mode immunity`.

| Argument | Description | Default |
|--------|-------------|--------|
| `--p` | Transition probability (pSI = pIR = pRS = p) | `0.5` |
| `--fmin` | Minimum immune fraction | `0.0` |
| `--fmax` | Maximum immune fraction | `0.8` |
| `--step` | Step size for immunity fraction | `0.01` |

Each plot automatically saves:

PNG plot

CSV data file

Example:

task3_phase.png
task3_phase.csv

The CSV file contains the data used to generate the figure.

Requirements

Python packages required:

numpy
matplotlib
numba

Install with:

pip install numpy matplotlib numba
Example Runs

Example arguments inside main().

Phase diagram
args = ap.parse_args([
    "--mode", "phase",
    "--step", "0.05",
    "--equil", "100",
    "--meas", "1000",
    "--out", "task3_phase.png",
])
Variance cut
args = ap.parse_args([
    "--mode", "varcut",
    "--step", "0.01",
    "--equil", "100",
    "--total", "10000",
    "--block", "200",
    "--out", "task4_varcut.png",
])
Immunity scan
args = ap.parse_args([
    "--mode", "immunity",
    "--out", "task5_immunity.png",
])
