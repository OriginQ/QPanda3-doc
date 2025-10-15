Quantum Profiling {#tutorial_quantum_profiling}
===============

[TOC]

@prev_tutorial{tutorial_hamiltonian}
@next_tutorial{tutorial_qcloud_service}

---

# Profiling Module Tutorial

This tutorial provides an overview of the profiling capabilities available for quantum circuits using the functions [draw_circuit_features](@ref pyqpanda3.profiling.profiling.draw_circuit_features) and [draw_circuit_profile](@ref pyqpanda3.profiling.profiling.draw_circuit_profile). These functions allow users to visualize circuit features and performance metrics effectively.

## Overview

Quantum circuits are complex structures that often require detailed analysis to understand their performance characteristics. The [Profiling](@ref pyqpanda3.profiling.profiling) module offers tools to visualize these aspects through radar charts for circuit features and performance profiles for execution times.

## draw_circuit_features

[draw_circuit_features](@ref pyqpanda3.profiling.profiling.draw_circuit_features) visualizes the features of a quantum circuit by generating a radar chart. It computes several metrics related to the circuit's structure and behavior, including connectivity, liveness, parallelism, entanglement, and critical depth.

## draw_circuit_profile

[draw_circuit_profile](@ref pyqpanda3.profiling.profiling.draw_circuit_profile) generates a performance profile of a quantum circuit, illustrating the execution times of individual quantum gates. It helps in identifying bottlenecks and optimizing performance.

## Example Usage

@add_toggle_python
@snippet samples/python/profiling.py Profiling sample
@end_toggle

After running, two images will be generated as shown below:

![Quantum Circuit Features](images/profiling/profiling_1.png){html: width=50%, latex: width=10cm}

![Quantum Circuit Profile](images/profiling/profiling_2.png){html: width=30%, latex: width=5cm}

@note
- To use `draw_circuit_profile`, you need to install the Graphviz library first.
