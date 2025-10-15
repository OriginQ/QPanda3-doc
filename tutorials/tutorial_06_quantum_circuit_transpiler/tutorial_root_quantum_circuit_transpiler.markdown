Quantum Circuit Transpiler {#tutorial_quantum_circuit_transpiler}
==========================

[TOC]

@prev_tutorial{tutorial_noise}
@next_tutorial{tutorial_quantum_compilation}

---

# Transpiler Module Tutorial

This tutorial provides an overview of the functionalities available in the [Transpiler](@ref pyqpanda3.transpilation.Transpiler) module, which includes generating circuit topologies, transpiling quantum programs, and decomposing quantum circuits. These operations are essential for optimizing and adapting quantum algorithms to specific hardware architectures.

## 1. Overview

The `Transpiler` module is designed to facilitate the preparation of quantum programs for execution by generating topologies, transpiling programs to fit specific architectures, and decomposing circuits into simpler components.

## 2. Functions and Classes

### 2.1 Transpiler Class

The [Transpiler](@ref pyqpanda3.transpilation.Transpiler) class is responsible for transpiling quantum programs to fit a specific topology and optimization level.

#### Constructor

Initializes a new instance of the [Transpiler](@ref pyqpanda3.transpilation.Transpiler.__init__) class.

Here is an example of how to constructor `Transpiler`.
@add_toggle_python

```python
from pyqpanda3.transpilation import Transpiler
transpiler = Transpiler()
```

@end_toggle

### Methods

#### transpile

[transpile](@ref pyqpanda3.transpilation.Transpiler.transpile) is an interface used to compile a quantum program (QProg) onto a specified backend topology through multiple compilation processes (passes). This process usually includes the following steps:
- Optimizing quantum circuits: By using different optimization strategies, the number and complexity of quantum gates can be reduced, thereby improving the execution efficiency of quantum programs.
- Mapping to a specified topology: Based on the topology of the target quantum computer, map qubits to actual physical qubits. This step takes into account the connection limitations of quantum computers, ensuring that quantum gates are physically achievable.
- Transform quantum gates: Convert high-level quantum gates in a quantum subroutine into low-level gates supported by the target hardware. This may involve the decomposition of some gates to adapt to the capabilities of the target hardware

@note
- **Chip Topology edges**: If `chip_topology_edges` is empty, the `layoutPass` and `routingPass` will be skipped during the transpilation process. This can affect the optimization and performance of the resulting circuit, so it is important to provide a valid chip topology when applicable.
- **Optimization Levels**:
    - `0`: No optimization is performed.
    - `1`: Basic optimizations are applied, including simple two-qubit gate cancellation and single-qubit gate merging.
    - `2`: Advanced optimizations are performed, including matrix optimization, two-qubit gate cancellation, and single-qubit gate merging.

Here is an example of how to use the `Transpiler` class to transpile a quantum circuit:

@add_toggle_python
@snippet samples/python/transpiler.py Transpiler sample
@end_toggle

**Run result:**

```python
prog: 
 
          ┌─┐ ┌──┐                           ┌─┐         
q_0:  |0>─┤H├ ┤RX├ ───*── ──── ───*── ───────┤M├ ─────── 
          └─┘ └──┘ ┌──┴─┐ ┌──┐ ┌──┴─┐        └╥┘  ┌─┐    
q_1:  |0>──── ──── ┤CNOT├ ┤U1├ ┤CNOT├ ───*────╫─ ─┤M├─── 
                   └────┘ └──┘ └────┘ ┌──┴─┐  ║   └╥┘┌─┐ 
q_2:  |0>──── ──── ────── ──── ────── ┤CNOT├──╫─ ──╫─┤M├ 
                                      └────┘  ║    ║ └╥┘ 
 c :   / ═════════════════════════════════════╩════╩══╩═
                                               0    1  2


Transpiler lavel 0: 
 
          ┌──┐ ┌──┐ ┌──┐                                                   ┌─┐                         >
q_0:  |0>─┤RY├ ┤RX├ ┤RX├ ──*─ ──── ──── ──── ──── ──── ──── ──── ──*─ ─────┤M├ ──── ──── ──── ──────── >
          ├──┤ ├──┤ ├──┤ ┌─┴┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌─┴┐ ┌──┐ └╥┘ ┌──┐ ┌──┐           ┌─┐ >
q_1:  |0>─┤RZ├ ┤RX├ ┤RZ├ ┤CZ├ ┤RZ├ ┤RX├ ┤RZ├ ┤RZ├ ┤RZ├ ┤RX├ ┤RZ├ ┤CZ├ ┤RZ├──╫─ ┤RX├ ┤RZ├ ──*─ ─────┤M├ >
          ├──┤ ├──┤ ├──┤ └──┘ └──┘ └──┘ └──┘ └──┘ └──┘ └──┘ └──┘ └──┘ └──┘  ║  └──┘ └──┘ ┌─┴┐ ┌──┐ └╥┘ >
q_2:  |0>─┤RZ├ ┤RX├ ┤RZ├ ──── ──── ──── ──── ──── ──── ──── ──── ──── ──────╫─ ──── ──── ┤CZ├ ┤RZ├──╫─ >
          └──┘ └──┘ └──┘                                                    ║            └──┘ └──┘  ║  >
 c :   / ═══════════════════════════════════════════════════════════════════╩═══════════════════════╩═
                                                                             0                       1

                        
q_0:  |0>──── ──── ──── 
                        
q_1:  |0>──── ──── ──── 
         ┌──┐ ┌──┐  ┌─┐ 
q_2:  |0>┤RX├ ┤RZ├ ─┤M├ 
         └──┘ └──┘  └╥┘ 
 c :   / ════════════╩═
                      2


Transpiler lavel 1: 
 
          ┌──┐ ┌──┐ ┌──┐                                                   ┌─┐                         >
q_0:  |0>─┤RZ├ ┤RX├ ┤RZ├ ──*─ ──── ──── ──── ──── ──── ──── ──── ──*─ ─────┤M├ ──── ──── ──── ──────── >
          ├──┤ ├──┤ ├──┤ ┌─┴┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌─┴┐ ┌──┐ └╥┘ ┌──┐ ┌──┐           ┌─┐ >
q_1:  |0>─┤RZ├ ┤RX├ ┤RZ├ ┤CZ├ ┤RZ├ ┤RX├ ┤RZ├ ┤RZ├ ┤RZ├ ┤RX├ ┤RZ├ ┤CZ├ ┤RZ├──╫─ ┤RX├ ┤RZ├ ──*─ ─────┤M├ >
          ├──┤ ├──┤ ├──┤ └──┘ └──┘ └──┘ └──┘ └──┘ └──┘ └──┘ └──┘ └──┘ └──┘  ║  └──┘ └──┘ ┌─┴┐ ┌──┐ └╥┘ >
q_2:  |0>─┤RZ├ ┤RX├ ┤RZ├ ──── ──── ──── ──── ──── ──── ──── ──── ──── ──────╫─ ──── ──── ┤CZ├ ┤RZ├──╫─ >
          └──┘ └──┘ └──┘                                                    ║            └──┘ └──┘  ║  >
 c :   / ═══════════════════════════════════════════════════════════════════╩═══════════════════════╩═
                                                                             0                       1

                        
q_0:  |0>──── ──── ──── 
                        
q_1:  |0>──── ──── ──── 
         ┌──┐ ┌──┐  ┌─┐ 
q_2:  |0>┤RX├ ┤RZ├ ─┤M├ 
         └──┘ └──┘  └╥┘ 
 c :   / ════════════╩═
                      2


Transpiler lavel 2: 
 
          ┌──┐ ┌──┐ ┌──┐                                                   ┌─┐                         >
q_0:  |0>─┤RZ├ ┤RX├ ┤RZ├ ──*─ ──── ──── ──── ──── ──── ──── ──── ──*─ ─────┤M├ ──── ──── ──── ──────── >
          ├──┤ ├──┤ ├──┤ ┌─┴┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌─┴┐ ┌──┐ └╥┘ ┌──┐ ┌──┐           ┌─┐ >
q_1:  |0>─┤RZ├ ┤RX├ ┤RZ├ ┤CZ├ ┤RZ├ ┤RX├ ┤RZ├ ┤RZ├ ┤RZ├ ┤RX├ ┤RZ├ ┤CZ├ ┤RZ├──╫─ ┤RX├ ┤RZ├ ──*─ ─────┤M├ >
          ├──┤ ├──┤ ├──┤ └──┘ └──┘ └──┘ └──┘ └──┘ └──┘ └──┘ └──┘ └──┘ └──┘  ║  └──┘ └──┘ ┌─┴┐ ┌──┐ └╥┘ >
q_2:  |0>─┤RZ├ ┤RX├ ┤RZ├ ──── ──── ──── ──── ──── ──── ──── ──── ──── ──────╫─ ──── ──── ┤CZ├ ┤RZ├──╫─ >
          └──┘ └──┘ └──┘                                                    ║            └──┘ └──┘  ║  >
 c :   / ═══════════════════════════════════════════════════════════════════╩═══════════════════════╩═
                                                                             0                       1

                        
q_0:  |0>──── ──── ──── 
                        
q_1:  |0>──── ──── ──── 
         ┌──┐ ┌──┐  ┌─┐ 
q_2:  |0>┤RX├ ┤RZ├ ─┤M├ 
         └──┘ └──┘  └╥┘ 
 c :   / ════════════╩═
                      2

```


If the `chip_topology_edges` parameters are not passed, routing pass will be skipped.
If the `basic_gates` parameters are not passed, translation pass will be skipped.


@add_toggle_python
@snippet samples/python/transpiler.py Transpiler no chip_topology_edges sample
@end_toggle

**Run result:**

```python
no chip_topology_edges prog: 
 
          ┌──┐ ┌──┐ ┌──┐                                                   ┌─┐                         >
q_0:  |0>─┤RZ├ ┤RX├ ┤RZ├ ──*─ ──── ──── ──── ──── ──── ──── ──── ──*─ ─────┤M├ ──── ──── ──── ──────── >
          ├──┤ ├──┤ ├──┤ ┌─┴┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌─┴┐ ┌──┐ └╥┘ ┌──┐ ┌──┐           ┌─┐ >
q_1:  |0>─┤RZ├ ┤RX├ ┤RZ├ ┤CZ├ ┤RZ├ ┤RX├ ┤RZ├ ┤RZ├ ┤RZ├ ┤RX├ ┤RZ├ ┤CZ├ ┤RZ├──╫─ ┤RX├ ┤RZ├ ──*─ ─────┤M├ >
          ├──┤ ├──┤ ├──┤ └──┘ └──┘ └──┘ └──┘ └──┘ └──┘ └──┘ └──┘ └──┘ └──┘  ║  └──┘ └──┘ ┌─┴┐ ┌──┐ └╥┘ >
q_2:  |0>─┤RZ├ ┤RX├ ┤RZ├ ──── ──── ──── ──── ──── ──── ──── ──── ──── ──────╫─ ──── ──── ┤CZ├ ┤RZ├──╫─ >
          └──┘ └──┘ └──┘                                                    ║            └──┘ └──┘  ║  >
 c :   / ═══════════════════════════════════════════════════════════════════╩═══════════════════════╩═
                                                                             0                       1

                        
q_0:  |0>──── ──── ──── 
                        
q_1:  |0>──── ──── ──── 
         ┌──┐ ┌──┐  ┌─┐ 
q_2:  |0>┤RX├ ┤RZ├ ─┤M├ 
         └──┘ └──┘  └╥┘ 
 c :   / ════════════╩═
                      2
```

If the init_mappings parameter is passed in, then routingPasses will be performed based on init_mappings.

@add_toggle_python
@snippet samples/python/transpiler.py Transpiler with init_mappings sample
@end_toggle

**Run result:**

```python
init_mapping prog: 
 
          ┌──┐ ┌──┐ ┌──┐                                                             ┌──┐ ┌──┐ ┌──┐ ┌──┐  ┌─┐ 
q_0:  |0>─┤RZ├ ┤RX├ ┤RZ├ ──── ──── ──── ──── ──── ──── ──── ──── ──── ──── ──── ──── ┤CZ├ ┤RZ├ ┤RX├ ┤RZ├ ─┤M├ 
          ├──┤ ├──┤ ├──┤ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ └─┬┘ └┬─┤ └──┘ └──┘  └╥┘ 
q_1:  |0>─┤RZ├ ┤RX├ ┤RZ├ ┤CZ├ ┤RZ├ ┤RX├ ┤RZ├ ┤RZ├ ┤RZ├ ┤RX├ ┤RZ├ ┤CZ├ ┤RZ├ ┤RX├ ┤RZ├ ──*─ ─┤M├ ──── ──── ──╫─ 
          ├──┤ ├──┤ ├──┤ └─┬┘ └──┘ └──┘ └──┘ └──┘ └──┘ └──┘ └──┘ └─┬┘ └┬─┤ └──┘ └──┘       └╥┘             ║  
q_2:  |0>─┤RZ├ ┤RX├ ┤RZ├ ──*─ ──── ──── ──── ──── ──── ──── ──── ──*─ ─┤M├ ──── ──── ──── ──╫─ ──── ──── ──╫─ 
          └──┘ └──┘ └──┘                                               └╥┘                  ║              ║  
 c :   / ═══════════════════════════════════════════════════════════════╩═══════════════════╩══════════════╩═
        

```
#### Overloaded `transpile` Method
The [transpile](@ref pyqpanda3.transpilation.Transpiler.transpile) interface has an overloaded function that is used to convert a quantum program into a format suitable for a specific quantum chip backend and optimize it according to the specified optimization level.

The parameters are described as follows:
- **`prog`**: The quantum program to be converted.
- **`backend`**: The backend configuration of the quantum chip. This parameter defines the chip's topology, supported gate operations, and other relevant information.
- **`init_mapping`**: The initial mapping, which is the relationship mapping from virtual qubits to physical qubits. The default is an empty mapping.
- **`optimization_level`**: The optimization level, which specifies the optimization strategy to be applied during the conversion process. The default value is 2.

This interface can be used in conjunction with `to_instruction` and `run_instruction` to compile a quantum program to a specified chip on the cloud platform, convert it into an instruction file, and run it on the chip.
Here is an example code for the complete process:


```python
from pyqpanda3.intermediate_compiler import convert_originir_file_to_qprog, convert_originir_string_to_qprog
from pyqpanda3.qcloud.qcloud import ChipBackend
from pyqpanda3.qcloud import QCloudService, LogOutput, QCloudOptions
from pyqpanda3.transpilation import Transpiler
from pyqpanda3.core import *

api_key = "" # Replace with your own key


def run_instruction_test(prog_1: QProg):
    # Obtain the service
    service = QCloudService(api_key)

    # Get the specified chip
    backend_wukong = service.backend("72")
    chip_info = backend_wukong.chip_info()

    # Get the chip backend
    backend: ChipBackend = chip_info.get_chip_backend()

    # Compile according to the chip information
    transpiler = Transpiler()
    prog_2 = transpiler.transpile(prog_1, backend, {}, 2)

    # Convert to chip instruction
    use_pattern = False
    ins = prog_2.to_instruction(backend, 1, use_pattern)

    # Run on the chip
    options = QCloudOptions()
    options.set_mapping(False)
    options.set_amend(False)

    job = backend_wukong.run_instruction([ins], 1000, options)
    result = job.result()
    print(result.get_probs_list())


# Test function
if __name__ == "__main__":
    ir = """
QINIT 8
CREG 8
H q[0]
H q[1]
H q[2]
H q[3]
H q[4]
H q[5]
CNOT q[0],q[4]
RZ q[4],(0.93926449)
CNOT q[0],q[4]
CNOT q[0],q[5]
RZ q[5],(0.42459902)
CNOT q[0],q[5]
CNOT q[1],q[4]
RZ q[4],(0.88779795)
CNOT q[1],q[4]
CNOT q[1],q[5]
RZ q[5],(0.46319893)
CNOT q[1],q[5]
CNOT q[2],q[5]
RZ q[5],(1.132264)
CNOT q[2],q[5]
CNOT q[3],q[5]
RZ q[5],(0.86206467)
CNOT q[3],q[5]
RX q[0],(2.3543679)
RX q[1],(2.3543679)
RX q[2],(2.3543679)
RX q[3],(2.3543679)
RX q[4],(2.3543679)
RX q[5],(2.3543679)
MEASURE q[0],c[0]
MEASURE q[1],c[1]
MEASURE q[2],c[2]
MEASURE q[3],c[3]
MEASURE q[4],c[4]
MEASURE q[5],c[5]
    """
    prog_1 = convert_originir_string_to_qprog(ir)
    run_instruction_test(prog_1)

```

The execution result is as follows:
```python
[{'0x0': 0.077, '0x1': 0.007, '0x10': 0.032, '0x11': 0.005, '0x12': 0.043, '0x13': 0.006, 
'0x14': 0.009, '0x15': 0.002, '0x16': 0.018, '0x17': 0.005, '0x18': 0.022, '0x19': 0.002,
'0x1a': 0.018, '0x1b': 0.005, '0x1c': 0.018, '0x1d': 0.002, '0x1e': 0.025, '0x1f': 0.003, 
'0x2': 0.084, '0x20': 0.036, '0x21': 0.004, '0x22': 0.039, '0x23': 0.003, '0x24': 0.013, 
'0x26': 0.011, '0x27': 0.006, '0x28': 0.03, '0x29': 0.004, '0x2a': 0.024, '0x2b': 0.002, 
'0x2c': 0.016, '0x2d': 0.006, '0x2e': 0.029, '0x2f': 0.006, '0x3': 0.018, '0x30': 0.014, 
'0x31': 0.003, '0x32': 0.019, '0x33': 0.005, '0x34': 0.006, '0x35': 0.002, '0x36': 0.009, 
'0x37': 0.002, '0x38': 0.009, '0x39': 0.002, '0x3a': 0.012, '0x3b': 0.001, '0x3c': 0.008, 
'0x3d': 0.001, '0x3e': 0.016, '0x3f': 0.001, '0x4': 0.023, '0x5': 0.002, '0x6': 0.03, 
'0x7': 0.012, '0x8': 0.042, '0x9': 0.008, '0xa': 0.042, '0xb': 0.004, '0xc': 0.038, 
'0xd': 0.005, '0xe': 0.046, '0xf': 0.008}]
```



### 2.2 generate_topology

[generate_topology](@ref pyqpanda3.transpilation.generate_topology) generates the topology for quantum circuits based on specified parameters.

### 2.3 decompose

The [decompose](@ref pyqpanda3.transpilation.decompose) function can be used to break down quantum programs and circuits into their constituent parts.
If the `basic_gates` parameters are not passed, translation pass will be skipped.

Here is an example of how to use the `decompose` function:

@add_toggle_python
@snippet samples/python/transpiler.py Decompose sample
@end_toggle

**Run result:**

```python
decomposed prog: 
 
          ┌──┐ ┌──┐ ┌──┐                                                   ┌─┐                         >
q_0:  |0>─┤RY├ ┤RX├ ┤RX├ ──*─ ──── ──── ──── ──── ──── ──── ──── ──*─ ─────┤M├ ──── ──── ──── ──────── >
          ├──┤ ├──┤ ├──┤ ┌─┴┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌──┐ ┌─┴┐ ┌──┐ └╥┘ ┌──┐ ┌──┐           ┌─┐ >
q_1:  |0>─┤RZ├ ┤RX├ ┤RZ├ ┤CZ├ ┤RZ├ ┤RX├ ┤RZ├ ┤RZ├ ┤RZ├ ┤RX├ ┤RZ├ ┤CZ├ ┤RZ├──╫─ ┤RX├ ┤RZ├ ──*─ ─────┤M├ >
          ├──┤ ├──┤ ├──┤ └──┘ └──┘ └──┘ └──┘ └──┘ └──┘ └──┘ └──┘ └──┘ └──┘  ║  └──┘ └──┘ ┌─┴┐ ┌──┐ └╥┘ >
q_2:  |0>─┤RZ├ ┤RX├ ┤RZ├ ──── ──── ──── ──── ──── ──── ──── ──── ──── ──────╫─ ──── ──── ┤CZ├ ┤RZ├──╫─ >
          └──┘ └──┘ └──┘                                                    ║            └──┘ └──┘  ║  >
 c :   / ═══════════════════════════════════════════════════════════════════╩═══════════════════════╩═
                                                                             0                       1

                        
q_0:  |0>──── ──── ──── 
                        
q_1:  |0>──── ──── ──── 
         ┌──┐ ┌──┐  ┌─┐ 
q_2:  |0>┤RX├ ┤RZ├ ─┤M├ 
         └──┘ └──┘  └╥┘ 
 c :   / ════════════╩═
                      2

```
Starting from version V0.2.0, the decompose function can also be used to decompose matrices. An example of its usage is as follows:

```python
from pyqpanda3.core import *
from pyqpanda3.transpilation import *
import numpy as np

# Generate a random unitary matrix Q
A = np.random.rand(4, 4) + 1j * np.random.rand(4, 4)
Q, R = np.linalg.qr(A)

basic_gates = ['RX', 'RY', 'RZ', 'CP']
prog = decompose(Q, basic_gates=basic_gates)

print(prog)
```
