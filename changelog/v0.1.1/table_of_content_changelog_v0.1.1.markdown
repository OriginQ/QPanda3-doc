v0.1.1 {#changelog_0_1_1}
===========================

## New features and important updates
1.The CPUQVM class of the quantum simulator on the CPU supports the simulation of quantum systems with specified noise models and Hamiltonians or Pauli operators, and supports the calculation of the expected results of quantum programs QProg in this system.

2.The full amplitude simulator on the cloud platform supports the simulation of quantum systems with specified noise models and Hamiltonians or Pauli operators, and supports the calculation of the expected results of quantum programs QProg in this system.

3.The interface for the CPUQVM class of the quantum simulator on the CPU to simulate a noise-free quantum system with a specified Hamiltonian or Pauli operator and calculate the expected result of the quantum program QProg in that system has been changed. The new interface is applicable to both setting specific noise model calculations and not setting noise model calculations.

4.The interface for calculating the expected results of the quantum program QProg in the system, where the full amplitude simulator on the cloud platform simulates a noiseless quantum system with a specified Hamiltonian or Pauli operator, has been changed. The new interface is applicable to both setting specific noise model calculations and not setting noise model calculations.

5.The Hamiltonian class adds the interface to_hamiltonian_pq2. The result returned by this interface is consistent with the result of the member function toHamiltonian of the PauliOperator class in QPanda2 (pyqpanda).

6.The PauliOperator class adds the interface to_hamiltonian_pq2. The results returned by this interface are consistent with the results of the member function toHamiltonian of the PauliOperator class in QPanda2 (pyqpanda).

7.We have restructured and updated the interface for obtaining real chip information related to quantum computing in quantum cloud computing services, and now the information obtained is more abundant.

 - Obtain the total number of bits, high fidelity bits, and available qubits


```python
from pyqpanda3.qcloud import QCloudService, LogOutput, ChipInfo

api_key = "302e020100301006010b6d33ad8772eb9705e844394453a3c8a"

service = QCloudService(api_key)
service.setup_logging(LogOutput.CONSOLE)
backend = service.backend("origin_wukong")

chip_info = backend.chip_info()
print(backend.name(), "all qubits :", chip_info.qubits_num())
print(backend.name(), "all available_qubits :", chip_info.available_qubits())
print(backend.name(), "all high_frequency_qubits :", chip_info.high_frequency_qubits())
```

Output is:

```python
origin_wukong all qubits : 72
origin_wukong all available_qubits : [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 19, 20, 21, 22, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 44, 45, 46, 47, 48, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 62, 63, 64, 65, 66, 67, 69, 70, 71]
origin_wukong all high_frequency_qubits : [0, 4, 9, 13, 16, 21, 24, 29, 32, 37, 41, 52, 57, 61, 65, 2, 14, 25, 33, 45, 62, 69, 6, 11, 26, 31, 35, 38, 47, 50, 55, 58, 67, 71, 7, 15, 19, 28, 40, 63, 48, 34, 60, 70, 53, 30, 
54, 44]
```

 - Obtain fidelity and other information for each single quantum logic gate and each set of double quantum logic gates

```python
from pyqpanda3.qcloud import QCloudService, LogOutput

api_key = "302e020100301006072a8648ce3d020106052b8104001c0417"

service = QCloudService(api_key)
service.setup_logging(LogOutput.CONSOLE)
backend = service.backend("origin_wukong")

chip_info = backend.chip_info()

single_qubit_info_list = chip_info.single_qubit_info()
for qubit_info in single_qubit_info_list:
    print(qubit_info)
   
double_qubits_info_list = chip_info.double_qubits_info()
for double_qubits in double_qubits_info_list:
    print("double qubits ",double_qubits.get_qubits(),
          "fidelity : ", double_qubits.get_fidelity()) 
```

Output is:

```python

+---------------------------------------------------------------------------------------------------------------+
|Qubit ID            |Single Gate Fidelity|Readout Fidelity    |Frequency           |T1                  |T2
+----------------------------------------------------------------------------------------------------------------+
|0                   |0.995               |0.9234              |4764                |9.254               |1.128
+----------------------------------------------------------------------------------------------------------------+


+----------------------------------------------------------------------------------------------------------------+
|Qubit ID            |Single Gate Fidelity|Readout Fidelity    |Frequency           |T1                  |T2
+----------------------------------------------------------------------------------------------------------------+
|1                   |0.9973              |0.9417              |4041                |17.436              |1.844
+----------------------------------------------------------------------------------------------------------------+
......

double qubits  [38, 44] fidelity :  0.9831
double qubits  [44, 50] fidelity :  0.0
double qubits  [39, 45] fidelity :  0.0
double qubits  [45, 46] fidelity :  0.9519
double qubits  [45, 51] fidelity :  0.9589
double qubits  [40, 46] fidelity :  0.0
double qubits  [45, 46] fidelity :  0.9519
double qubits  [46, 47] fidelity :  0.0
double qubits  [41, 47] fidelity :  0.964
......
```
8.Modified the `control()` method of `QCircuit` to be an in-place operation, not return the circuit after control.
  - **Usage before change**:

```python
    from pyqpanda3.core import QCircuit
    cir = QCircuit()
    cir = cir.control([1])
```
  - **Usage after change**:

```python
    from pyqpanda3.core import QCircuit
    cir = QCircuit()
    cir.control([1])
```

## Fixed bug
1.Correct the issue where the quantum simulator CPUQVM class on the CPU, the full amplitude simulator on the cloud platform, and the real machine on the cloud platform output incorrect results when calculating expectations for Hamiltonians or Pauli operators.

2.Solved the problem of multiple control gate errors in GPU computing.

3.Solved the issues of crashes and computation anomalies in quantum cloud computing services when submitting queries for extremely large batch tasks.

4.Adapted to usage scenarios in Mac environments, resolved compilation and runtime exceptions in multiple Mac environments.

5.Fixed a bug in the `Transpiler` interface that caused the program to crash due to non-continuous topology.

## Optimize
1.The Unitary class of the quantum information module supports all quantum logic gates listed in the current enumeration type GateType.

2.The internal implementation of the Unitary class in the quantum information module uses optimized computational logic to improve computational efficiency.

3.In the interfaces related to converting OriginIR to QProg and QASM to QProg, if a QProg object is generated that does not contain quantum logic gates, an exception will be thrown.

4.For RPhi gate, the OriignIR instruction string generated by QProg to OriginIR related interface can be recognized by QPanda2 (pyqpanda).
