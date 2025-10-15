v0.1.0 {#changelog_0_1_0}
===========================

# QGate
## Prelude
In classical computing, the most fundamental unit is the bit, and the most basic control mode is the logic gate. We can achieve our circuit control objectives through the combination of logic gates. Similarly, the way to manipulate quantum bits is through quantum logic gates.
pyqpanda3 offers a more concise and lightweight approach to constructing and using quantum logic gates.

## Changes

- Optimize space occupancy

    In pyqpanda, quantum logic gates store their own matrix form, which consumes a large amount 
of memory space for generating ultra large quantum circuits. pyqpanda3, the matrix of quantum circuits is obtained at runtime, effectively reducing memory space occupation

```python
from pyqpanda3.core import H

h_gate= H(1)

# Retrieve matrix at runtime
matrix = h_gate.matrix()
```


## New Features
- Simplify the construction and initialization process.

 In most scenarios, we only need to know the logic gate type and operation bit index to construct quantum logic gates, while in pyqpanda, we need to apply for bits in advance.

```python
from pyqpanda import CPUQVM,H,CNOT

qvm = CPUQVM()
qvm.init_qvm()

qvm.set_configure(29, 29)
qubits = qvm.qAlloc_many(4)
cbits = qvm.cAlloc_many(4)

h_gate= H(qubits[0])
cnot_gate= CNOT(qubits[0], qubits[1])
```

In pyqpanda3, The process has been simplified.

```python

from pyqpanda3.core import QCircuit,H,CNOT,RX,CP
circuit = QCircuit(2)
circuit << H(0)
circuit << CNOT(0, 1)
circuit << RX(0, 3.9)
circuit << RX(0, 1.2)
circuit << RX(0, 5.6)
circuit << CP(0, 1, 4.9)

```

- Storage and space optimization

    Compared with pyqpanda, pyqpanda3 optimizes the 
information storage of quantum logic gates, reduces memory usage, and can effectively reduce more than 70% of space for the construction of large quantum circuits

- More diverse and concise development interfaces

 - Obtain quantum logic gate information and parameters

```python
from pyqpanda3.core import H

h_gate= H(1)

# get quantum gate gate_type
gate_type = h_gate.gate_type()

# get quantum gate qubits
qubits = h_gate.qubits()

# get quantum gate parameters
parameters = h_gate.parameters()

# get quantum gate control_qubits
control_qubits = h_gate.control_qubits()

# get quantum gate qubits num
qubits_num = h_gate.qubits_num()

# get quantum gate matrix
matrix = h_gate.qubits()

# dagger
h_dag_gate = h_gate.dagger()

```

- Simplified unnecessary quantum logic gate types.

pyqpanda has many quantum logic gates that are prone to misleading repetition. In the previous version, we supported the following types of logic gates

```python
class GateType:
    GATE_NOP
    GATE_UNDEFINED
    P0_GATE
    P1_GATE
    PAULI_X_GATE
    PAULI_Y_GATE
    PAULI_Z_GATE
    X_HALF_PI
    Y_HALF_PI
    Z_HALF_PI
    HADAMARD_GATE
    T_GATE
    S_GATE
    P_GATE
    CP_GATE
    RX_GATE
    RY_GATE
    RZ_GATE
    RXX_GATE
    RYY_GATE
    RZZ_GATE
    RZX_GATE
    U1_GATE
    U2_GATE
    U3_GATE
    U4_GATE
    CU_GATE
    CNOT_GATE
    CZ_GATE
    MS_GATE
    CPHASE_GATE
    ISWAP_THETA_GATE
    ISWAP_GATE
    SQISWAP_GATE
    SWAP_GATE
    TWO_QUBIT_GATE
    P00_GATE
    P11_GATE
    TOFFOLI_GATE
    ORACLE_GATE   
    I_GATE   
    BARRIER_GATE
    RPHI_GATE
```

pyqpanda3 has completed simplification and optimization

```python
class GateType:
    BARRIER
    CNOT
    CP
    CU
    CZ
    H
    I
    ISWAP
    ORACLE
    P
    RPHI
    RX
    RXX
    RY
    RYY
    RZ
    RZX
    RZZ
    S
    SQISWAP
    SWAP
    SX
    T
    TOFFOLI
    U1
    U2
    U3
    U4
    X
    X1
    Y
    Y1
    Z
    Z1
```

# Circuit & QProg

## Prelude

Quantum circuit and program is an important model in the field of quantum computing, used to describe and study the operation process of quantum algorithms and quantum computers. It is a graphical representation method used to demonstrate the operations and interactions between qubits, similar to circuit diagrams in classical computing.

## Changes

- The conversion between quantum programs and originir is more convenient and user-friendly`

  In pyqpanda, quantum programs and originir conversions are performed through independent 
external interfaces

```python
from pyqpanda import *

if __name__ == "__main__":

    machine = CPUQVM()
    machine.init_qvm()
    qlist = machine.qAlloc_many(4)
    clist = machine.cAlloc_many(4)
    prog = QProg()
    prog_cir = QCircuit()

    prog_cir << Y(qlist[2]) << H(qlist[2]) << CNOT(qlist[0],qlist[1])

    prog << H(qlist[2]) << Measure(qlist[1],clist[1])

    print(convert_qprog_to_originir(prog,machine))
```

 In pyqpanda3, it is directly implemented through the member functions of quantum programs

```python
from pyqpanda3.core import QProg

if __name__ == "__main__":
    originir_str = """
        QINIT 3
        CREG 2
        H q[2]
        H q[0]
        H q[1]
        CONTROL q[1]
        RX q[2],(-3.141593)
        ENDCONTROL
        CONTROL q[0]
        RX q[2],(-3.141593)
        RX q[2],(-3.141593)
        ENDCONTROL
        DAGGER
        H q[1]
        // CR q[0],q[1],(1.570796)
        H q[0]
        ENDDAGGER
        MEASURE q[0],c[0]
        MEASURE q[1],c[1]
    """

    prog = QProg(originir_str)
```

**Example Usage:**
```python

# Import the QCircuit class
from pyqpanda3.core import QCircuit,H,CNOT

# Create a quantum circuit with 2 qubits
circuit = QCircuit(2)

# Append gates to the circuit
circuit << H(0)  # Apply Hadamard gate on qubit 0
circuit << CNOT(0,1)  # Apply CNOT gate with control 0 and target 1
```

```python
from pyqpanda3.core import QProg,H,CNOT,measure,CPUQVM,draw_qprog

prog = QProg()
prog << H(0)
for i in range(1, 5):
    prog << CNOT(i - 1, i)
for i in range(5):
    prog << measure(i, i)
    

qvm = CPUQVM()
qvm.run(prog, 1024)
qvm.result().get_prob_list()
qvm.result().get_prob_dict()
```

## New Features

- Bottom level data structure optimization

In pyqpanda, we use linked lists to store internal quantum operation nodes. In PyQPanda3, we optimized this method by using sequential containers

| **Characteristics/Scenarios**        | **Quantum Program Sequential Container(std::vector)** | **Quantum program linked list(std::list, std::forward_list)** |
|--------------------------------------|-------------------------------------------------------|---------------------------------------------------------------|
| **Memory Layout**                    | Continuous memory allocation improves cache hit rate and access efficiency. | Distributed memory allocation leads to low cache hit rates and slow access speeds. |
| **Random access efficiency**         | Support random access of O (1), such as `QCircuit[i]`| Random access is not supported and needs to be traversed from scratch, with an access complexity of O (n). |
| **Traverse performance**             | Supports efficient traversal, with continuous memory allocation resulting in high traversal efficiency and a complexity of O (n). | The traversal efficiency is low, memory allocation is decentralized, and pointer operations increase additional overhead. |
| **Memory usage**                     | Additional space management capacity is required (such as during dynamic expansion), but the overall memory overhead is relatively small. | Each node requires additional storage of pointers, resulting in high memory overhead. |
| **Stability (pointer or reference)** | The insertion or deletion of elements may cause pointers or references to become invalid (such as ` std::) Vector will reallocate memory. | Element insertion/deletion does not invalidate pointers or references to other elements.|

