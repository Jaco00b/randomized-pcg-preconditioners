# Randomized Preconditioners for Conjugate Gradient

Bachelor thesis project (DTU, Department of Applied Mathematics and Computer
Science, 2025): **Iterative Solvers for Large Linear Systems of Equations**.
Full thesis: [thesis.pdf](thesis.pdf)

Compares two randomized preconditioners for solving large regularized linear
systems `(A + uI)x = b` with the Preconditioned Conjugate Gradient (PCG)
method, where `A` is a symmetric positive semidefinite Gaussian kernel matrix:

- **Randomized Nystrom preconditioner** — low-rank approximation via a
  Gaussian sketch (implemented from the pseudocode in Frangella, Tropp &
  Udell, *Randomized Nystrom Preconditioning*, SIAM J. Matrix Anal. Appl., 2023)
- **RPCholesky preconditioner** — randomly pivoted partial Cholesky
  (Chen, Epperly, Tropp & Webber, arXiv:2207.06503)

**Main finding:** Both preconditioners cut CG iteration counts substantially,
but the Nystrom sketch becomes expensive to construct as the rank grows.
RPCholesky achieves similar or better iteration reductions at up to ~60x lower
preconditioner construction time, making it the more practical choice for
Gaussian kernel systems.

## Structure

| File | Description |
|---|---|
| `Experiment.py` | Runs the full experiment suite and generates plots |
| `PCG.py` | Preconditioned CG solver (supports both preconditioners) |
| `RandomizedNystrom.py` | Randomized Nystrom approximation (Algorithm 1 in the thesis) |
| `RPCholesky.py` | RPCholesky and accelerated/block variants (adapted from the RPCholesky paper's reference code) |
| `HelperFunctions/` | Kernel matrices, PSD matrix classes, low-rank factorizations, test matrix gallery |
| `plots/` | Generated figures |

## Running

Requires Python 3 with `numpy`, `scipy`, `matplotlib`, `scikit-learn`, `tqdm`.

```
python Experiment.py smile    # Gaussian kernel matrix from smiley-face data
python Experiment.py spiral   # Gaussian kernel matrix from spiral data
python Experiment.py both
```

Each experiment runs 50 randomized instances per sketch size and reports
iteration distributions (boxplots + histograms) comparing the two
preconditioners.
