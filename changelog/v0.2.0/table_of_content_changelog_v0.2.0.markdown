v0.2.0 {#changelog_0_2_0}
===========================

## New features and important updates
- **Optimized the CPU measurement algorithm, involving CPUQVM and GPUQVM.**

Performance improvements were made for large-qubit measurement processes (when the measurement node is at the end), resulting in a significant reduction in execution time.

A very obvious example,It can be concluded that there has been a significant improvement in operating speed

```python
from pyqpanda3.core import QProg,H,CNOT,measure,CPUQVM
prog = QProg()
prog << H(0)

for i in range(21):   
    prog << CNOT(i, i + 1)
    
for i in range(22):   
    prog << measure(i, i)

machine = CPUQVM()
machine.run(prog, 10000)
measure_result = machine.result().get_counts()
print(measure_result)
```

Output is as follows.

```python
{'0000000000000000000000': 4957, '1111111111111111111111': 5043}
```

- **Quantum cloud computing service.** 

For tasks targeting real quantum chips, users can choose to transmit quantum programs in binary stream format when submitting tasks, offering faster communication efficiency compared to the original approach. The existing run interface has been updated with additional parameters, while all other usage remains unchanged from the previous version.

```python
import time
from pyqpanda3.core import QProg,H,CNOT,measure
from pyqpanda3.qcloud import QCloudService,QCloudOptions,JobStatus
prog = QProg()
prog << H(0)
prog << CNOT(0, 1)
prog.append(measure(0,0))

api_key = "302e020030100602a8648ce3020160528100050204615ad5e";
service = QCloudService(api_key)
backend = service.backend("72")

options = QCloudOptions()
job = backend.run(prog, 1000, options, True)

while True: 
    status = job.status()
    if status == JobStatus.FINISHED: 
        break 
    print(job.job_id()," wait for finished.")
    time.sleep(2)

result = job.result()
print(result.get_probs())
```

Output is as follows.

```python
{'0': 0.55, '1': 0.45}
```

- **The quantum cloud cluster tasks have been standardized and adjusted for both partial amplitude and single amplitude interfaces.**

The task submission interfaces for cluster partial amplitude and single amplitude have slight differences in parameters: besides QProg, they correspond to an amplitude array and a single amplitude value, respectively. The query interface is completely identical, both using [QCloudResult.get_amplitudes](@ref pyqpanda3.qcloud.QCloudResult.get_amplitudes), which returns an array.

```python
from pyqpanda3.core import QProg,H,CNOT,RX,RY
from pyqpanda3.qcloud import QCloudService

prog = QProg(6)
prog << H(0)
prog << CNOT(1, 2)
prog << RY(0, 0.34)
prog << RX(1, 0.98)

api_key = "302e020100301006094453a3c8a/6327";

service = QCloudService(api_key)
service.setup_logging()
single_amplitude_backend = service.backend("single_amplitude")

single_amplitude_job = single_amplitude_backend.run(prog, "0")
print(single_amplitude_job.result().get_amplitudes())

partial_amplitude_backend = service.backend("partial_amplitude")
partial_amplitude_job = partial_amplitude_backend.run(prog, ["0","1","2"])

print(partial_amplitude_job.result().get_amplitudes())
```

The output is as follows.

```python
{'0': (0.5093563616148238+0j)}
{'0': (0.5093563616148238+0j), '1': (0.7204633002944995+0j), '2': (0.5093563616148238+0j)}
```

Note that cluster tasks support asynchronous operations but do not support submitting multiple circuits in batch mode.

- **Noise simulation has been added to the GPU virtual machine.** 

Like the CPUQVM virtual machine, it supports the following noise models, and the interface usage remains completely identical.The supported noise errors are as follows.

```python
def pauli_x_error(prob: float) -> QuantumError

def pauli_y_error(prob: float) -> QuantumError

def pauli_z_error(prob: float) -> QuantumError

def phase_damping_error(prob: float) -> QuantumError

def amplitude_damping_error(prob: float) -> QuantumError

def decoherence_error(arg0: float, arg1: float, arg2: float) -> QuantumError

def depolarizing_error(prob: float) -> QuantumError
```

The example program is as follows.

```python
from pyqpanda3.core import QCircuit,QProg,H,RX,RY,CNOT,measure,GateType
from pyqpanda3.core import GPUQVM,NoiseModel,pauli_x_error

circuit = QCircuit(3)
circuit << H(0)
circuit << RY(1, 0.3)
circuit << RY(2, 2.7)
circuit << RX(0, 1.5)
circuit << CNOT(0, 1)
circuit << CNOT(1, 2)

prog = QProg()
prog.append(circuit)

prog.append(measure(0, 0))
prog.append(measure(1, 1))
prog.append(measure(2, 2))

model = NoiseModel()
model.add_all_qubit_quantum_error(pauli_x_error(0.5),GateType.RX)
model.add_all_qubit_quantum_error(pauli_x_error(0.9),GateType.H)

machine = GPUQVM()
machine.run(prog, 1000, model)
measure_result = machine.result().get_counts()
print(measure_result)
```

Output is :

```python
{'011': 466, '001': 3, '100': 468, '111': 22, '000': 17, '101': 13, '010': 11}
```

- **The related interface for expectation value calculation has been changed.**

(1) Use CPUQVM to calculate the expectation value of a quantum circuit on a quantum system with a specified Hamiltonian. (pyqpanda3.core)

old interface

[API doc of pyqpanda3.core.CPUQVM.expval_hamiltonian](@ref pyqpanda3.core.core.CPUQVM.expval_hamiltonian)

```python
# old interface
# note: This old interface is still retained in this version but will be deprecated in the future.
expval_hamiltonian(self: core.CPUQVM, prog: core.QProg, hamiltonian: QPanda3::Hamiltonian, shots: int = 1000, noise_model: core.NoiseModel = <core.NoiseModel object at 0x000002548518BB30>)
```

new interface 

[API doc of pyqpanda3.core.expval_hamiltonian](@ref pyqpanda3.core.core.expval_hamiltonian)

```python
# new interface
# If you want to use CPUQVM to calculate the expectation value, the parameter backend needs to be passed with the argument 'CPU'.
expval_hamiltonian(node: core.QProg, hamiltonian: QPanda3::Hamiltonian, shots: int = 1, model: core.NoiseModel = <core.NoiseModel object at 0x0000026A734CB370>, used_threads: int = 4, backend: str = \'CPU\') 
```
The example program is as follows.
```python
from pyqpanda3.core import CPUQVM, QProg, S, BARRIER,expval_hamiltonian
from pyqpanda3.hamiltonian import Hamiltonian

def test_expectation2():
    # Prepare the quantum simulator
    qvm = CPUQVM()
    # Prepare quantum program
    prog = QProg()
    prog << S(0)
    prog << BARRIER([0, 1, 2])
    """The qubits used in qprog should be consistent with the qubits in the Pauli operator. The simulator determines the qubits that should be included in the quantum system based on the qbits in QProg, which are three quantum #bits in this case"""
    # Prepare the Pauli operator
    Ham = Hamiltonian({"X0 Z1": 2, "X1 Y2": 3})
    print("without I:\n", Ham)
    # Calculate Expectation
    try:
        # Calculate Expectation
        res = qvm.expval_hamiltonian(prog, Ham) #old interface,will be deprecated in the future.
        print("res:", res)
        res2 = expval_hamiltonian(prog,Ham,backend='CPU') # new interface
        print('res2:',res2)
    except Exception as e:
        print(e)

if __name__ == "__main__":
    test_expectation2()
```

Output is :
```bash
without I:
 { qbit_total = 3, pauli_with_coef_s = { 'X0 Z1 ':2 + 0j, 'X1 Y2 ':3 + 0j, } }
res: 0.0
res2: 0.0
Warrning! This interface will be deprecated in the future. Please use the new interface expval_hamiltonian (a global interface exported by pyqpanda3.core).
```

(2) Use GPUQVM to calculate the expectation value of a quantum circuit on a quantum system with a specified Hamiltonian. (pyqpanda3.core)

new interface 

[API doc of pyqpanda3.core.expval_hamiltonian](@ref pyqpanda3.core.core.expval_hamiltonian)

```python
# new interface
# If you want to use GPUQVM to calculate the expectation value, the parameter backend needs to be passed with the argument 'GPU'.
expval_hamiltonian(node: core.QProg, hamiltonian: QPanda3::Hamiltonian, shots: int = 1, model: core.NoiseModel = <core.NoiseModel object at 0x0000026A734CB370>, used_threads: int = 4, backend: str = \'CPU\') 
```

The example program is as follows.
```python
from pyqpanda3.core import QProg, S, BARRIER,expval_hamiltonian
from pyqpanda3.hamiltonian import Hamiltonian
def test_expectation2():
    # Prepare quantum program
    prog = QProg()
    prog << S(0)
    prog << BARRIER([0, 1, 2])
    """The qubits used in qprog should be consistent with the qubits in the Pauli operator. The simulator determines the qubits that should be included in the quantum system based on the qbits in QProg, which are three quantum #bits in this case"""
    # Prepare the Pauli operator
    Ham = Hamiltonian({"X0 Z1": 2, "X1 Y2": 3})
    print("without I:\n", Ham)
    # Calculate Expectation
    try:
        # Calculate Expectation
        res2 = expval_hamiltonian(prog,Ham,backend='GPU') # new interface
        print('res2:',res2)
    except Exception as e:
        print(e)

if __name__ == "__main__":
    test_expectation2()
```

Output is :
```bash
without I:
 { qbit_total = 3, pauli_with_coef_s = { 'X0 Z1 ':2 + 0j, 'X1 Y2 ':3 + 0j, } }
res2: 0.0
```

(3) Use CPUQVM to calculate the expectation value of a quantum circuit on a quantum system with a specified Hamiltonian. (pyqpanda3.vqcircuit)

old interface
```python
# old interface (CPUQVM, batch processing mode, deprecated)
expval_hamiltonian(self: pyqpanda3.vqcircuit.VQCResult, hamiltonian: QPanda3::Hamiltonian, exp_type: pyqpanda3.vqcircuit.ExpectationType = <ExpectationType.THEORETICAL: 0>)

# old interface(CPUQVM, non-batch processing mode, deprecated)
expval_hamiltonian(self: pyqpanda3.vqcircuit.VQCResult, hamiltonian: QPanda3::Hamiltonian, idx_s: list[int], exp_type: pyqpanda3.vqcircuit.ExpectationType = <ExpectationType.THEORETICAL: 0>)
```

new interface

[API doc of pyqpanda3.vqcircuit.VQCResult.expval_hamiltonian](@ref pyqpanda3.vqcircuit.VQCResult.expval_hamiltonian)

```python
# new interface (batch processing mode, with NoiseModel)
# If you want to use GPUQVM to calculate the expectation value, the parameter backend needs to be passed with the argument 'CPU'.
expval_hamiltonian(self: vqcircuit.VQCResult, hamiltonian: QPanda3::Hamiltonian, shots: int = 1000, model: QPanda3::NoiseModel, used_threads: int = 4, backend: str = 'CPU')

# new interface (batch processing mode, without NoiseModel)
# If you want to use GPUQVM to calculate the expectation value, the parameter backend needs to be passed with the argument 'CPU'.
expval_hamiltonian(self: vqcircuit.VQCResult, hamiltonian: QPanda3::Hamiltonian, used_threads: int = 4, backend: str = 'CPU')

# new interface (no-batch processing mode, with NoiseModel)
# If you want to use GPUQVM to calculate the expectation value, the parameter backend needs to be passed with the argument 'CPU'.
expval_hamiltonian(self: vqcircuit.VQCResult, hamiltonian: QPanda3::Hamiltonian, idx_s: list[int], shots: int = 1000, model: QPanda3::NoiseModel, used_threads: int = 4, backend: str = 'CPU')

# new interface (no-batch processing mode, without NoiseModel)
# If you want to use GPUQVM to calculate the expectation value, the parameter backend needs to be passed with the argument 'CPU'.
expval_hamiltonian(self: vqcircuit.VQCResult, hamiltonian: QPanda3::Hamiltonian, idx_s: list[int], used_threads: int = 4, backend: str = 'CPU')
```


(4) Use GPUQVM to calculate the expectation value of a quantum circuit on a quantum system with a specified Hamiltonian. (pyqpanda3.vqcircuit)

new interface

[API doc of pyqpanda3.vqcircuit.VQCResult.expval_hamiltonian](@ref pyqpanda3.vqcircuit.VQCResult.expval_hamiltonian)

```python
# new interface (batch processing mode, with NoiseModel)
# If you want to use GPUQVM to calculate the expectation value, the parameter backend needs to be passed with the argument 'GPU'.
expval_hamiltonian(self: vqcircuit.VQCResult, hamiltonian: QPanda3::Hamiltonian, shots: int = 1000, model: QPanda3::NoiseModel, used_threads: int = 4, backend: str = 'CPU')

# new interface (batch processing mode, without NoiseModel)
# If you want to use GPUQVM to calculate the expectation value, the parameter backend needs to be passed with the argument 'GPU'.
expval_hamiltonian(self: vqcircuit.VQCResult, hamiltonian: QPanda3::Hamiltonian, used_threads: int = 4, backend: str = 'CPU')

# new interface (no-batch processing mode, with NoiseModel)
# If you want to use GPUQVM to calculate the expectation value, the parameter backend needs to be passed with the argument 'GPU'.
expval_hamiltonian(self: vqcircuit.VQCResult, hamiltonian: QPanda3::Hamiltonian, idx_s: list[int], shots: int = 1000, model: QPanda3::NoiseModel, used_threads: int = 4, backend: str = 'CPU')

# new interface (no-batch processing mode, without NoiseModel)
# If you want to use GPUQVM to calculate the expectation value, the parameter backend needs to be passed with the argument 'GPU'.
expval_hamiltonian(self: vqcircuit.VQCResult, hamiltonian: QPanda3::Hamiltonian, idx_s: list[int], used_threads: int = 4, backend: str = 'CPU')
```

(5) Use CPUQVM to calculate the expectation value of a quantum circuit on a quantum system with a specified PauliOperator object. (pyqpanda3.core)

old interface

[API doc of pyqpanda3.CPUQVM.expval_pauli_operator](@ref pyqpanda3.core.CPUQVM.expval_pauli_operator)

```python
# old interface
# note: This old interface is still retained in this version but will be deprecated in the future.
expval_pauli_operator(self: pyqpanda3.core.CPUQVM, prog: pyqpanda3.core.QProg, pauli_operator: QPanda3::PauliOperator, shots: int = 1000, noise_model: pyqpanda3.core.NoiseModel = <pyqpanda3.core.NoiseModel object at 0x000001DCCEEE5E30>)
```

new interface

[API doc of pyqpanda3.core.expval_pauli_operator](@ref pyqpanda3.core.core.expval_pauli_operator)

```python
# new interface
# If you want to use CPUQVM to calculate the expectation value, the parameter backend needs to be passed with the argument 'CPU'.
expval_pauli_operator(node: core.QProg, pauli_operator: QPanda3::PauliOperator, shots: int = 1, model: core.NoiseModel = <core.NoiseModel object at 0x0000026A734CB5B0>, used_threads: int = 4, backend: str = \'CPU\')
```

The example program is as follows.
```python
from pyqpanda3.core import CPUQVM, QProg, S, BARRIER,expval_pauli_operator
from pyqpanda3.hamiltonian import PauliOperator
def test_expectation2():
    # Prepare the quantum simulator
    qvm = CPUQVM()
    # Prepare quantum program
    prog = QProg()
    prog << S(0)
    prog << BARRIER([0, 1, 2])
    """The qubits used in qprog should be consistent with the qubits in the Pauli operator. The simulator determines the qubits that should be included in the quantum system based on the qbits in QProg, which are three quantum #bits in this case"""

    # Prepare the Pauli operator
    op = PauliOperator({"X0 Z1": 2, "X1 Y2": 3})
    print("without I:\n", op)
    # Calculate Expectation
    try:
        # Calculate Expectation
        res = qvm.expval_pauli_operator(prog, op) #old interface,will be deprecated in the future.
        print("res:", res)
        res2 = expval_pauli_operator(prog,op,backend='CPU') # new interface
        print('res2:',res2)
    except Exception as e:
        print(e)

if __name__ == "__main__":
    test_expectation2()
```
Output is :
```bash
without I:
 { qbit_total = 3, pauli_with_coef_s = { 'X0 Z1 ':2 + 0j, 'X1 Y2 ':3 + 0j, } }
res: 0.0
res2: 0.0
Warrning! This interface will be deprecated in the future. Please use the new interface expval_pauli_operator (a global interface exported by pyqpanda3.core).
```

(6) Use GPUQVM to calculate the expectation value of a quantum circuit on a quantum system with a specified PauliOperator object. (pyqpanda3.core)

new interface

[API doc of pyqpanda3.core.expval_pauli_operator](@ref pyqpanda3.core.core.expval_pauli_operator)

```python
# new interface
# If you want to use GPUQVM to calculate the expectation value, the parameter backend needs to be passed with the argument 'GPU'.
expval_pauli_operator(node: core.QProg, pauli_operator: QPanda3::PauliOperator, shots: int = 1, model: core.NoiseModel = <core.NoiseModel object at 0x0000026A734CB5B0>, used_threads: int = 4, backend: str = \'CPU\')
```

The example program is as follows.
```python
from pyqpanda3.core import QProg, S, BARRIER,expval_pauli_operator
from pyqpanda3.hamiltonian import PauliOperator
def test_expectation2():
    # Prepare quantum program
    prog = QProg()
    prog << S(0)
    prog << BARRIER([0, 1, 2])
    """The qubits used in qprog should be consistent with the qubits in the Pauli operator. The simulator determines the qubits that should be included in the quantum system based on the qbits in QProg, which are three quantum #bits in this case"""

    # Prepare the Pauli operator
    op = PauliOperator({"X0 Z1": 2, "X1 Y2": 3})
    print("without I:\n", op)
    # Calculate Expectation
    try:
        # Calculate Expectation
        res2 = expval_pauli_operator(prog,op,backend='GPU') # new interface
        print('res2:',res2)
    except Exception as e:
        print(e)

if __name__ == "__main__":
    test_expectation2()
```
Output is :
```bash
without I:
 { qbit_total = 3, pauli_with_coef_s = { 'X0 Z1 ':2 + 0j, 'X1 Y2 ':3 + 0j, } }
res2: 0.0
```

(7) Use CPUQVM to calculate the expectation value of a quantum circuit on a quantum system with a specified PauliOperator object. (pyqpanda3.vqcircuit)

old interface
```python
# old interface (CPUQVM, batch processing mode, deprecated)
expval_pauli_operator(self: pyqpanda3.vqcircuit.VQCResult, hamiltonian: QPanda3::Hamiltonian, exp_type: pyqpanda3.vqcircuit.ExpectationType = <ExpectationType.THEORETICAL: 0>)

# old interface(CPUQVM, non-batch processing mode, deprecated)
expval_pauli_operator(self: pyqpanda3.vqcircuit.VQCResult, hamiltonian: QPanda3::Hamiltonian, idx_s: list[int], exp_type: pyqpanda3.vqcircuit.ExpectationType = <ExpectationType.THEORETICAL: 0>)
```

new interface

[API doc of pyqpanda3.vqcircuit.VQCResult.expval_pauli_operator](@ref pyqpanda3.vqcircuit.vqcircuit.VQCResult.expval_pauli_operator)

```python
# new interface (batch processing mode, with NoiseModel)
# If you want to use GPUQVM to calculate the expectation value, the parameter backend needs to be passed with the argument 'CPU'.
expval_pauli_operator(self: vqcircuit.VQCResult, pauli_operator: QPanda3::PauliOperator, shots: int = 1000, model: QPanda3::NoiseModel, used_threads: int = 4, backend: str = 'CPU')

# new interface (batch processing mode, without NoiseModel)
# If you want to use GPUQVM to calculate the expectation value, the parameter backend needs to be passed with the argument 'CPU'.
expval_pauli_operator(self: vqcircuit.VQCResult, hamiltonian: QPanda3::Hamiltonian, used_threads: int = 4, backend: str = 'CPU')

# new interface (no-batch processing mode, with NoiseModel)
# If you want to use GPUQVM to calculate the expectation value, the parameter backend needs to be passed with the argument 'CPU'.
expval_pauli_operator(self: vqcircuit.VQCResult, hamiltonian: QPanda3::Hamiltonian, idx_s: list[int], shots: int = 1000, model: QPanda3::NoiseModel, used_threads: int = 4, backend: str = 'CPU')

# new interface (no-batch processing mode, without NoiseModel)
# If you want to use GPUQVM to calculate the expectation value, the parameter backend needs to be passed with the argument 'CPU'.
expval_pauli_operator(self: vqcircuit.VQCResult, hamiltonian: QPanda3::Hamiltonian, idx_s: list[int], used_threads: int = 4, backend: str = 'CPU')
```

(8) Use GPUQVM to calculate the expectation value of a quantum circuit on a quantum system with a specified PauliOperator object. (pyqpanda3.vqcircuit)

[API doc of pyqpanda3.vqcircuit.VQCResult.expval_pauli_operator](@ref pyqpanda3.vqcircuit.vqcircuit.VQCResult.expval_pauli_operator)

new interface 
```python
# new interface (batch processing mode, with NoiseModel)
# If you want to use GPUQVM to calculate the expectation value, the parameter backend needs to be passed with the argument 'GPU'.
expval_pauli_operator(self: vqcircuit.VQCResult, pauli_operator: QPanda3::PauliOperator, shots: int = 1000, model: QPanda3::NoiseModel, used_threads: int = 4, backend: str = 'CPU')

# new interface (batch processing mode, without NoiseModel)
# If you want to use GPUQVM to calculate the expectation value, the parameter backend needs to be passed with the argument 'GPU'.
expval_pauli_operator(self: vqcircuit.VQCResult, hamiltonian: QPanda3::Hamiltonian, used_threads: int = 4, backend: str = 'CPU')

# new interface (no-batch processing mode, with NoiseModel)
# If you want to use GPUQVM to calculate the expectation value, the parameter backend needs to be passed with the argument 'GPU'.
expval_pauli_operator(self: vqcircuit.VQCResult, hamiltonian: QPanda3::Hamiltonian, idx_s: list[int], shots: int = 1000, model: QPanda3::NoiseModel, used_threads: int = 4, backend: str = 'CPU')

# new interface (no-batch processing mode, without NoiseModel)
# If you want to use GPUQVM to calculate the expectation value, the parameter backend needs to be passed with the argument 'GPU'.
expval_pauli_operator(self: vqcircuit.VQCResult, hamiltonian: QPanda3::Hamiltonian, idx_s: list[int], used_threads: int = 4, backend: str = 'CPU')
```

- **The Compilation Module Adds Support for Oracle Gates**

The `Transpiler` and `decompose` under the compilation module both support the handling of Oracle gates.  

Example code is as follows:
```python
from pyqpanda3.core import QProg, CNOT, Oracle
from pyqpanda3.transpilation import Transpiler, decompose

prog = QProg()

matrix = CNOT(0, 1).matrix()
prog << Oracle([0, 1], matrix)

t = Transpiler()
prog2 = t.transpile(prog, [], {}, 2, ['RX', 'RY', 'RZ', 'CNOT'])
prog3 = decompose(prog, ['RX', 'RY', 'RZ', 'CNOT'])

print(prog2)
print(prog3)
```

- **Quantum Circuit Compilation and Decomposition Can Be Decomposed into Specified Gate Sets**

The `Transpiler` and `decompose` under the compilation module both support decomposition into specified basic gate sets.  

Example code is as follows:
```python
from pyqpanda3.core import QProg, random_qcircuit
from pyqpanda3.transpilation import Transpiler, decompose

prog = QProg()
prog << random_qcircuit(list(range(3)), 10, ['RX', 'RY', 'RZ', 'U3', 'H', 'SWAP', 'CNOT'])

t = Transpiler()
basic_gates = ['RX', 'RY', 'RZ', 'CP']
prog2 = t.transpile(prog, [], {}, 2, basic_gates=basic_gates)
prog3 = decompose(prog, basic_gates=basic_gates)

print(prog2)
print(prog3)
```

- **Add matrix decomposition to a specified gate set**

  Add a function overload to the `decompose` interface under the compilation module to decompose a matrix into a specified set of basic gates.  
  Example code is as follows:
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

- **Added Pauli Operator Grouping Functionality**  
    
The `PauliOperator` class has added the `group_commuting` interface, which groups commuting Pauli operators together.  

Example code is as follows:
```python
import random
from pyqpanda3.hamiltonian import PauliOperator


def generate_random_pauli_operator(num_qubits, num: int = 10, max_coeff=1.0):
    """
    Generate a set of random Pauli operators.

    Parameters:
        num_operators (int): The number of Pauli operators to generate.
        num_qubits (int): The number of qubits the operators act on.
        max_coeff (float): The maximum value of the coefficients (default is 1.0).

    Returns:
        list[PauliOperator]: A list of generated random Pauli operators.
    """
    pauli_symbols = ["I", "X", "Y", "Z"]  # Symbols for Pauli operators
    pauli_string_list = []
    coefficient_list = []
    for _ in range(num):
        # Randomly generate Pauli operators for each qubit
        pauli_string = "".join(random.choice(pauli_symbols) for i in range(num_qubits))
        # Randomly generate coefficients (complex numbers)
        real_part = random.uniform(-max_coeff, max_coeff)
        imag_part = random.uniform(-max_coeff, max_coeff)
        coefficient = complex(real_part, imag_part)
        pauli_string_list.append(pauli_string)
        coefficient_list.append(coefficient)

    # Initialize the PauliOperator object using the constructor
    operator = PauliOperator(pauli_string_list, coefficient_list, True)

    return operator


# Generate a random Pauli operator
operator = generate_random_pauli_operator(3, 30)
# Grouping
operators: list[PauliOperator] = operator.group_commuting(True)
```

- **Generate a PauliOperator object from a real matrix**

The example program is as follows.
```python

from pyqpanda3.hamiltonian import PauliOperator

# prepare real matrix
mat =[[0.77386158,0.29688609,0.74116861,0.67669958]
,[0.09515128,0.69212901,0.09219669,0.94389747]
,[0.72101242,0.41921056,0.55843845,0.92963435]
,[0.46651845,0.30803558,0.1703278,0.33562784]]

# Generate a PauliOperator object from a real matrix
op = PauliOperator(mat)
print("op:\n", op)
```
Output is :
```bash
op:
 { qbit_total = 2, pauli_with_coef_s = { 'X0 ':0.373 + 0j, 'Y0 ':-0 -0.24026j, 'X0 Z1 ':-0.176981 + 0j, 'Y0 Z1 ':0 + 0.139393j, 'X1 ':0.678529 + 0j, 'Z0 X1 ':0.052562 + 0j, 'Y1 ':-0 -0.164005j, 'Z0 Y1 ':0 + 0.153926j, 'X0 X1 ':0.413656 + 0j, 'Y0 X1 ':-0 -0.134299j, 'X0 Y1 ':0 + 0.0292082j, 'Y0 Y1 ':-0.157953 + 0j, '':0.590014 + 0j, 'Z0 ':0.0761358 + 0j, 'Z1 ':0.142981 + 0j, 'Z0 Z1 ':-0.0352695 + 0j, } }
```

- **Generate a Hamiltonian object from a real matrix.**

The example program is as follows.
```python

from pyqpanda3.hamiltonian import Hamiltonian

# prepare real matrix
mat =[[0.77386158,0.29688609,0.74116861,0.67669958]
,[0.09515128,0.69212901,0.09219669,0.94389747]
,[0.72101242,0.41921056,0.55843845,0.92963435]
,[0.46651845,0.30803558,0.1703278,0.33562784]]

# Generate a Hamiltonian object from a real matrix
op = Hamiltonian(mat)
print("op:\n", op)
```
Output is :
```bash
op:
 { qbit_total = 2, pauli_with_coef_s = { 'X0 ':0.373 + 0j, 'Y0 ':-0 -0.24026j, 'X0 Z1 ':-0.176981 + 0j, 'Y0 Z1 ':0 + 0.139393j, 'X1 ':0.678529 + 0j, 'Z0 X1 ':0.052562 + 0j, 'Y1 ':-0 -0.164005j, 'Z0 Y1 ':0 + 0.153926j, 'X0 X1 ':0.413656 + 0j, 'Y0 X1 ':-0 -0.134299j, 'X0 Y1 ':0 + 0.0292082j, 'Y0 Y1 ':-0.157953 + 0j, '':0.590014 + 0j, 'Z0 ':0.0761358 + 0j, 'Z1 ':0.142981 + 0j, 'Z0 Z1 ':-0.0352695 + 0j, } }
```
   
- **Generate a matrix from a PauliOperator object.**

[API doc](@ref pyqpanda3.hamiltonian.hamiltonian.PauliOperator.matrix)

The example program is as follows.
```python
from pyqpanda3.hamiltonian import PauliOperator

# prepare PauliOperator object
op = PauliOperator(pauli_with_coef_s = {
    'X0 ':0.373 + 0j, 'Y0 ':-0 -0.24026j, 'X0 Z1 ':-0.176981 + 0j, 'Y0 Z1 ':0 + 0.139393j,
    'X1 ':0.678529 + 0j, 'Z0 X1 ':0.052562 + 0j, 'Y1 ':-0 -0.164005j, 'Z0 Y1 ':0 + 0.153926j,
    'X0 X1 ':0.413656 + 0j, 'Y0 X1 ':-0 -0.134299j, 'X0 Y1 ':0 + 0.0292082j, 'Y0 Y1 ':-0.157953 + 0j,
    '':0.590014 + 0j, 'Z0 ':0.0761358 + 0j, 'Z1 ':0.142981 + 0j, 'Z0 Z1 ':-0.0352695 + 0j, }
)
# Generate a matrix from a PauliOperator object.
mat = op.matrix()
print('mat:\n',mat)
```
Output is :
```bash
mat:
[[0.7738613+0.j 0.296886 +0.j 0.74117  +0.j 0.6766998+0.j]
[0.095152 +0.j 0.6921287+0.j 0.0921958+0.j 0.943898 +0.j]
[0.721012 +0.j 0.4192102+0.j 0.5584383+0.j 0.929634 +0.j]
[0.4665182+0.j 0.308036 +0.j 0.170328 +0.j 0.3356277+0.j]]
```
   
- **Generate a matrix from a Hamiltonian object.**

[API doc](@ref pyqpanda3.hamiltonian.hamiltonian.Hamiltonian.matrix)

The example program is as follows.
```python
from pyqpanda3.hamiltonian import Hamiltonian

# prepare Hamiltonian object
op = Hamiltonian(pauli_with_coef_s = {
    'X0 ':0.373 + 0j, 'Y0 ':-0 -0.24026j, 'X0 Z1 ':-0.176981 + 0j, 'Y0 Z1 ':0 + 0.139393j,
    'X1 ':0.678529 + 0j, 'Z0 X1 ':0.052562 + 0j, 'Y1 ':-0 -0.164005j, 'Z0 Y1 ':0 + 0.153926j,
    'X0 X1 ':0.413656 + 0j, 'Y0 X1 ':-0 -0.134299j, 'X0 Y1 ':0 + 0.0292082j, 'Y0 Y1 ':-0.157953 + 0j,
    '':0.590014 + 0j, 'Z0 ':0.0761358 + 0j, 'Z1 ':0.142981 + 0j, 'Z0 Z1 ':-0.0352695 + 0j, }
)
# Generate a matrix from a Hamiltonian object.
mat = op.matrix()
print('mat:\n',mat)
```
Output is :
```bash
mat:
[[0.7738613+0.j 0.296886 +0.j 0.74117  +0.j 0.6766998+0.j]
[0.095152 +0.j 0.6921287+0.j 0.0921958+0.j 0.943898 +0.j]
[0.721012 +0.j 0.4192102+0.j 0.5584383+0.j 0.929634 +0.j]
[0.4665182+0.j 0.308036 +0.j 0.170328 +0.j 0.3356277+0.j]]
```
    
- **Generate a QASM 2.0 instruction string from a QProg object.**

[API doc of convert_qprog_to_qasm](@ref pyqpanda3.intermediate_compiler.intermediate_compiler.convert_qprog_to_qasm)

The example program is as follows.
```python
from pyqpanda3.intermediate_compiler import convert_qprog_to_qasm
from pyqpanda3.core import QProg,RX,CNOT

# prepare QProg object
prog = QProg()
prog << RX(0,3.1415926)<<CNOT(0,2)
#  Generate a QASM 2.0 instruction string from a QProg object. The precision of the door parameters is set by the second input parameter of the interface.
qasm = convert_qprog_to_qasm(prog,10)
print('qasm:\n',qasm)
```
Output is :
```bash
qasm:
 OPENQASM 2.0;
include "qelib1.inc";
qreg q[3];
creg c[0];
rx(3.1415926000) q[0];
cx q[0],q[2];
```

- **Changes to the implementation related to generating a QProg object from QASM.**

Before the change, different qubit register array names all pointed to the register array named "q"

After the change, different qubit register array names identify different starting register indexes and element counts

[API doc of convert_qasm_string_to_qprog](@ref pyqpanda3.intermediate_compiler.intermediate_compiler.convert_qasm_string_to_qprog)

The example program is as follows.
```python
from pyqpanda3.intermediate_compiler import convert_qasm_string_to_qprog
from pyqpanda3.core import QProg

qasm='''
OPENQASM 2.0;
include "qelib1.inc";
qreg q1[2];
qreg q2[2];
creg c[0];
x q1[0];
y q1[1];
z q2[0];
s q2[1];
'''

prog:QProg = convert_qasm_string_to_qprog(qasm)
print('prog:\n',prog)
print('used qbits in prog:',prog.qubits())
```
Before changing, the output is:
```bash
prog:
 
          ┌─┐ ┌─┐ 
q_0:  |0>─┤X├ ┤Z├ 
          ├─┤ ├─┤ 
q_1:  |0>─┤Y├ ┤S├ 
          └─┘ └─┘ 
 c :   / ═
          


used qbits in prog: [0, 1]
```


After changing, the output is :
```bash
prog:
 
          ┌─┐ 
q_0:  |0>─┤X├ 
          ├─┤ 
q_1:  |0>─┤Y├ 
          ├─┤ 
q_2:  |0>─┤Z├ 
          ├─┤ 
q_3:  |0>─┤S├ 
          └─┘ 
 c :   / ═
          


used qbits in prog: [0, 1, 2, 3]
```

- **Add quantum state preparation functionality.**

Which can be invoked via the [Encode](@ref pyqpanda3.core.core.Encode) class. The supported encoding methods are consistent with those in QPanda2. For specific usage, refer to the [Quantum State Encoding](@ref tutorial_quantum_state_preparation) section.





## Fixed bug
1. Resolved occasional crashes and exceptions when running pyqpanda3 on macOS.

2. Address the issue of errors in merging some Pauli terms in some special cases when constructing a PauliOperator object.

3. Resolve the issue of parsing gate errors when generating a QProg object from QASM.

## Optimize
1. We optimized the simulation performance of CPUQVM by integrating the mimalloc library, achieving an approximately 30% speedup in full-amplitude simulation compared to before.

2. Optimizing the internal implementation of quantum circuits for desired calculations on quantum systems with specified Pauli operators or Hamiltonians.