Overall, quantum programs require traversal and space allocation, so using sequential containers can improve overall system efficiency.


- Enriched other member function interfaces

```python
from pyqpanda3.core import QProg, QCircuit, H, RY, RX, CNOT

circuit = QCircuit(3)
circuit << H(0)
circuit << RY(1, 0.3)
circuit << RY(2, 2.7)
circuit << RX(0, 1.5)
circuit << CNOT(0, 1)
circuit << CNOT(1, 2)

prog = QProg()

# append nodes
prog << circuit

# get depth
depth = prog.depth()
```

# Simulator
## Prelude
Simulators are used to simulate the evolution process of quantum computing circuits, including quantum systems in perfect states and simulations with noise. In the latest pyqpanda3 version, there are currently six types of virtual machines supported, including full amplitude simulators, single amplitude simulators, density matrix simulators, Clifford circuit simulators, and simulations with noise. Compared to pyqpanda, we have removed multiple redundant interfaces and used them in a more unified manner

## New Features
- Initialization process simplified.

 In the quantum simulator code of pyqpanda, the application and release of qubits, the construction of quantum circuits, and the binding of the virtual machine are integrated together. For example:

```python
from pyqpanda import *

qvm = CPUQVM()
qvm.init_qvm()

qvm.set_configure(29, 29)
qubits = qvm.qAlloc_many(4)
cbits = qvm.cAlloc_many(4)

prog = QProg()
prog << H(qubits[0]) << CNOT(qubits[0], qubits[1]) << Measure(qubits[0], cbits[0])

```

In pyqpanda3, complete decoupling has been achieved, and the explicit init configuration has been removed.

- Removal of Global Virtual Machine Functions and Usage:

```python
from pyqpanda import *
init(QMachineType.CPU)
machine = init_quantum_machine(QMachineType.CPU)
```

In the above pyqpanda code, the globally used virtual machine and its initialization method have been deprecated.

- Optimization of Noise Model Configuration and Usage.

The noise model is used to simulate the noise impact of real environments in quantum circuit computing tasks,In pyqpanda, setting noise parameters required a complex noise model, and the noise model and noise errors are not decoupled.

```python
from pyqpanda import *
noise = Noise()
noise.add_noise_model(NoiseModel.BITFLIP_KRAUS_OPERATOR, GateType.PAULI_X_GATE, 0.1)
qv0 = [q[0], q[1]]
noise.add_noise_model(NoiseModel.DEPHASING_KRAUS_OPERATOR, GateType.HADAMARD_GATE, 0.1, qv0)
qves = [[q[0], q[1]], [q[1], q[2]]]
noise.add_noise_model(NoiseModel.DAMPING_KRAUS_OPERATOR, GateType.CNOT_GATE, 0.1, qves)

f0 = 0.9
f1 = 0.85
noise.set_readout_error([[f0, 1 - f0], [1 - f1, f1]])
noise.set_rotation_error(0.05)
```

In pyqpanda3, noise errors and noise models are decoupled.

```python
from pyqpanda3.core import pauli_x_error, NoiseModel,GateType,depolarizing_error,phase_damping_error

pauli_error = pauli_x_error(0.6)

model = NoiseModel()
model.add_all_qubit_quantum_error(pauli_error,GateType.RX)
model.add_all_qubit_quantum_error(depolarizing_error(0.9),GateType.RX)
model.add_all_qubit_quantum_error(phase_damping_error(0.9),GateType.RX)
```

- The interface for running simulator and obtaining results has been simplified. Refer to the code below for details.

 - (1) Full Amplitue :The full amplitude and noise simulations in pyqpanda3 are merged into one simulator, which is run using a unified run function method, with the noise model as an optional parameter

