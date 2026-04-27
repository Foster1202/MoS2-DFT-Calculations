# MoS2 Monolayer DFT Calculations

This directory contains the Quantum ESPRESSO input and output files used for a DFT study of monolayer MoS2.  The workflow covers geometry relaxation, SCF convergence checks, electronic band structure, density of states, phonons, and optical-response post-processing.

The `tmp` scratch directory and report/manuscript files are intentionally omitted from the uploaded version.  All important input files, selected output files, pseudopotentials, and processed data files are included for review.

## Main Files

- `MoS2.cif` - starting monolayer MoS2 structure.
- `mo_pbe_v1.uspp.F.UPF`, `s_pbe_v1.4.uspp.F.UPF` - PBE ultrasoft pseudopotentials used by the main calculations.
- `vc-relax.in`, `vc-relax.out` - variable-cell relaxation of the monolayer, with in-plane cell relaxation.
- `scf.in`, `scf.out` - final self-consistent calculation using the relaxed structure.
- `scf_band.in`, `scf_band.out`, `nscf_band.in`, `bands_pp.in`, `mos2_bands.dat*` - band-structure calculation and post-processing.
- `nscf_dos.in`, `dos_pp.in`, `mos2.dos` - density-of-states calculation and post-processing.
- `ph.mos2.in`, `q2r.in`, `matdyn.in`, `mos2.dyn*`, `mos2.fc`, `mos2.freq*` - phonon calculation and phonon-dispersion post-processing.
- `spectrum.in`, `optical/` - optical-response inputs and processed dielectric/EELS data.
- `conv_test_files/`, `ecut_energy.txt`, `kpts_energy.txt` - cutoff-energy and k-point convergence tests.

## Calculation Settings

The main production calculations use:

- Exchange-correlation functional: PBE
- Plane-wave cutoff: `ecutwfc = 60 Ry`
- Charge-density cutoff: `ecutrho = 600 Ry`
- SCF k-point mesh: `9 9 1`
- DOS k-point mesh: `16 16 1`
- Band and phonon path: `Gamma - M - K - Gamma`
- Vacuum spacing: approximately `17.3 Angstrom` along the out-of-plane direction

The convergence-test files show how the final cutoff and k-point settings were selected.

## Typical Run Order

From the project root, create a scratch directory before rerunning calculations:

```bash
mkdir -p tmp
```

Then run the Quantum ESPRESSO steps in this order:

```bash
pw.x < vc-relax.in > vc-relax.out
pw.x < scf.in > scf.out

pw.x < scf_band.in > scf_band.out
pw.x < nscf_band.in > nscf_band.out
bands.x < bands_pp.in > bands_pp.out

pw.x < nscf_dos.in > nscf_dos.out
dos.x < dos_pp.in > dos_pp.out

ph.x < ph.mos2.in > ph.mos2.out
q2r.x < q2r.in > q2r.out
matdyn.x < matdyn.in > matdyn.out
```

The optical-response data in `optical/` were generated separately from the included response-function files.

## Notes for Review

Most `.out`, `.dat`, `.freq`, and `.dyn` files are retained so the calculation results can be checked without rerunning the full workflow.  The large QE scratch directory is not needed for review and can be regenerated from the input files.
