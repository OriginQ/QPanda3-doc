Welcome to QPanda3
====================

[TOC]

<table>
  <thead>
    <tr>
      <th style="font-size: 20px;">[中文](https://qcloud.originqc.com.cn/document/qpanda-3/cn/index.html)</th>
      <th style="font-size: 20px; background: #ffd70033;">[English](https://qcloud.originqc.com.cn/document/qpanda-3/index.html) </th>
    </tr>
  </thead>
</table>



**QPanda3** (Quantum Programming Architecture for NISQ Device Application v3) is a high-performance quantum programming framework that enhances quantum computing efficiency through optimized circuit compilation, an advanced instruction stream format (OriginBIS), and hardware-aware execution strategies. These engineering optimizations significantly improve both processing speed and system performance, addressing key challenges in the NISQ era. A core innovation, OriginBIS, accelerates encoding speeds by up to 86.9× compared to OpenQASM 2.0, while decoding is 35.6× faster, leading to more efficient data handling, reduced memory overhead, and improved communication efficiency. This directly enhances the execution of quantum circuits, making large-scale quantum simulations more feasible. Comprehensive benchmarking demonstrates QPanda3’s superior performance: quantum circuit construction is 1.30× faster, execution speeds improve by 1.03×, and transpilation efficiency increases by 10.70× over Qiskit. Notably, in compiling a 118-qubit W-state circuit on a 2D-grid topology, QPanda3 achieves an unprecedented 597.41× speedup, underscoring its ability to handle complex quantum workloads at scale. By combining high-speed quantum processing with a modular and extensible software architecture, QPanda3 provides a practical bridge between today’s NISQ devices and future fault-tolerant quantum computing. It facilitates real-world applications in financial modeling, materials science, and combinatorial optimization, while its robust and scalable design supports industrial adoption and cloud-based deployment.

> QPanda3 is a quantum computing library designed to provide developers with tools and interfaces related to quantum computing. 

### Overview    
  
- Enhancing the computational capabilities of existing quantum devices
  - Through efficient compilation techniques, optimized qubit mapping strategies, and quantum gate compression algorithms, QPanda3 enables NISQ devices to execute target algorithms more efficiently, achieving superior computational performance for specific tasks.
  - It supports fast [quantum circuit transpilation](#tutorial_quantum_circuit_transpiler), which is  **faster** than Qiskit 2.0.1. For more details, see [QPanda3 vs Qiskit 2.0.1](#welcome_benchpress_compare_ref_id) .
  - It supports high-performance simulation of quantum circuits, based on the technological foundation of QPanda2's underlying simulation computations.

- Optimizing software-hardware co-design in quantum computing
  - Unlike classical computing, quantum computing requires software to have deep awareness of hardware characteristics. QPanda3 employs adaptive qubit mapping and hardware-aware compilation strategies to ensure optimal execution across different quantum processors, thereby improving overall computational fidelity.
  - QPanda3 supports all types of [quantum logic gates](#tutorial_quantum_gate), including single-qubit gates, two-qubit gates, controlled gates, and Oracle gates.
  - QPanda3 supports all [fundamental quantum simulators](#tutorial_quantum_simulator), including single-amplitude simulators, partial-amplitude simulators, full-amplitude simulators, noise simulators, density matrix simulators, and GPU simulators, providing a unified execution method.
  - It supports [quantum circuit profiling](#tutorial_quantum_profiling), enabling the development of more efficient quantum programs and quantum algorithms.
  - QPanda3 fully supports the relevant functionalities of [Variational Quantum Circuit](#tutorial_variational_quantum_circuit), including the construction of parameterized quantum circuits, reuse of variational quantum circuit structures, the use of expressions as gate parameters for parameterized quantum gates, generation of quantum circuits with given parameter values, calculation of Hamiltonian expectations, calculation of gradients, and other practical features. [QPanda3 demonstrates high efficiency in calculating Hamiltonian expectations and gradient values](#welcome_vqc_cal_gradient_compare_ref_id).

- Advancing quantum computational advantages
  - Although some NISQ devices have demonstrated computational superiority for certain problems, achieving large-scale quantum computing necessitates further engineering breakthroughs. QPanda3 adopts a modular architecture, allowing seamless integration with future fault-tolerant quantum computing systems, thereby reducing the transition cost from NISQ to next-generation quantum technologies.



    
### Benchmark

#### Compilation Efficiency{#welcome_benchpress_compare_ref_id}

Experiments on [benchpress](https://github.com/Qiskit/benchpress), in a fully connected topology, QPanda3's compilation speed is, on average, 7.03× faster than Qiskit, with peak acceleration reaching 123.33×. In a square topology, its average compilation speed surpasses Qiskit by 15.87×, with a peak of 597.41×. For heavy-hexagon topologies, QPanda3 achieves an average 12.45× speedup, with a peak of 313.86×. In linear topology, the average speedup is 7.43×, with peak acceleration reaching 374.48×. We have introduced more detailed experimental content in the literature  [QPanda3: A High-Performance Software-Hardware Collaborative Framework for Large-Scale Quantum-Classical Computing Integration](
https://doi.org/10.48550/arXiv.2504.02455). The webpage [Benchmark](#benchmark) cites the latest experimental results from relevant studies.


![QPanda3 vs Qiskit 2.0.1](images/qpanda3_vs_qiskit_benchpress_runtime.png)

#### Gradient Calculation Efficiency{#welcome_vqc_cal_gradient_compare_ref_id}

The figure **Gradient Calculation Time Comparison** shows the comparison results of the average time required by QPanda3 and other quantum computing-related SDKs (DeepQuantum, PennyLane-Lightning, PennyLane-Default, qiskit) to complete the gradient calculation tasks for the same variational quantum circuits (the x-axis represents different variational quantum circuits determined by the total number of qubits and the number of circuit layers). This figure indicates that QPanda3 has a significant advantage in terms of gradient calculation efficiency for variational quantum circuits.

![Gradent calcuaton Time Comparison](images/vqc_calculate_gradient_time_compare.png)

### How to use

pyqpanda3 is a high-performance Python quantum programming library built on the QPanda3 core and wrapped with pybind11.

It exposes QPanda3’s core functionalities seamlessly through Python interfaces, enabling developers to enjoy the simplicity and flexibility of Python while maintaining the high-efficiency execution performance of the C++ core.

Thanks to the zero-overhead binding mechanism of pybind11, pyqpanda3 incurs almost no performance loss when invoking quantum circuit compilation, simulation, and cloud execution in Python. This design also lowers the learning curve and reduces the barrier to entry.

pyqpanda3 fully inherits QPanda3’s optimized compiler, hardware-aware mapping, quantum simulators, and cloud computing service interfaces, supporting end-to-end development from local simulation to execution on real quantum hardware. Its modular architecture and unified API design allow users to implement complex quantum algorithms with less code, achieving both research efficiency and engineering performance.

pyqpanda3 has the following system environment requirements:

#### Windows

| Software                                | Version          |
|-----------------------------------------|------------------|
| [Microsoft Visual C++ Redistributable x64](https://aka.ms/vs/17/release/vc_redist.x64.exe) | 2015-2022             |
| Python                                  | >= 3.9 && <= 3.13 |

#### Linux

| Software | Version          |
|----------|------------------|
| GCC      | >=8.0         |
| Python   | >= 3.9 && <= 3.13 |

#### Mac

| Software | Version          |
|----------|------------------|
| clang      | >=15.0         |
| Python   | >= 3.9 && <= 3.13 |


Before using pyqpanda3, you need to install the corresponding Python-dependent library. pyqpanda3 supports installation via `pip`, and its installation command is as follows:
@code{.bash}
pip install pyqpanda3
@endcode

After pyqpanda3 is installed, it can be used directly. Here is a simple usage example:

@code{python}
from pyqpanda3.core import QCircuit, QProg, H, CNOT, measure, CPUQVM

# Create a quantum circuit
circuit = QCircuit()

# Construct GHZ state
circuit << H(0)         # Apply Hadamard gate on qubit 0
circuit << CNOT(0,1)    # Apply CNOT gate with control 0 and target 1
circuit << CNOT(1,2)    # Apply CNOT gate with control 1 and target 2

# Create a quantum prog and compose the circuit
prog = QProg()
prog << circuit

# Add measure operation 
prog << measure(0,0) << measure(1,1) << measure(2,2)

# Create a QVM
qvm = CPUQVM()

# Execute the prog and obtain the result
qvm.run(prog,1000)
result = qvm.result().get_counts()

# Print qprog and result
print(prog)
print(result)

@endcode

The running result is as follows:
@code{.bash}
          ┌─┐               ┌─┐         
q_0:  |0>─┤H├ ───*── ───────┤M├ ─────── 
          └─┘ ┌──┴─┐        └╥┘  ┌─┐    
q_1:  |0>──── ┤CNOT├ ───*────╫─ ─┤M├─── 
              └────┘ ┌──┴─┐  ║   └╥┘┌─┐ 
q_2:  |0>──── ────── ┤CNOT├──╫─ ──╫─┤M├ 
                     └────┘  ║    ║ └╥┘ 
 c :   / ════════════════════╩════╩══╩═
                              0    1  2
{'111': 489, '000': 511}
@endcode