```python
    from pyqpanda3.core import QCircuit,H,RY,RX,CNOT,QProg,measure,NoiseModel,CPUQVM
    from pyqpanda3.core import pauli_x_error,GateType

    circuit = QCircuit(3)
    circuit << H(0)
    circuit << RY(1, 0.3)
    circuit << RY(2, 2.7)
    circuit << RX(0, 1.5)
    circuit << CNOT(0, 1)
    circuit << CNOT(1, 2)

    prog = QProg()
    prog << circuit
    prog << measure(0, 0)
    prog << measure(1, 1)
    prog << measure(2, 2)
    
    model = NoiseModel()
    model.add_all_qubit_quantum_error(pauli_x_error(0.5),GateType.RX)

    machine = CPUQVM()
    machine.run(prog, 1000, model)
    measure_result = machine.result().get_counts()
```

 - Partial Amplitude

```python
    from pyqpanda3.core import QCircuit,H,RY,RX,CNOT,QProg
    from pyqpanda3.core import PartialAmplitudeQVM
    circuit = QCircuit(3)
    circuit << H(0)
    circuit << RY(1, 0.3)
    circuit << RY(2, 2.7)
    circuit << RX(0, 1.5)
    circuit << CNOT(0, 1)
    circuit << CNOT(1, 2)

    prog = QProg()
    prog << circuit
    
    machine = PartialAmplitudeQVM()
    
    machine.run(prog)
    measure_result = machine.get_state_vector(["0","1","2"])
```

 - DensityMatrixSimulator

```python
    from pyqpanda3.core import QCircuit,H,RX,CNOT,QProg,GateType
    from pyqpanda3.core import DensityMatrixSimulator,pauli_x_error,NoiseModel,depolarizing_error
    circuit = QCircuit(2)
    circuit << H(0)
    circuit << CNOT(0, 1)
    circuit << RX(0, 3.9)
    circuit << RX(0, 1.2)
    circuit << RX(0, 5.6)

    prog = QProg()
    prog << circuit
    
    machine = DensityMatrixSimulator()
    
    machine.run(prog)
    density_matrix = machine.density_matrix()
    
    pauli_error = pauli_x_error(0.6)
    
    model = NoiseModel()
    model.add_all_qubit_quantum_error(pauli_error,GateType.RX)
    model.add_all_qubit_quantum_error(depolarizing_error(0.9),GateType.RX)
    
    machine.run(prog, model)
    density_matrix = machine.density_matrix()
```

 - Stabilizer is used to simulate Clifford circuits, which only contain H, X, Y, Z, CNOT, S, CNOT, CZ quantum circuits. The maximum simulation can exceed 5000 quantum bits

```python
    from pyqpanda3.core import QProg,measure,random_qcircuit,Stabilizer

    prog = QProg()
    prog << random_qcircuit([0,1,2,3,4,5 ], 20, ["H","X","Y","Z","S","CZ","CNOT","SWAP"])
    prog << measure(0,0)
    prog << measure(1,1)
    prog << measure(2,2)
    
    machine = Stabilizer()
    
    machine.run(prog,1000)
    measure_result = machine.result().get_counts()
```

# Intermediate Compiler

## Prelude

pyqpanda3 has optimized the implementation of quantum program translation. It has replaced the cross-language syntax parser Antlr4, used in pyqpanda, with a line-by-line scanning method for quantum program translation, resulting in a fivefold improvement in overall execution efficiency.

Furthermore, pyqpanda3 supports the parsing of comments in quantum programs.


## New Features

The intermediate compiler module provides an interface that supports converting an instruction set string in the OriginIR format into a quantum program (QProg).
@code{.python}
from pyqpanda3.intermediate_compiler import convert_originir_string_to_qprog

prog = convert_originir_string_to_qprog(originir_str)
@endcode

The intermediate compiler module provides an interface that supports converting a text file containing OriginIR instruction set string into a quantum program (QProg).
@code{.python}
from pyqpanda3.intermediate_compiler import convert_originir_file_to_qprog

prog = convert_originir_file_to_qprog(originir_file_path)
@endcode

The intermediate compiler module provides an interface that supports converting an instruction set string in OpenQASM format into a quantum program (QProg).
@code{.python}
from pyqpanda3.intermediate_compiler import convert_qasm_string_to_qprog

prog = convert_qasm_string_to_qprog(qasm_str)
@endcode

The intermediate compiler module provides an interface that supports converting a text file containing OpenQASM instruction set string into a quantum program (QProg).
@code{.python}
from pyqpanda3.intermediate_compiler import convert_qasm_file_to_qprog

prog = convert_qasm_file_to_qprog(qasm_file_path)
@endcode

The intermediate compiler module provides an interface that supports converting a quantum program (QProg) into an instruction set string in the OriginIR format.
@code{.python}
from pyqpanda3.intermediate_compiler import convert_qprog_to_originir

convert_str = convert_qprog_to_originir(prog)
@endcode

As can be seen, in the pyqpanda3 translation QProg interface definition, the parameter list eliminates the QuantumMachine type parameter used for conversion in the old version, and instead only uses the instruction set string or quantum program file path string as parameters to be converted. Additionally, the return value is no longer a list containing the QProg object, quantum bits, and classical bits, but is now solely the QProg object.

# Hamiltonian
## Prelude
As of the latest version, pyqpanda3 supports the use of qpanda2-style and qiskit-style parameter forms or PauliOperator objects to directly construct objects of the Hamiltonian class. Objects of the Hamiltonian class support addition, subtraction, scalar multiplication, multiplication, and tensor products, as well as mixed operations. They also support displaying the main information of the object by omitting the "I" or omitting the string of qubit indexes.

## New Features

The Hamiltonian supports the direct construction of the Hamiltonian using the parameters of the constructed PauliOperator.
When constructing the Hamiltonian, it is no longer necessary to first construct the Pauli operator and then construct the Hamiltonian
```python
H2 = Hamiltonian({"Z0 I1": 1})
```
```python
Ham = Hamiltonian([("XXZ", [0, 1, 4], 1 + 2j), ("ZZ", [1, 2], -1 + 1j)] )
```
The Hamiltonian supports mixed operations of addition, subtraction, scalar multiplication, multiplication, and tensor product

```python
H1 = Hamiltonian({"X0 Y1":1})
H2 = Hamiltonian({"X0 Y1": 2})
H3 = H1+H2
```

