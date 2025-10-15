v0.3.0 {#changelog_0_3_0}
===========================

## New features and important updates

1.Variational quantum circuits support structural reuse as well as the use of expressions as gate parameters for parameterized gates.

```python
from pyqpanda3.vqcircuit import VQCircuit
from pyqpanda3.core import QCircuit, X, RX, RY, Y

def get_vqc1():
    vqc = VQCircuit()
    vqc.set_Param([2])  # 1> Agree that the parameter θ is a one-dimensional vector with a length of 2, and 2> The parameter θ is bound to the variational quantum circuit U(θ) and cannot be used for other variational quantum circuits
    p0 = vqc.Param([0], 'p0')  # Assign the name p0 to θ_0
    p1 = vqc.Param([1], 'p1')  # Assign the name p1 to θ_1
    e = 3.14 * p0 * p1 + p1 + 4  # An expression composed of elements of the parameter θ (supports mixed operations of addition, multiplication, and scalar multiplication)
    vqc << X(0)  # Add an X gate to the variational quantum circuit U(θ)
    vqc << RX(0, 3.14)  # Add an RX gate to the variational quantum circuit U(θ), with a fixed parameter value of 3.14
    vqc << RY(0, e)  # Add an RY gate to the variational quantum circuit U(θ), the parameters of the gate are variable and are expressions composed of elements of the parameter θ
    return vqc

def get_vqc2():
    vqc2 = VQCircuit()
    vqc2.set_Param([3])
    P0 = vqc2.Param([0], 'P0')
    P1 = vqc2.Param([1], 'P1')
    P2 = vqc2.Param([2], 'P2')
    cir = QCircuit()
    cir << Y(0)
    vqc2 << cir  # Add all quantum logic gates in the QCircuit object to the variational quantum circuit U(θ)
    vqc1 = get_vqc1()  # For the convenience of distinction, the parameter θ corresponding to vqc1 is called θ1, and the parameter θ corresponding to vqc2 is called θ2; The two elements of θ1 are θ1[0] and θ1[1], and their respective names are p0 and p1; The three elements of θ2 are θ2[0], θ2[1], and θ2[2], and their respective names are P0, P1, and P2
    vqc2.append(vqc1, [(vqc1.Param([0]), vqc2.Param([0])), (vqc1.Param([1]), vqc2.Param([1]))])  # Reuse the structure of vqc1. For the parameter θ, θ1[0] will be replaced with θ2[0], and θ1[1] will be replaced with θ2[1]
    return vqc2

vqc1 = get_vqc1()
print('vqc1:')
vqc1.display_ansatz()  # Print the structure of the variational quantum circuit
print()
vqc2 = get_vqc2()
print('vqc2:')
vqc2.display_ansatz()
```
Output result of the example code
```bash
vqc1:
X q[0]
RX q[0],(3.14000000)
RY q[0],(((3.14*p0*p1+p1)+4))

vqc2:
Y q[0]
X q[0]
RX q[0],(3.14000000)
RY q[0],(((3.14*P0*P1+P1)+4))
```

2.Variational quantum circuits support gradient calculation and provide an interface to obtain both the expectation value and the gradient value simultaneously.

```python
from pyqpanda3.vqcircuit import VQCircuit,DiffMethod
from pyqpanda3.core import QCircuit,X,RX,RY,Y,CPUQVM,QProg
from pyqpanda3.hamiltonian import Hamiltonian
def get_vqc():
    # Prepare the variational quantum circuit U(θ)
    vqc = VQCircuit()
    vqc.set_Param([2])  # 1> Agree that the parameter θ is a one-dimensional vector with a length of 2, and 2> The parameter θ is bound to the variational quantum circuit U(θ) and cannot be used for other variational quantum circuits
    vqc << RX(0, vqc.Param([0]))  # Add a parameterized quantum gate RX, whose parameter corresponds to the first element of the vector θ
    vqc << RY(1, vqc.Param([1]))
    return vqc

def get_param_val():
    # Prepare the actual numerical values θval for the parameter θ
    param_val = [2.14, 3.14]  # The number of elements should be consistent with the number of elements in the parameter θ (vector)
    return param_val

def get_hamiltonian():
    # Prepare the Hamiltonian operator H
    paulis = [
        ('YY', [0, 1], (12.36525580995888 + 14.85172018664403j)),  # (Pauli basis, qubit indices on which the Pauli basis acts, coefficient)
        ('YX', [0, 1], (12.920260765526914 + 26.29613065236354j))  # The qubit indices used should be consistent with the qubit indices used in the quantum circuit
    ]
    return Hamiltonian(paulis)

def get_gradient(vqc, param_val, ham):
    # Obtain the gradient value (Method 1)
    return vqc.get_gradients(params=param_val, observable=ham, diff_method=DiffMethod.ADJOINT_DIFF)

def get_gradient2(vqc: VQCircuit, param_val, ham):
    # Obtain the gradient value (Method 2)
    # The interface get_gradients_and_expectation can obtain both the gradient value and the expectation value simultaneously
    return vqc.get_gradients_and_expectation(params=param_val, observable=ham, diff_method=DiffMethod.ADJOINT_DIFF)

vqc = get_vqc()  # Prepare the variational quantum circuit U(θ)
param_val = get_param_val()  # Prepare the actual numerical values θval for the parameter θ
ham = get_hamiltonian()  # It is recommended not to use H as the variable name to avoid naming conflicts with the H gate
res1 = get_gradient(vqc, param_val, ham)  # Obtain the gradient value (Method 1)
res2 = get_gradient2(vqc, param_val, ham)  # Obtain the gradient value (Method 2)
print('res1:',res1)
print('res1 gradient:',res1.gradients())
print('res2:',res2)
print('res2 gradient:',res2.gradients())
```
Output result of the example code
```bash
res1: {gradient(double):{Parameter.at([0]):0.0110905,Parameter.at([1]):23.3932}}
res1 gradient: [0.011090474368968659, 23.39317090007428]
res2: (gradient and expectation){"expectation":-0.017333,"gradients":{Parameter.at([0]):0.0110905,Parameter.at([1]):23.3932}}
res2 gradient: [0.011090474368968659, 23.39317090007428]
```

3.Add support for SQISWAP, ECHO, IDLE, CRX, CRY, and CRZ gates

```python
from pyqpanda3.intermediate_compiler import convert_qprog_to_originir,convert_originir_string_to_qprog
from pyqpanda3.core import QProg
from pyqpanda3.core import X,CP,CRX,CRY,CRZ,ECHO,SQISWAP,IDLE
from pyqpanda3.vqcircuit import VQCircuit,DiffMethod
ir_str="""
QINIT 2
CREG 1
CRX q[0],q[1],(3.14000000)
CRY q[0],q[1],(4.14000000)
CRZ q[0],q[1],(5.14000000)
ECHO q[0]
SQISWAP q[0],q[1]
IDLE q[0],(3.66666006000)
"""

prog = QProg(ir_str)    # OriginIR -> QProg
ir_2 = prog.originir(8) # QProg -> OriginIR
print('ir_2:\n',ir_2)

prog3 = QProg()
prog3 << CRX(0,1,30.14) # append CRX to prog3
prog3 << CRY(0,1,40.14) # append CRY to prog3
prog3 << CRZ(0,1,50.14) # append CRZ to prog3
prog3 << ECHO(0) # append ECHO to prog3
prog3 << SQISWAP(0,1)   # append SQISWAP to prog3
prog3 << IDLE(0,30.66666) # append IDLE to prog3
ir_3 = prog3.originir(8) # QProg -> OriginIR
print('ir_3:\n',ir_3)

vqc = VQCircuit()
vqc.set_Param([0])
vqc << CRX(0,1,30.14) # append CRX to vqc
vqc << CRY(0,1,40.14) # append CRY to vqc
vqc << CRZ(0,1,50.14) # append CRZ to vqc
vqc << ECHO(0) # append ECHO to vqc
vqc << SQISWAP(0,1)   # append SQISWAP to vqc
vqc << IDLE(0,30.66666) # append IDLE to vqc
cir = vqc([]).at([0])
print('cir ir:\n',cir.originir(8))
```
Output result of the example code
```bash
ir_2:
 QINIT 2
CREG 1
CONTROL q[0]
RX q[1],(3.14000000)
ENDCONTROL
CONTROL q[0]
RY q[1],(4.14000000)
ENDCONTROL
CONTROL q[0]
RZ q[1],(5.14000000)
ENDCONTROL
ECHO q[0]
SQISWAP q[0],q[1]
IDLE q[0],(3.00000000)

ir_3:
 QINIT 2
CREG 1
CONTROL q[0]
RX q[1],(30.14000000)
ENDCONTROL
CONTROL q[0]
RY q[1],(40.14000000)
ENDCONTROL
CONTROL q[0]
RZ q[1],(50.14000000)
ENDCONTROL
ECHO q[0]
SQISWAP q[0],q[1]
IDLE q[0],(30.00000000)

cir ir:
 QINIT 2
CREG 1
CONTROL q[0]
RX q[1],(30.14000000)
ENDCONTROL
CONTROL q[0]
RY q[1],(40.14000000)
ENDCONTROL
CONTROL q[0]
RZ q[1],(50.14000000)
ENDCONTROL
ECHO q[0]
SQISWAP q[0],q[1]
IDLE q[0],(30.00000000)
```

4.Added the `ChipBackend` class, which stores the information used by the chip backend for compilation, and added the `transpile` interface, which compiles a quantum program to a specified chip backend by passing in the `ChipBackend` parameter. Additionally, QCloud has added the functionality to obtain the chip backend from the cloud platform.

Example usage is as follows:

```python
import time
from pyqpanda3.qcloud import *
from pyqpanda3.core import *
from pyqpanda3.transpilation import Transpiler

apikey = ''

def test_backend():
    print("test_get_topo")
    service = QCloudService(apikey)
    # service.setup_logging()
    backends = service.backends()
    for backend in backends:
        print(backend)

    backend_wukong = service.backend("WK_C102_400")
    backend = backend_wukong.chip_info().get_chip_backend()

    all_qbits = list(range(10))
    circuit = random_qcircuit(all_qbits, 100, ["RX", "RY", "RZ", "H", "U1", "U2", "U3", "CP", "CNOT", "SWAP"])

    prog_1 = QProg()
    prog_1 << circuit
    for i in all_qbits:
        prog_1 << measure(i, i)

    transpiler = Transpiler()
    print("Enter transpile")
    prog_2 = transpiler.transpile(prog_1, backend, {}, 2)

if __name__ == "__main__":
    test_backend()
```

5. Added the interface for exporting quantum chip instructions in `QProg`. Quantum chip instructions are files that can be directly sent to the chip for execution. Currently, the instruction format of this interface only supports the original quantum computer.
Using this interface requires that the quantum program already consists entirely of the chip's basic instructions (RPhi, CZ, Measure, and ECHO).
`to_instruction` has three parameters. The first parameter is the specified chip backend. The second parameter is the offset of the qubit during the instruction conversion process. The third parameter is whether to use pattern to layer the instructions.


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
    backend_wukong = service.backend("WK_C102_400")
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

5.Quantum cloud services add instruction set tasks, which can compile quantum programs into instruction sets supported by the specified backend and then directly run the instruction set tasks.

```python
from pyqpanda3.qcloud import QCloudService,QCloudOptions
from pyqpanda3.core import CZ, measure, QProg

prog = QProg()
prog << CZ(9, 10)
prog << CZ(10, 11)
prog.append(measure(9,9))

online_key = "api_key"

service = QCloudService(online_key)
service.setup_logging()
backend = service.backend("WK_C102_400")

options = QCloudOptions()
options.set_amend(False)

ins = prog.to_instruction(backend.chip_backend())
job = backend.run_instruction([ins,ins], 1000, options)

chip_info = backend.chip_info()

result = job.result()
print(result.get_probs_list())
```

The output is:

```python
[{'0x0': 0.7249999761581421, '0x1': 0.2749999940395355}, {'0x0': 0.75, '0x1': 0.25}]
```



## Fixed bug

1.Fix the occasional crash issue that occurs during OrginIR conversion in multi-threaded programs.

2.Fix the error that occurs when SIMD optimization is triggered for multi-controlled gates with more than 14 qubits.

3.Optimize the decomposition algorithm for multi-controlled gates to improve efficiency.

4.Enhance the performance of the transpile interface.
