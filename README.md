# continuationHYPAD
Developed by [David Y. Risk-Mora](https://orcid.org/0000-0002-7004-408X) and collaborators. 

## Markings
### Distribution
Distribution Statement A: Approved for Public Release; Distribution is Unlimited. PA# AFRL-2025-2361

### Acknowledgement 
This material is based on research sponsored by the Air Force Research Laboratory under agreement number FA8650-20-2-5853. The U.S. Government is authorized to reproduce and distribute reprints for Government purposes notwithstanding any copyright notation thereon. The views and conclusions contained herein are those of the authors and should not be interpreted as necessarily representing the official policies or endorsements, either expressed or implied, of the Air Force Research Laboratory of the U.S. Government.

## Citation
If you use this framework/dataset, build on or find our research useful for your work please cite as, 
```
@misc{Risk-Mora2025continuationHYPAD,
  author       = {Risk‑Mora, David Y. and Aristizabal, Mauricio and Millwater, Harry and Restrepo, David},
  title        = {continuationHYPAD},
  year         = {2025},
  month        = {Mar},
  day          = {27},
  version      = {0.1},
  howpublished = {\url{https://github.com/AMMS-Lab-UTSA/continuationHYPAD}},
  note         = {GitHub repository}
}
```

## Overview
`continuationHYPAD` is a Python library for nonlinear finite element analysis using continuation methods. It enables efficient computation of post-buckling and critical buckling loads by leveraging hypercomplex automatic differentiation (HYPAD) within finite element models (FEM). The package provides a solver for nonlinear structural problems and includes utilities for sensitivity analysis, imperfection modeling, and reduced-order modeling.

### Features
- **Continuation Solver**: Implements an arc-length continuation method for nonlinear FEM problems.
- **Hypercomplex Automatic Differentiation (HYPAD)**: Enables efficient sensitivity analysis.
- **Imperfection Modeling**: Uses Chebyshev polynomials for reduced-order imperfection representation.

### Installation
To install `continuationHYPAD`, clone the repository and setup the environment using `conda`:
```bash
git clone https://github.com/davidyrisk/continuationHYPAD.git
cd continuationHYPAD
conda env create -f env.yml
conda activate pyoti-env
conda develop .
```

### Requirements
This package is tested on:
- **Operating System:** Ubuntu running under Windows Subsystem for Linux (WSL)
- **Python:** 3.8+
- In order, to execute the example script the raw data must be downloaded from <a href="https://utsacloud-my.sharepoint.com/:f:/g/personal/david_risk_my_utsa_edu/EqDZgOpZVaFGpHZTN22UnP8B-fdf26iqqkx8Fnyxak3miA?e=wQwKxK" style="color:#268cd7"> this link</a> and placed in the directory `continuationHYPAD/MC`.



### Repository Structure
After cloning the repository and downloading necessary data the structure should be:
```
continuationHYPAD/
│── MC/
│   ├── raw data...
│── continuationHYPAD/
│   ├── __init__.py
│   ├── continuationHYPAD.py
│   ├── utils.py
│── examples/
│   ├── example_arch.py
│── env.yml
│── README.md
│── LICENSE
```
Execute all scripts from the root directory `continuationHYPAD/.`

### Running Example Script
In order to run the example script
```bash
OMP_NUM_THREADS=1
PYTHONPATH=. python examples/example_arch.py 
```

## Function Documentation
### `continuationHYPAD.continuationSolver`
`continuationSolver` implements a nonlinear finite element method (FEM) solver using a continuation method. This function incrementally applies the load while updating the stiffness matrix to track the structure's response, even through instabilities. In order to access the full documentation please go to <a href="https://.../doi/..." style="color:#268cd7"> this link.</a>

#### Key Processes
1. **Initialization**:
   - Extracts mesh connectivity and nodal degrees of freedom.
   - Assembles the global stiffness matrix.
   - Sets up boundary conditions and external loads.

2. **Incremental Load Application**:
   - Iteratively applies load in small increments.
   - Updates the reference configuration for the Updated Lagrangian (UL) formulation.
   - Computes the tangent stiffness matrix.
   - Checks for singularities in the system.

3. **Predictor-Corrector Scheme**:
   - Predicts the next equilibrium state using a tangent stiffness update.
   - Corrects the solution iteratively until convergence is achieved.

4. **Convergence Check**:
   - Ensures the residual force is below the given tolerance.
   - Stops if convergence fails or the load ratio limit is reached.

5. **Postprocessing**:
   - Stores displacement and load ratio history for further analysis.
   - Allows exporting results for a selected degree of freedom.

This function enables efficient continuation-based solution tracking of nonlinear structural problems, making it suitable for postbuckling and stability analysis.

#### Usage
```python
import continuationHYPAD

# Define input parameters (example)
Coords = ...
Connect = ...
ndof = 3
Ebc = ...
Loads = ...
E = ...
A = ...
I = ...
export_node = 5

# Run the solver
U_step, lr_step, DOFexport = continuationHYPAD.continuationSolver(
    "frame", Coords, Connect, ndof, Ebc, Loads, E, A, I, export_node)
```

### Accessing Utility Functions `continuationHYPAD.utils`

#### Usage
You import utility functions:
```python
from continuationHYPAD.utils import function
result = function(args)
```

Or access them through the module:
```python
import continuationHYPAD.utils
result = continuationHYPAD.utils.function(args)
```

## License
This project is licensed, see the [LICENSE](LICENSE) file for details.