```python
H1 = Hamiltonian({"X0 Y1":1})
H2 = Hamiltonian({"X0 Y1": 2})
H3 = H1-H2
```
```python
H1 = Hamiltonian({"X0 Y1":1})
H2 = Hamiltonian({"X0 Y1": 2})
H3 = H1*H2
```
```python
H1 = Hamiltonian({"X0 Y1":1})
H3 = (3.3+1.j) * H1
H4 = H1*(4.4+1.j)
```
```python
H1 = Hamiltonian({"X0 Y1":1})
H2 = Hamiltonian({"X0 Y1": 2})
H3 = H1.tensor(H2)
```

The Hamiltonian supports printing the corresponding string in the omitting "I" mode

```python
from pyqpanda3.hamiltonian import Hamiltonian
Ham = Hamiltonian({"X0 Z1":2,"X1 Y2":3})
print("without I:\n",Ham)
```

Output
```bash
without I:
 { qbit_total = 3, pauli_with_coef_s = { 'X0 Z1 ':2 + 0j, 'X1 Y2 ':3 + 0j, } }
```

The Hamiltonian supports printing the corresponding string by omitting qbit index

```python
from pyqpanda3.hamiltonian import Hamiltonian
Ha = Hamiltonian({"X0 Z1": 2, "X1 Y2": 3})
print("without I:\n", Ha)
print("with I:\n", Ha.str_with_I(True))
```
Output
```bash
without I:
 { qbit_total = 3, pauli_with_coef_s = { 'X0 Z1 ':2 + 0j, 'X1 Y2 ':3 + 0j, } }
with I:
 { qbit_totql = 3, paulis = [ 'IZX', 'YXI', ], coefs = [ 2 + 0j, 3 + 0j, ] }
```

# Operator
## Prelude
As of the latest version, pyqpanda3 supports the direct construction of PauliOperator objects using qpanda2-style and qiskit-style parameter forms. Objects of the PauliOperator class support addition, subtraction, scalar multiplication, multiplication, and tensor products, as well as mixed operations. They also support displaying the main information of the object by omitting the "I" or omitting the string of qubit indexes.

## Changes
Use the PauliOperator class to provide unified simulation of the first mock examination for basic Pauli operators and Pauli operator combinations

```python
op = PauliOperator("X")
op = PauliOperator("Y")
op = PauliOperator("Z")
op = PauliOperator("I")
H1 = PauliOperator({"X0 Y1":1})
```

## New Features
PauliOperator support qsikit-style construction methods
```python
# op means that "X0 X1 Z4" is applied to the quantum system with the coefficient 1+2.j, and "Z1 Z2" is applied to the quantum system with the coefficient -1+1.j
op = PauliOperator([("XXZ", [0, 1, 4], 1 + 2j), ("ZZ", [1, 2], -1 + 1j)] )
```
PauliOperator support tensor product operations

```python
H1 = PauliOperator({"X0 Y1":1})
H2 = PauliOperator({"X0 Y1":1})
H3 = H1.tensor(H2)
```

PauliOperator support scalar multiplication

```python
H1 = PauliOperator({"X0 Y1":1})
H3 = (3.3+1.j) * H1
```

PauliOperator supports mixed operations of addition, subtraction, scalar multiplication, multiplication, and tensor product

PauliOperator supports printing the corresponding string by omitting "I"

PauliOperator supports printing corresponding strings by omitting qbit indexes

```python
from pyqpanda3.hamiltonian import PauliOperator
Ha = PauliOperator({"X0 Z1": 2, "X1 Y2": 3})
print("without I:\n", Ha)
print("with I:\n", Ha.str_with_I(True))
```
Output
```bash
without I:
 { qbit_total = 3, pauli_with_coef_s = { 'X0 Z1 ':2 + 0j, 'X1 Y2 ':3 + 0j, } }
with I:
 { qbit_totql = 3, paulis = [ 'IZX', 'YXI', ], coefs = [ 2 + 0j, 3 + 0j, ] }
```

# Profiling Module

## Prelude
The profiling module is designed to intuitively present information about quantum programs. It analyzes quantum programs using two methods and displays the results through visualizations for users.

## New Features

- `draw_circuit_profile`: This function analyzes quantum programs, outputting the execution time of the quantum program and its subroutine, as well as the calling relationships between various quantum gates. Its inputs include the quantum circuit, names of quantum gates, and their execution times on hardware, with the output being a flowchart of the calling relationships between the quantum gates.
- `draw_circuit_features`: This function visualizes the features of a quantum circuit by generating a radar chart. It computes several metrics related to the circuit's structure and behavior, including connectivity, liveness, parallelism, entanglement, and critical depth, The meaning of specific features can be found in reference [1].

## references
1. Tomesh T, Gokhale P, Omole V, et al. Supermarq: A scalable quantum benchmark suite[C]//2022 IEEE International Symposium on High-Performance Computer Architecture (HPCA). IEEE, 2022: 587-603.

# QCloudService
## Prelude
In complex quantum circuit simulations, it is necessary to rely on high-performance computer clusters or real quantum computers to replace local computing with cloud computing, which to some extent reduces users' computing costs and provides a better computing experience.
The Origin Quantum Cloud Platform submits tasks to quantum computers or computing clusters deployed remotely through the Origin South Service, and queries the processing results,

## New Features

- Origin Quantum Cloud Service Initialization process simplified.
 
 In the quantum cloud service code of pyqpanda, the application and release of qubits, the construction of quantum circuits, and the binding of the virtual machine are integrated together. 

For example, This is the usage method of pyqpanda, and the process is quite complicated

```python
from pyqpanda import *
import numpy as np

qm = QCloud()
qm.set_configure(72,72);

qm.init_qvm("302e020100301006072a86006052b8104001c041730502010424100b6d33ad8772eb9708494453a3c8a/16327",True)

q = qm.qAlloc_many(6)
c = qm.cAlloc_many(6)

prog = QProg()
prog << hadamard_circuit(q)\
    << RX(q[1], np.pi / 4)\
    << RX(q[2], np.pi / 4)\
    << RX(q[1], np.pi / 4)\
    << CZ(q[0], q[1])\
    << CZ(q[1], q[2])\
    << Measure(q[0], c[0])\
    << Measure(q[1], c[1])
```

