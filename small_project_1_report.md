# Small Project 1: Configurable Fixed-Point FNT Butterfly and 16-Point FNT Core

## Overview

Starting from a basic FNT butterfly, I built a 16-point FNT/IFNT core. The goal was to verify finite-field addition, subtraction, multiplication, twiddle factors, and butterfly connectivity before assembling these units into a four-stage radix-2 DIT structure.

## What I implemented

- An FNT butterfly unit with modulus Q=12289;
- A 16-point FNT core supporting forward and inverse transforms;
- Four butterfly stages with eight butterflies per stage, 32 butterflies logically;
- Inter-stage registers and valid delays;
- Bit-reversed input reordering;
- N-inverse normalization in the inverse transform;
- A Python reference model and an Icarus Verilog testbench;
- Point-by-point automatic comparison of forward and inverse transforms.

## Implementation process

First, I chose Q=12289 because 12289-1 is divisible by 16, so a 16th root of unity can be constructed. The forward root is 4134, the inverse root is 10984, and the modular inverse of 16 is 11521.

Second, I implemented the butterfly:

t = b x w mod Q
y0 = a + t mod Q
y1 = a - t mod Q

Third, I loaded the 16 inputs in bit-reversed order into stage 0 and passed them through four radix-2 stages. Every stage stores its butterfly results in registers, allowing different data blocks to occupy different stages at the same time.

Fourth, I used the inverse root for the inverse transform and multiplied by the modular inverse of 16 at the end. Applying the inverse transform after the forward transform therefore recovers the original 16 symbols.

## Verification

This portable verification uses 16 deterministic grayscale-like test values as input. Python computed the reference FNT results, and the RTL testbench computed the same data. The simulation output was written to small1_results.csv and compared point by point:

- Forward transform: 16/16 frequency-domain values matched;
- Inverse transform: all 16 time-domain values recovered;
- Maximum error: 0; butterfly-unit smoke test: passed.

## Results and significance

This project established the core methods needed by the later projects: choosing finite-field parameters, implementing modular arithmetic, partitioning butterfly stages, inserting inter-stage registers, and automatically comparing Python and RTL results.

The current version is a fully unrolled 16-point educational design. It can be extended to 64 or 256 points, or the butterfly schedule can be changed to a resource-shared structure.

## File list

### Core RTL

- fnt_butterfly.v: standalone butterfly unit;
- fnt16_pipeline.v: four-stage 16-point FNT/IFNT pipeline;
- tb_fnt_butterfly.v: standalone butterfly testbench;
- tb_fnt16_pipeline.v: standalone 16-point FNT/IFNT testbench.

### Python and simulation driver

- scripts/run_small_project1.py: generate test data, compile RTL, run simulation, and compare automatically;
- sim/small1_input.mem: 16 input symbols;
- sim/small1.out: Icarus Verilog 16-point FNT simulation executable;
- sim/small1_butterfly.out: Icarus Verilog butterfly smoke-test executable;
- sim/small1_results.csv: RTL output for forward and inverse transforms.

### Shared files from later projects

- fnt_chain_top.v: integrated FNT, compensation-LUT, and IFNT top level;
- tb_fnt_chain.v: integrated-chain testbench;
- scripts/run_fnt_project.py: early 16-point chain verification script;
- fnt_seq_core.v: resource-shared FNT core supporting 16, 64, and 256 points;
- fnt_configurable_top.v: configurable top level with UART, bypass, and serial interfaces.

## Current limitations

This small project verifies finite-field FNT only; it is not a complete complex FFT or a physical fiber-channel model. Its purpose is to establish a correct hardware transform core, pipeline, and verification method for the later dispersion-compensation and configurable-system projects.
