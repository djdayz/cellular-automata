# cellular-automata
Cellular automata: Game of Life and SIRS

# Game of Life (50×50, periodic boundary conditions), gameoflife.py

This project simulates **Game of Life** on a 2D square lattice with **periodic boundary conditions**. It includes:

- animation of selected initial conditions
- equilibration-time analysis from random initial conditions
- glider centre-of-mass (COM) tracking and speed estimation
- saving plots and corresponding data files

The code is written in Python and uses **NumPy**, **Matplotlib**, and **Numba**.

---

## Features

### 1. Animation
The program can animate the Game of Life starting from:

- a random initial condition
- a blinker (oscillator)
- a glider (moving pattern)
- a beehive (still life / absorbing state)

### 2. Equilibration study
For random initial conditions, the code measures the number of active (alive) sites \(A(t)\) over time and defines an equilibration time \(t_{eq}\) as the first time at which \(A(t)\) remains constant for a chosen number of steps.

It can then:

- run many simulations
- build a histogram of equilibration times
- optionally show an example trace \(A(t)\)

### 3. Glider centre of mass and speed
Two glider analysis modes are included:

- **`glider_com_stop`**  
  Tracks the glider COM until it approaches a boundary, then fits straight lines to \(x_{COM}(t)\) and \(y_{COM}(t)\) to estimate velocity and speed.

- **`glider_com_mod`**  
  Tracks the glider COM continuously on the periodic lattice, producing sawtooth behaviour in time and a segmented wrapped trajectory.

### 4. Output files
For each plot, the program saves a corresponding **data file** and **image file** using filenames beginning with `gol_`.

---

## Command-line arguments

### General arguments

| Argument | Type | Default | Description |
|---|---:|---:|---|
| `--L` | int | `50` | Lattice size \(L \times L\) |
| `--mode` | str | `equilibrate` | Run mode: `animate`, `equilibrate`, `glider_com_stop`, `glider_com_mod` |
| `--seed` | int | `0` | Random seed |
| `--outdir` | str | `gol_outputs` | Output directory for `gol_*` files |

### Animation arguments

| Argument | Type | Default | Description |
|---|---:|---:|---|
| `--init` | str | `glider` | Initial condition: `random`, `blinker`, `glider`, `beehive` |
| `--steps` | int | `500` | Number of animation frames / steps |
| `--interval` | int | `40` | Animation frame interval in ms |
| `--density` | float | `0.20` | Random initial density (used when `--init random`) |

### Equilibration-study arguments

| Argument | Type | Default | Description |
|---|---:|---:|---|
| `--runs` | int | `500` | Number of random simulations |
| `--max_steps` | int | `5000` | Maximum number of steps per run |
| `--plateau_len` | int | `50` | Number of consecutive constant steps required for equilibrium |
| `--bins` | int | `30` | Number of histogram bins |
| `--example_trace` | flag | off | Also plot and save one example \(A(t)\) trace |

### Glider COM arguments

| Argument | Type | Default | Description |
|---|---:|---:|---|
| `--com_steps` | int | `500` | Number of steps used for glider COM tracking |
| `--margin` | int | `3` | Boundary margin used in `glider_com_stop` mode |
| `--glider_x` | int | `10` | Top-left x-position of the glider |
| `--glider_y` | int | `10` | Top-left y-position of the glider |
| `--no_com_plots` | flag | off | Disable COM plots |

---

# SIRS Model Simulation, SIRS.py

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

- PNG plot
- CSV data file

Example:

task3_phase.png
task3_phase.csv

The CSV file contains the data used to generate the figure.