In pyqpanda3, complete decoupling has been achieved, and the explicit init configuration has been removed.

```python
#your real api token
api_key = "XXXXX"

from pyqpanda3.core import QProg,H,Y,X,measure
import pyqpanda3.qcloud as qcloud
prog = QProg()
prog << H(0)
prog << X(0)
prog << Y(0)
prog << measure(0,0)
    
service = qcloud.QCloudService(api_key)
```

- Unified Computing Interface

In pyqpanda, different computing interfaces are independent.

```python
result0 = QCM.full_amplitude_measure(measure_prog, 100)
result1 = QCM.full_amplitude_pmeasure(pmeasure_prog, [0, 1, 2])
result2 = QCM.partial_amplitude_pmeasure(pmeasure_prog, ["0", "1", "2"])
batch_prog = [prog for _ in range (6)]
real_chip_measure_batch_result = machine.batch_real_chip_measure(batch_prog, 1000, real_chip_type.origin_72)
```

In pyqpanda3, a unified approach is implemented using overloaded run functions.

At present, we support submission and query methods for cluster tasks such as full amplitude simulation, single amplitude simulation, partial amplitude simulation, and noisy simulation, as well as single and batch task submission and query for real quantum computing chip devices.

```python
from pyqpanda3.core import QProg,H,Y,X,measure
import pyqpanda3.qcloud as qcloud

#your real api token
api_key = "XXXXX"
prog = QProg()
prog << H(0)
prog << X(0)
prog << Y(0)
prog << measure(0,0)

service = qcloud.QCloudService(api_key)
backend = service.backend("origin_wukong")

options = qcloud.QCloudOptions()
job = backend.run(prog, 1000, options)
result = job.result()
```

- Optimized Synchronous and Asynchronous Processes

In pyqpanda, synchronous and asynchronous methods are provided separately.
pyqpanda3 simplifies this process.

Synchronous Process, **job.result()** will continuously loop through the task results until the task calculation is completed or there is an error

```python
    job = backend.run(prog, 1000)
    result= job.result()
    probs = result.get_probs()
```

Asynchronous method as above, Job.status() will query the current task status and return the query result

```python
    while True: 
      status = job.status() 
      if status == JobStatus.FINISHED: 
        break 
      sleep(5) 
```

- Added support for querying individual tasks, including the ability to query any historical task ID.

```python
    job = QCloudJob("A45DE13AA1920B115220C19BC0C0F4A5")
    probs = job.result().get_probs()
```

# QuantumInformation
## Prelude
As of the latest version, pyqpanda3 supports the use of StateVector and DensityMatrix to represent two basic quantum state representations, namely state vector and density matrix. It supports the use of five forms of describing quantum channels, including Kraus, Choi, Chi, SuperOp, and PTM, as well as the conversion between these five representations. In addition, it also provides information analysis tools such as Hellinger distance, Hellinger fidelity, and KL divergence, as well as a dedicated class for obtaining the unitary matrix of the quantum circuit QCircuit, and a Matrix class that can obtain the transpose matrix, adjoint matrix, and L2 norm.

## New Features

Abstract and simulate quantum states, including state vectors and density matrices, and support the application of quantum circuits for evolution operations on quantum systems represented by state vectors and density matrices

```python
# Construct a DensityMatrix object
dm = DensityMatrix(data)
```

```python
# Construct a StateVector object
stv = StateVector([1+1.j,2+2.j,3+3.j,4+4.j,5+5.j,6+6.j,7+7.j,8+8.j])
```

The abstraction and simulation of the representation methods Kraus, Choi, PTM, SuperOp, and Chi in quantum channel 5 support the pairwise conversion between these representation methods, and support the use of these quantum channels for evolving quantum states
```python
#Prepare a one-dimensional array consisting of a two-dimensional matrix
K0 = np.array([[0, 1], [1+0.j, 0]])
K1 = np.array([[1, 0], [0+0.j, -1]])
K2 = np.array([[0+0.j, -1.j], [1.j, 0]])
Ks1 = [K0, K1, K2]
# Constructing a Kraus object
Kra2 = Kraus(Ks1)
```
```python
# Constructing a Choi object from a Kraus object
choi = Choi(Kra3)
```
```python
# Constructing a Chi object from a Kraus object
chi = Chi(Kra3)
```
```python
# Constructing a SuperOp object from a Kraus object
sop = SuperOp(Kra3)
```
```python
# Constructing a PTM object from a Kraus object
ptm = PTM(Kra3)
```

Information analysis tools, including the calculation of Hellinger distance, Hellinger fidelity, and KL divergence between discrete probability distributions, including KL divergence between discrete probability distributions and KL divergence between continuous probability distributions

```python
# Calculate the Hellinger distance for p distribution and q distribution
hd = hellinger_distance(p_prob,q_prob)
```

```python
# Calculate the Hellinger fidelity for p distribution and q distribution
hd = hellinger_fidelity(p_prob,q_prob)
```

```python
# Calculate KL divergence for p distribution's probability values and q distribution's probability values
kld = KL_divergence(p_prob_val,q_prob_val)
```
Others, including a matrix class and a class for obtaining the unitary matrix of the quantum circuit QCircuit object

```python
data = np.array(data, dtype=complex)
# Constructing a Matrix object
m = Matrix(data)
```

```python
# Obtain the unitary matrix of QCircuit
matrix = Unitary(cir)
```

# Transpiler Module

## Prelude
This module provides functionalities for compiling quantum circuits, enhancing optimization and mapping to specific hardware topologies.

## New Features

- A new `Transpiler` class has been added, which compiles circuits using the `transpile()` interface. This interface has four parameters: `QProg`, `chip_topology_edges`, `init_mapping`, and `optimization_level`. The `QProg` parameter is the quantum circuit to be compiled, `chip_topology_edges` is the target topology for compilation, `init_mapping` is the mapping of virtual qubits to physical qubits, and `optimization_level` can be set to 0, 1 or 2:
  - `0`: No optimization is performed.
  - `1`: Basic optimizations are applied, including simple two-qubit gate cancellation and single-qubit gate merging.
  - `2`: Advanced optimizations are performed, including matrix optimization, two-qubit gate cancellation, and single-qubit gate merging.

