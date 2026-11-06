# Promela & Spin Workflow

A small project demonstrating the Spin/JSpin toolchain on Linux: create a Promela model, simulate process execution, generate a C verifier, and inspect automata using Graphviz.

Authors: Qiaoyu Wan, Yiwen Wei, Shuhan Zhou and Ieun Kim.

## Model

[`models/hello.pml`](models/hello.pml) defines a `main` process that prints `hello,world!`. An `init` process launches it with `run main()`. The example illustrates process creation, execution and termination.

This repository demonstrates the verification toolchain. It does not implement a network protocol, message channels, retransmission or custom temporal-logic properties.

## Run a simulation

Prerequisites: Spin on your PATH. For verification, also install a C compiler (`cc` or GCC). JSpin requires Java; graphical output requires Graphviz.

```sh
spin models/hello.pml
```

An archived simulation trace is included in [`examples/simulation.txt`](examples/simulation.txt). It shows the `init` process creating `main`, the print action, and both processes terminating. It is historical output, not a new run on the current machine.

## Generate and run a verifier

Run from the repository root:

```sh
mkdir -p build
cp models/hello.pml build/hello.pml
cd build
spin -a hello.pml
cc -O2 -DSAFETY -o pan pan.c
./pan
```

Spin generates `pan.c` and companion files. They are tool-generated verifier artifacts, not manually written project source. Build products are kept outside the tracked source tree. This model contains no explicit assertions or LTL claims; a successful run is not a proof of network protocol correctness.

## JSpin and automata

Open `models/hello.pml` in a configured JSpin installation to explore simulation and verification. The original workflow used JSpin/SpinSpider and Graphviz for automata visualization.

The supplied graph export reused node IDs across processes, so it is not included as an authoritative state diagram. Regenerate a graph from the model with the configured tools instead.

## Project preparation

The source model is unchanged. A historical trace originally named `hello.dot` was renamed to `simulation.txt` because it contains execution text, not DOT graph syntax. The original report describes setup in an Ubuntu virtual machine with VMware, Spin, JSpin, Java and Graphviz.

Reports and the large virtual-machine archive are excluded because they contain local credentials and machine-specific information. Generated C files and the compiled verifier are also excluded. Spin and Graphviz were not installed on the preparation machine, so the commands above have not been re-executed there.
