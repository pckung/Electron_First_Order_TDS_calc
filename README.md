# First-Order TDS at Example Zone Axes (Ni)

A minimal, self-contained Jupyter notebook that computes the **first-order
(one-phonon) thermal diffuse scattering (TDS)** intensity of FCC nickel at a
handful of common zone axes ([001], [011], [111], [112]), starting from a
`phonopy` phonon calculation.

## Contents

| File | Description |
|---|---|
| [`TDS_zone_axis_example.ipynb`](./TDS_zone_axis_example.ipynb) | The example notebook — phonon dispersion, first-order TDS theory, and zone-axis TDS maps. |
| `POSCAR` | Conventional (4-atom) FCC Ni unit cell, VASP format, `a = 3.518 Å`. |
| `FORCE_SETS` | Harmonic force-constant dataset (`phonopy` format) for a 3×3×3 supercell of the above cell. |

No other input files are needed — all material parameters (electron
scattering-factor coefficients, Debye-Waller model fit parameters, atomic mass)
are hard-coded in the notebook itself.

## What the notebook does

1. Loads the harmonic phonons from `POSCAR` + `FORCE_SETS` with `phonopy`
   (`primitive_matrix="auto"` resolves the 1-atom rhombohedral FCC primitive
   cell).
2. Plots the phonon dispersion along the standard FCC path
   (Γ–X–W–X′–K–Γ–L) as a sanity check.
3. Implements the first-order TDS intensity

   $$
   I_1^{TDS}(\mathbf{q}) =  \frac{N\hbar f(\mathbf{q})^2 e^{-2M(\mathbf{q})}}{4\mu} \sum_{\mathbf{k}}\left[\sum_j\frac{1}{\omega_{\mathbf{k},j}}\coth\left(\frac{\hbar\omega_{\mathbf{k},j}}{2k_BT}\right) |2\pi\mathbf{q}\cdot\mathbf{\hat{e}}_{\mathbf{k},j}|^2\right] _{\mathbf{k}=\mathbf{q}\pm \mathbf{g_q}}
   $$

   with the Debye-Waller factor following Sears & Shelley (1991),
   $2W = 2B(\sin\theta/\lambda)^2$.
4. Builds a dense 3D grid of scattering vectors once, then obtains each zone
   axis by slicing out the thin slab through the origin perpendicular to that
   zone axis and rotating it into an in-plane (x, y) frame.
5. Plots the resulting first-order TDS intensity for four example zone axes,
   with Bragg spots overlaid for reference, and demonstrates re-evaluating the
   same slice at different temperatures.

## Requirements

```bash
pip install numpy scipy matplotlib phonopy jupyterlab ipykernel
```

Tested with:

```
numpy==2.4.6
scipy==1.17.1
matplotlib==3.11.2
phonopy==4.6.0
```

## Running it

Run all cells top to bottom. `POSCAR` and `FORCE_SETS` must stay alongside the
notebook (the default paths passed to `phonopy.load`).
