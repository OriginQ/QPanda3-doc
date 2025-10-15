v0.3.2 {#changelog_0_3_2}
===========================

## New features and important updates
1. Add the MS gate and provide the corresponding transpilation process in the transpile module. This logic gate will be used for future integration with ion trap-based transpilation.
```python
from pyqpanda3.core import MS,CPUQVM,QProg,QCircuit
from pyqpanda3.quantum_info import Unitary

cir = QCircuit()
cir << MS(0,1)
mat = Unitary(cir).ndarray()
print('mat of MS:',mat)
prog = QProg()
prog << cir
print('prog ir:',prog.originir())
qvm = CPUQVM()
qvm.run(prog,1)
stv = qvm.result().get_state_vector()
print('stv:',stv)
```
Output result of the example code
```bash
mat of MS: [[0.70710678+0.j         0.        +0.j         0.        +0.j
  0.        -0.70710678j]
 [0.        +0.j         0.70710678+0.j         0.        -0.70710678j
  0.        +0.j        ]
 [0.        +0.j         0.        -0.70710678j 0.70710678+0.j
  0.        +0.j        ]
 [0.        -0.70710678j 0.        +0.j         0.        +0.j
  0.70710678+0.j        ]]
prog ir: QINIT 2
CREG 1
MS q[0],q[1]

stv: [(0.7071067811865476+0j), 0j, 0j, -0.7071067811865476j]
```

2. Add support for dynamic quantum circuits of [branch](#branch_qprog_id) type and [loop](#loop_qprog_id) type.

3. Add new interfaces to allow users to [access the internal data within PauliOperator class objects](#pauli_operator_reveal_internal_data).

4. Add new interfaces to allow users to [obtain PauliOperator object for simulating Hamiltonian](#hamiltonian_pauli_operator).

5. Optimize the internal implementation of OriginBIS.

6. Add `QGATE` (custom gate) translation functionality in the OriginIR section. 

7. Add dynamic circuit (`QIF` and `QWHILE`) translation functionality in the OriginIR section.  

8. Add dynamic circuit (`if`) translation functionality in the OpenQASM section.

9. Add a new quantum bit remapping interface that accepts dictionary-style parameters, allowing you to directly specify the mapping between the original quantum bits and the mapped quantum registers. Refer to the following code:

```python
from pyqpanda3.core import *

prog = QProg()
prog << H(60)
prog << X(63)
prog << Y(61)
prog << CNOT(60, 61)
prog << CZ(60, 60)
prog << RXX(60, 63, 0.22)
prog << RYY(62, 64, 0.33)
prog << SWAP(60, 63)
prog << measure(60, 60)
prog << measure(63, 61)
prog << measure(61, 60)
prog << measure(63, 62)
prog << measure(64, 63)

print(prog.originir())

map_qubits = {}
map_qubits[60] = 5
map_qubits[61] = 3
map_qubits[62] = 1
map_qubits[63] = 2
map_qubits[64] = 0

map_cbits ={}
map_cbits[60] = 0
map_cbits[61] = 1
map_cbits[62] = 2
map_cbits[63] = 3
map_cbits[64] = 4

prog.remap(map_qubits, map_cbits);
print(prog.originir())
```

In the mapping dictionaries, the key represents the original circuit bit, and the value corresponds to the mapped quantum bit index. Note that values cannot be duplicated, otherwise undefined behavior will occur.

Output result:

```python
QINIT 65
CREG 64
H q[60]
X q[63]
Y q[61]
CNOT q[60],q[61]
CZ q[60],q[60]
RXX q[60],q[63],(0.22000000)
RYY q[62],q[64],(0.33000000)
SWAP q[60],q[63]
MEASURE q[60],c[60]
MEASURE q[63],c[61]
MEASURE q[61],c[60]
MEASURE q[63],c[62]
MEASURE q[64],c[63]

QINIT 6
CREG 4
H q[5]
X q[2]
Y q[3]
CNOT q[5],q[3]
CZ q[5],q[5]
RXX q[5],q[2],(0.22000000)
RYY q[1],q[0],(0.33000000)
SWAP q[5],q[2]
MEASURE q[5],c[0]
MEASURE q[2],c[1]
MEASURE q[3],c[0]
MEASURE q[2],c[2]
MEASURE q[0],c[3]
```

10. In the quantum cloud computing service, the chip measurement task can be configured to return sampled measurement results, as well as to specify the quantum bit block to map to. Refer to the following code:

```python
from pyqpanda3.core import *
from pyqpanda3.qcloud  import *

prog = QProg()
prog << H(0)
prog << X(1)
prog << Y(2)
prog << CNOT(0, 3)
prog.append(measure(0,0))

apikey = "d55f91309f0a4f2b1e7db872aaa05b3ca5edf79aa816b23135628dd765";

service = QCloudService(apikey)
backend = service.backend("WK_C102_400")

options = QCloudOptions()
options.set_is_prob_counts(True)
options.set_specified_block([32, 33, 34,35])
job = backend.run([prog, prog], 1000, options)

result = job.result()
print(result.get_counts_list())
```
In the above code, the member functions of [QCloudOptions](@ref pyqpanda3.qcloud.QCloudOptions) are used to set the specified bit block (set_specified_block) and to enable sampling measurement results (set_is_prob_counts).

```python
[{'0': 518, '1': 482}, {'0': 505, '1': 495}]
```