- The current compiled basic gate set is [`X1`, `RZ`, `CZ`]

- In the future, InitPass, LayoutPass, RoutingPass, OptimizationPass, and TranslationPass will be gradually opened up, allowing users to freely combine these passes to customize the transpiler process.

**Example Usage:**

```python
from pyqpanda3.core import QProg,H,RX,CNOT,U1,CNOT,measure,draw_qprog
from pyqpanda3.transpilation import Transpiler,generate_topology

prog_1 = QProg()
prog_1 << H(0) << RX(0, 1.25) << CNOT(0, 1) << U1(1, 0.0001) << CNOT(0, 1) << CNOT(1, 2)
for i in range(3):
    prog_1 << measure(i, i)
# Create topology structure
topo = generate_topology(20, "square")
transpiler = Transpiler()
prog_level_0 = transpiler.transpile(prog_1, topo, {}, 0)
prog_level_1 = transpiler.transpile(prog_1, topo, {}, 1)
prog_level_2 = transpiler.transpile(prog_1, topo, {}, 2)
```

# Variational Quantum Circuit



## Prelude

As of the latest version, pyqpanda3 uses VQCircuit to support variational quantum circuits. Provide placeholder support for setting variable parameters for logic gates in Ansatz using multidimensional arrays, support for generating QCircuit in batches based on multidimensional arrays, and support for calculating the expected values of Hamiltonians/Pauli operators for the generated QCircuit. In addition, VQCircuit also supports enabling the layer mechanism to better generate QCircuits composed of multiple quantum circuits with the same structure connected in series.

## Changes
The name of Class was changed from VQC to VQCircuit to avoid conflicts with the abbreviation of the variational quantum circuit classifier.
<a href="#VQCircuit()">Code example</a>

When constructing Ansatz, the method of adding non-parametric gates and parameter-fixed gates has changed. pyqpanda3 uses the same functions as QCircuit and QProg to add corresponding quantum logic gates.
<a href="#add_qgate_qcircuit">Code example</a>

When constructing Ansatz, the method of adding parameters that require updating quantum logic gates has been changed. pyqpanda3 uses the  functions with same names as those which are for generate QCircuit or QProg.  These functions can specify which gate parameters of a certain type with parameters need to be updated.
<a href="#add_vqgate">Code example</a>

The methods for generating specific QCircuit objects vary. The VQCircuit object generates corresponding QCircuit objects in batches by passing in a multidimensional array.
<a href="#res.at()">Code example</a>

After generating a QCircuit object, the return form of the result is different. pyqpanda3 uses the VQCResult object to manage the generated QCircuit
<a href="#VQResult">Code example</a>

## New Features

<a id="VQCircuit()"></a>

Using VQCircuit as the class name can avoid ambiguity between the abbreviations for "Variational Quantum Circuit" and "Variational Quantum Classifier" when referring to VQC.
<a id="VQCircuit()code"></a>

```python
# Construct a VQCircuit object
from pyqpanda3.vqcircuit import VQCircuit
vqc = VQCircuit()
```

<a id="add_qgate_qcircuit"></a>

Using unified functions' names to add quantum logic gates.
When constructing Ansatz, the method of adding non-parametric gates and parameter-fixed gates has changed. pyqpanda3 uses the same functions as QCircuit and QProg to add corresponding quantum logic gates

If you want to direct run this example code to try this function. The easiest way is to copy all the code blocks from <a href="#VQCircuit()code">The first code example</a> to this code into a python script file.

```python
# pyqpanda3 uses the same functions as QCircuit and QProg to add corresponding quantum logic gates
from pyqpanda3.core import QCircuit,RX,RY,U2,Y,H,X,CP
# Add the parameters of variable quantum logic gates with parameters one by one and fix them.
vqc << H(0)<<X(0)<<CP(1,2,3.14)

# Add variable sub-circuit logic gates in batches, with parameters fixed for gates with parameters.
cir = QCircuit()
cir << Y(0)<<U2(1,4.14,5.14)
vqc << cir
```

<a id="add_vqgate"></a>

It provides a placeholder mechanism that can be used to add quantum logic gates with variable parameters when constructing the Ansatz of a variational quantum circuit.
When constructing Ansatz, the method of adding parameters that require updating quantum logic gates has been changed. pyqpanda3 uses the  functions with same names as those which are for generate QCircuit or QProg.  These functions can specify which gate parameters of a certain type with parameters need to be updated.

If you want to direct run this example code to try this function. The easiest way is to copy all the code blocks from <a href="#VQCircuit()code">The first code example</a> to this code into a python script file.

```python
# 1>pyqpanda3 uses the functions with same names as those which are for generate QCircuit or QProg.
# These functions can specify which gate parameters of a certain type with parameters need to be updated.
# 2>Before use these functions.The dimension information of the multidimensional array required to construct
# a QCircuit object needs to be agreed upon in advance (with the layer mechanism disabled by default), or
# the dimension information of the multidimensional array required to construct a sub-circuit for one layer
# needs to be agreed upon (when the hierarchical mechanism is enabled).
# 3>Please use member function set_Param to set the dimension information.
vqc.set_Param([3,2])
# member function get_Param_dims is designed to get the dimension information.
dims = vqc.get_Param_dims()
print("dims:",dims)
# Add variable quantum logic gates one by one, with parameters of the parameterized gates being variable.
vqc << RX(0,vqc.Param([0,0],"theta"))<< RX(2,vqc.Param("theta"))<< RX(1,vqc.Param([1,0]))<< RY(0,vqc.Param([0,1]))<< U2(1,vqc.Param([1,1]),5.55)<< RY(2,vqc.Param([2,1]))
```

Run the program composed of the sequence introduced in the documentation, from code block <a href="#VQCircuit()code">The first code example</a> to this code block. The output corresponding to this code block is as follows:

```bash
dims: [3, 2]
```

It provides a practical interface that can be used to print information related to the Ansatz.

If you want to direct run this example code to try this function. The easiest way is to copy all the code blocks from <a href="#VQCircuit()code">The first code example</a> to this code into a python script file.


