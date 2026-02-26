# Fatigue Damage UMAT Subroutine for Abaqus  
## Microstructure-Informed Cyclic Damage Modeling of Short-Fiber Composites

This repository contains a custom **UMAT (User Material) subroutine** developed for Abaqus to simulate **fatigue damage evolution in short-fiber composites** under cyclic loading.

The model incorporates:

- Nonlinear constitutive response
- Matrix damage evolution (quasi-static)
- Fatigue degradation under cyclic loading
- History-dependent internal variables
- Microstructure-informed calibration

---

## Objective

To provide a robust, research-grade UMAT implementation capable of:

- Predicting stiffness degradation under cyclic loading
- Capturing progressive matrix damage
- Modeling fatigue life reduction
- Supporting microstructure-informed RVE simulations
- Enabling integration with Voronoi-based fiber models

---

## Repository Contents

### Core Subroutines

- **`fatigue_damage_umat.f`**  
  Main fatigue damage UMAT:
  - Stress update algorithm
  - Consistent tangent stiffness
  - Damage evolution law
  - Cycle-dependent degradation
  - Internal state variables tracking

- **`matrix_damage_static.f`**  
  Static damage calibration subroutine:
  - Monotonic damage evolution
  - Used for parameter identification
  - Baseline matrix behavior

---

### Supporting Scripts

- **`Discrete_model_abaqus.py`**  
  Abaqus CAE automation script:
  - Creates model
  - Assigns UMAT
  - Defines boundary conditions
  - Submits fatigue simulations

- **`voronoi_fiber_summary_S1_AF.csv`**  
  Example microstructure region data used for RVE simulations.

---

## Theoretical Background

The implemented fatigue model includes:

### 1. Stress–Strain Relation

Effective stress formulation:

σ = (1 − D) C₀ : ε

Where:
- D = scalar damage variable
- C₀ = undamaged stiffness tensor
- ε = strain tensor

---

### 2. Damage Evolution

Damage evolves as a function of:

- Equivalent strain / stress measure
- Accumulated cycle count
- Internal history variable κ

General form:

D = f(κ, N)

Where:
- κ = maximum historical equivalent strain
- N = number of cycles

---

### 3. Fatigue Degradation

The cyclic degradation law accounts for:

- Progressive stiffness loss
- Damage accumulation per cycle
- Possible nonlinear acceleration near failure

---

## Abaqus Implementation

The UMAT provides:

- Stress tensor (STRESS)
- Consistent tangent matrix (DDSDDE)
- Updated state variables (STATEV)
- Energy tracking

Compatible with:
- Abaqus/Standard
- Implicit cyclic simulations
- RVE-based periodic or KUBC boundary conditions

---

## Requirements

- Abaqus/Standard (2020+ recommended)
- Intel Fortran Compiler (ifort)
- Windows or Linux supported

---

## How to Compile UMAT

### Windows Example:

```bash
abaqus make library=fatigue_damage_umat.f
````

### Linux Example:

```bash
abaqus make library=fatigue_damage_umat.f
```

Ensure:

* Fortran compiler is properly linked
* `abaqus verify -user_std` runs successfully

---

## How to Run Simulation

### 1) Prepare Input File

In Abaqus:

* Define material as:

  * Mechanical → User Material
  * Specify number of state variables
* Assign UMAT

Example:

```
*User Material, constants=...
```

---

### 2) Submit Job

```bash
abaqus job=FatigueModel user=fatigue_damage_umat.f
```

---

## State Variables (Example)

Typical STATEV usage:

| Index | Variable                    |
| ----- | --------------------------- |
| 1     | Damage variable D           |
| 2     | Maximum equivalent strain κ |
| 3     | Cycle counter               |
| 4     | Energy release measure      |

(Adjust based on your implementation.)

---

## Calibration Workflow

1. Use `matrix_damage_static.f` to calibrate:
For quasi-static damage simualtion
   * Elastic modulus
   * Damage initiation threshold
   * Softening parameter

2. Calibrate fatigue parameters:

   * Cycle sensitivity exponent
   * Degradation coefficient
   * Accumulation rate

3. Validate against:

   * Experimental cyclic stress–strain
   * Stiffness degradation curves
   * S–N data

---

## Example Applications

* Short-fiber thermoplastic RVEs
* Voronoi-based microstructure simulations
* Region-wise fatigue degradation
* Microstructure-to-fatigue surrogate modeling

## Verification & Validation

Before engineering deployment:

* Verify tangent stiffness consistency
* Check energy balance
* Perform mesh sensitivity study
* Validate against experimental fatigue curves

---
