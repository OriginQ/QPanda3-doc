v0.2.2 {#changelog_0_2_2}
===========================

## New features and important updates

1.Added remap functionality for quantum gates, circuits, and programs

The [remap](@ref pyqpanda3.core.QProg.remap) interface requires 1-2 parameters: the target qubit mapping result and the classical register mapping result. For example, when the original quantum circuit contains 6 qubits and 3 classical registers, invoking prog.remap([5,4,3,2,1,0], [3,2,1]) will produce the following mappings:

‌Qubits‌ are remapped from the default initial configuration [0,1,2,3,4,5] to [5,4,3,2,1,0].

‌Classical registers‌ are remapped from the initial configuration [0,1,2] to [3,2,1].

@note
The remap interface for quantum circuits and quantum logic gates ‌only accepts a single parameter‌, which specifies the target qubit mapping result.

Here’s a basic example of how to use the [remap](@ref pyqpanda3.core.QProg.remap) function:

```python
from pyqpanda3.core import H, CNOT, SWAP, measure, QProg
 
prog = QProg()
prog << H(0)
prog << H(2)
prog << H(4)
prog << CNOT(0, 1)
prog << CNOT(0, 2)
prog << CNOT(0, 3)
prog << CNOT(2, 4)
prog << SWAP(1, 3)
prog << measure(0, 0)
prog << measure(1, 1)

print(prog.originir())
prog.remap([5,4,3,2,1,0],[3,2,1])
print(prog.originir())
```

The result output of the program is as follows:

```python

# before remap

QINIT 5
CREG 2
H q[0]
H q[2]
H q[4]
CNOT q[0],q[1]
CNOT q[0],q[2]
CNOT q[0],q[3]
CNOT q[2],q[4]
SWAP q[1],q[3]
MEASURE q[0],c[0]
MEASURE q[1],c[1]

# after remap

QINIT 6
CREG 4
H q[5]
H q[3]
H q[1]
CNOT q[5],q[4]
CNOT q[5],q[3]
CNOT q[5],q[2]
CNOT q[3],q[1]
SWAP q[4],q[2]
MEASURE q[5],c[3]
MEASURE q[4],c[2]

```


2.Add version for pyqpanda3

```python
import pyqpanda3
print(pyqpanda3.__version__)
```

The output is as follows:

```python
0.2.2
```

3. Added interface for obtaining chip topology structure
[get_chip_topology](@ref pyqpanda3.qcloud.ChipInfo.get_chip_topology). This function return all edges in the specified chip topology.You can pass in the qubits to be used to obtain the topology of the specified qubits.
The obtained topo structure can be used in the transpile interface. If the topology is a non-connected graph, the compiler will default to using the largest connected subgraph for compilation.
For example, the Wukong chip has 72 qubits, but the largest connected subgraph consists of 23 qubits. Therefore, the maximum number of qubits supported for compilation is a 23-qubit circuit.

```python
from pyqpanda3.qcloud import *

apikey = ''
service = QCloudService(apikey)

backend_wukong = service.backend("72")

topo_use_qbits = list(range(30))
topo = backend_wukong.chip_info().get_chip_topology(topo_use_qbits)

circuit = random_qcircuit([0,1,2,3,4], 100, ["RX", "RY", "RZ", "H", "U1", "U2", "U3", "CP", "CNOT", "SWAP"])
prog_1 = QProg()
prog_2 = QProg()
prog_1 << circuit
transpiler = Transpiler()
basic_gates = ["U3", "CZ"]
prog_2 = transpiler.transpile(prog_1, topo, {}, 2, basic_gates)
```

## Fixed bug
1. Corrected the depolarizing noise operator error