```python
# Display Ansatz related information.
print("display_ansatz:")
vqc.display_ansatz()
```

Run the program composed of the sequence introduced in the documentation, from code block <a href="#VQCircuit()code">The first code example</a> to this code block. The output corresponding to this code block is as follows:

```bash
display_ansatz:
---gates of a layer circuit:
___
gate id:0
gate type:H
qbits:0,
gate's params' total:0
gate's params:
___
gate id:1
gate type:X
qbits:0,
gate's params' total:0
gate's params:
___
gate id:2
gate type:CP
qbits:1,2,
gate's params' total:1
gate's params:
 1th param is fixed. It's val is 3.14
___
gate id:3
gate type:Y
qbits:0,
gate's params' total:0
gate's params:
___
gate id:4
gate type:U2
qbits:1,
gate's params' total:2
gate's params:
 1th param is fixed. It's val is 4.14
 2th param is fixed. It's val is 5.14
___
gate id:5
gate type:RX
qbits:0,
gate's params' total:1
gate's params:
 1th param is mutable. 
   It's pos in parameters is:0,0
   It's label is:theta
___
gate id:6
gate type:RX
qbits:2,
gate's params' total:1
gate's params:
 1th param is mutable. 
   It's pos in parameters is:0,0
   It's label is:theta
___
gate id:7
gate type:RX
qbits:1,
gate's params' total:1
gate's params:
 1th param is mutable. 
   It's pos in parameters is:1,0
   It's label is:
___
gate id:8
gate type:RY
qbits:0,
gate's params' total:1
gate's params:
 1th param is mutable. 
   It's pos in parameters is:0,1
   It's label is:
___
gate id:9
gate type:U2
qbits:1,
gate's params' total:2
gate's params:
 1th param is mutable. 
   It's pos in parameters is:1,1
   It's label is:
 2th param is fixed. It's val is 5.55
___
gate id:10
gate type:RY
qbits:2,
gate's params' total:1
gate's params:
 1th param is mutable. 
   It's pos in parameters is:2,1
   It's label is:
---
```
<a id="res.at()"></a>

VQCircuit supports using multi-dimensional arrays to store parameters for batch generating QCircuit objects. The following code is used to prepare a multi-dimensional array data, which will be used in the subsequent example code for batch generating QCircuit objects.

If you want to direct run this example code to try this function. The easiest way is to copy all the code blocks from <a href="#VQCircuit()code">The first code example</a> to this code into a python script file.

```python
# 1>Prepare a multidimensional array for batch construction of QCircuit objects. This multidimensional array
# contains multiple multidimensional arrays used to construct individual QCircuit objects or individual layered sub-circuits.
# 2>For example, set_Param([3,2]) specifies that constructing a QCircuit object (with the hierarchical mechanism disabled) or
# constructing a layered sub-circuit (with the hierarchical mechanism enabled) requires a multidimensional array of shape [3,2].
# Here, data is a multidimensional array of shape (2,3,2,3,2), which contains 12 multidimensional arrays of shape [3,2] (calculated as 2*3*2).
# These multidimensional arrays can be accessed using the indexing format data[i][j][k].
import numpy as np
data = np.zeros((3,2,3,2))

for i1 in range(3):
    for i2 in range(2):
        for i3 in range(3):
            for i4 in range(2):
                data[i1][i2][i3][i4] = (i1+1)*1000+(i2+1)*100+(i3+1)*10+(i4+1)
```

VQCircuit object support disable hierarchical mechanism.

If you want to direct run this example code to try this function. The easiest way is to copy all the code blocks from <a href="#VQCircuit()code">The first code example</a> to this code into a python script file.

@note 
The VQCircuit default constructor disable hierarchical mechanism by default.

```python
# Disable hierarchical mechanism
vqc.disable_layer()
```

In pyqpanda3, it is so easy to use multi-dimensional arrays to store parameters for batch generating QCircuit objects.

If you want to direct run this example code to try this function. The easiest way is to copy all the code blocks from <a href="#VQCircuit()code">The first code example</a> to this code into a python script file.

```python
# 1> Apply the parameter data to generate multiple QCircuits in batch and store them in cirs
# 2> Here, 3*2 QCircuits are generated, totaling 12 QCircuits. Each QCircuit is updated by VQC
# based on the last two dimensions of data, which is an array with a shape of (3,2)
res = vqc(data)
```

<a id="VQResult"></a>

Using VQResult to manage the result of generating QCircuit object by VQCircuit object.
This allows users to access each QCircuit object using multi-dimensional array indexing.

If you want to direct run this example code to try this function. The easiest way is to copy all the code blocks from <a href="#VQCircuit()code">The first code example</a> to this code into a python script file.

```python
# Obtaining QCircuits form result using multidimensional indexing.
from pyqpanda3.core import draw_qprog

for i1 in range(3):
    for i2 in range(2):
        cir = res.at([i1,i2])
        # 1> Here, you can use each QCircuit object directly or apply custom processing to each QCircuit.
        # 2> If you wish to use a visual approach to understand the structure of the QCircuit object,
        # please uncomment the two adjacent lines of code below. These lines of code will use the draw_qprog interface to print the structure of the QCircuit object.
        # if len(cir.count_ops())>0:
        #     print(draw_qprog(circuit=cir,param_show=True,expend_map={'all':2}))
```

If you wish to construct a QCircuit that is composed of substructures (subcircuits) concatenated together, enabling the layer mechanism within VQCircuit will make it more convenient for users to batch construct QCircuit objects using parameters stored in a multi-dimensional array.

If you want to direct run this example code to try this function. The easiest way is to copy all the code blocks from <a href="#VQCircuit()code">The first code example</a> to this code into a python script file.

