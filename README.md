# 16-Point FNT Hardware Core

A pipelined 16-point finite-field number theoretic transform (FNT) hardware core with Verilog simulation and Python-based verification.

## Contents

- `fnt_butterfly.v` — finite-field butterfly unit
- `fnt16_pipeline.v` — pipelined 16-point datapath
- `tb_fnt_butterfly.v`, `tb_fnt16_pipeline.v` — Verilog testbenches
- `run_small_project1.py` — reproducible verification script
- simulation logs, vectors, and result tables
- `paper_fnt16-hardware-core_en.docx` — English design report

## Focus

Finite-field arithmetic, pipelined RTL design, simulation-based verification, and reproducible hardware experiments.

## Design report

[Read the English design report](./paper_fnt16-hardware-core_en.docx)

## Reproduce verification

Python 3 and Icarus Verilog (iverilog and vvp on PATH). Install NumPy with `python -m pip install numpy`.

Run `python run_small_project1.py` from the repository root. The script regenerates simulation outputs using deterministic test vectors. It does not require files from the author's computer.

## Scope

These are educational RTL simulations. Passing simulation is not a claim of FPGA timing closure, board-level validation, or a physical fiber-channel model.