```python
# Enable hierarchical mechanism
vqc.enable_layer()

# 1> Apply the parameter data to generate multiple QCircuits in batches and store them in cirs
# 2> Here, 3 QCircuits consisting of two layers of sub-circuits are generated, for a total of 6 QCircuits,
# 3> Each QCircuit is generated by VQC by updating parameters based on the last 3-dimensional data, specifically an
# array with a shape of (2, 3, 2).
# 4> Each QCircuit is composed of two layers of sub-circuits connected in series, and each sub-circuit layer is obtained
# by setting specific parameters for the Ansatz based on the last two-dimensional data, which is an array with a shape of (3, 2).
res = vqc(data)

# Obtaining QCircuits form result using multidimensional indexing.
from pyqpanda3.core import draw_qprog

for i1 in range(3):
    cir = res.at([i1])
    # 1> Here, you can use each QCircuit object directly or apply custom processing to each QCircuit.
    # 2> If you wish to use a visual approach to understand the structure of the QCircuit object,
    # please uncomment the two adjacent lines of code below. These lines of code will use the draw_qprog interface to print the structure of the QCircuit object.
    # if len(cir.count_ops())>0:
    #     print(draw_qprog(circuit=cir,param_show=True,expend_map={'all':2}))
    
```

The VQResult object provides useful post-processing functionalities for the VQCircuit object.
If you only want to obtain the expectation value of the Hamiltonian corresponding to a specific QCircuit in res, the following code is appropriate.

Please to watch [Hamiltonian](@ref tutorial_hamiltonian) to learn more information about pyqpanda3.hamiltonian.Hamiltonian

If you want to direct run this example code to try this function. The easiest way is to copy all the code blocks from <a href="#VQCircuit()code">The first code example</a> to this code into a python script file.

```python
# of the results of quantum program execution in quantum systems with  Hamiltonian actions

from pyqpanda3.hamiltonian import Hamiltonian
from pyqpanda3.vqcircuit import ExpectationType
Ha = Hamiltonian(pauli_with_coef_s = { 'X0 Z1 ':2 + 0j, 'X1 Y2 ':3 + 0j, })

# calculate the theoretical hamiltonian expected val of res.at([2]) which is a QCircuit object
expectation = res.expval_hamiltonian(hamiltonian=Ha,idx_s=[2],exp_type=ExpectationType.THEORETICAL)
print("expectation1:",expectation)
```

Run the program composed of the sequence introduced in the documentation, from code block <a href="#VQCircuit()code">The first code example</a> to this code block. The output corresponding to this code block is as follows:

```bash
expectation1: -1.9289474504185782
```

If you want to obtain all the expectation values of the Hamiltonian for all QCircuit objects in res in batch, the following code can effectively achieve this goal.

Please to watch [Hamiltonian](@ref tutorial_hamiltonian) to learn more information about pyqpanda3.hamiltonian.Hamiltonian

If you want to direct run this example code to try this function. The easiest way is to copy all the code blocks from <a href="#VQCircuit()code">The first code example</a> to this code into a python script file.

```python
# calculate the theoretical hamiltonian expected vals of QCircuit objects in res
expectations = res.expval_hamiltonian(hamiltonian=Ha,exp_type=ExpectationType.THEORETICAL)
# 1> Using multi-dimensional array indexing to obtain computation results.
for i1 in range(3):
    # 2> Use expal_at([i1]) to get the he theoretical hamiltonian expected val of res.at([i1]) which is a QCircuit object
    expectation = res.expval_at([i1])
    print(f"expectation1 [{i1}]:", expectation)
```

Run the program composed of the sequence introduced in the documentation, from code block <a href="#VQCircuit()code">The first code example</a> to this code block. The output corresponding to this code block is as follows:

```bash
expectations1 [0]: -1.214227039562041
expectations1 [1]: 0.12937804539398823
expectations1 [2]: -1.9289474504185782
```

If you only want to obtain the expectation value of the PauliOperator corresponding to a specific QCircuit in res, the following code is appropriate.

Please to watch [PauliOperator](@ref tutorial_table_of_content_PauliOperator) to learn more information about pyqpanda3.hamiltonian.PauliOperator

If you want to direct run this example code to try this function. The easiest way is to copy all the code blocks from <a href="#VQCircuit()code">The first code example</a> to this code into a python script file.

```python
# of the results of quantum program execution in quantum systems with Pauli operators

from pyqpanda3.hamiltonian import PauliOperator
pauli = PauliOperator(pauli_with_coef_s = { 'X0 Z1 ':2 + 0j, 'X1 Y2 ':3 + 0j, })

# calculate the theoretical PauliOperator expected val of res.at([2]) which is a QCircuit object
expectation = res.expval_pauli_operator(pauli_operator=pauli,idx_s=[2],exp_type=ExpectationType.THEORETICAL)
print("expectation2:",expectation)
```

Run the program composed of the sequence introduced in the documentation, from code block <a href="#VQCircuit()code">The first code example</a> to this code block. The output corresponding to this code block is as follows:

```bash
expectation2: -1.9289474504185782
```

If you want to obtain all the expectation values of the PauliOperator for all QCircuit objects in res in batch, the following code can effectively achieve this goal.

Please to watch [PauliOperator](@ref tutorial_table_of_content_PauliOperator) to learn more information about pyqpanda3.hamiltonian.PauliOperator

If you want to direct run this example code to try this function. The easiest way is to copy all the code blocks from <a href="#VQCircuit()code">The first code example</a> to this code into a python script file.

```python
# calculate the theoretical PauliOperator expected vals of QCircuit objects in res
pauli = PauliOperator(pauli_with_coef_s = { 'X0 Z1 ':2 + 0j, 'X1 Y2 ':3 + 0j, })
expectations = res.expval_pauli_operator(pauli_operator=pauli,exp_type=ExpectationType.THEORETICAL)

# 1> Using multi-dimensional array indexing to obtain computation results.
for i1 in range(3):
    # 2> Use expal_at([i1]) to get the he theoretical PauliOperator expected val of res.at([i1]) which is a QCircuit object
    expectation = res.expval_at([i1])
    print(f"expectation2 [{i1}]:", expectation)
```

Run the program composed of the sequence introduced in the documentation, from code block <a href="#VQCircuit()code">The first code example</a> to this code block. The output corresponding to this code block is as follows:

```bash
expectations2 [0]: -1.214227039562041
expectations2 [1]: 0.12937804539398823
expectations2 [2]: -1.9289474504185782
```
