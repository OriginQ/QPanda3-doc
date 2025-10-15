Noise  {#tutorial_noise}
=============================================================================

[TOC]

@prev_tutorial{tutorial_quantum_simulator}
@next_tutorial{tutorial_quantum_circuit_transpiler}

-------------------------------------------------------------------------------------------------------------------------------

## Brief
In practical quantum computers, computational errors are inevitable due to the physical characteristics of qubits. To better simulate these errors in a quantum virtual machine, QPanda3 introduces various noise models and error models, making the simulation process closer to real quantum computers. These noise and error models allow for fine-tuned adjustments of qubits, quantum logic gates, and error parameters, satisfying a variety of complex noise scenarios.

### Introduction to Noise Models

- amplitude_damping_error

The [amplitude_damping_error](@ref pyqpanda3.core.core.amplitude_damping_error) represents the relaxation noise model of a qubit. Its Kraus operators are defined as follows:

\f[ 
K_1 = \begin{bmatrix} 1 & 0 \\ 0 & \sqrt{1 - p} \end{bmatrix}, \quad
K_2 = \begin{bmatrix} 0 & \sqrt{p} \\ 0 & 0 \end{bmatrix} 
\f]

This model requires a single noise parameter.

- pauli_z_error

The [pauli_z_error](@ref pyqpanda3.core.core.pauli_z_error) represents the dephasing noise model of a qubit. Its Kraus operators are defined as follows:

\f[ 
K_1 = \begin{bmatrix} \sqrt{1 - p} & 0 \\ 0 & \sqrt{1 - p} \end{bmatrix}, \quad
K_2 = \begin{bmatrix} \sqrt{p} & 0 \\ 0 & -\sqrt{p} \end{bmatrix} 
\f]

This model requires a single noise parameter.

- decoherence_error

The [decoherence_error](@ref pyqpanda3.core.core.decoherence_error) combines the above two models (relaxation and dephasing noise). Their relationship is described as follows:

\f[ 
P_{\text{damping}} = 1 - e^{-\frac{t_{\text{gate}}}{T_1}}, \quad
P_{\text{dephasing}} = 0.5 \times \big(1 - e^{-(\frac{t_{\text{gate}}}{T_2} - \frac{t_{\text{gate}}}{2T_1})}\big) 
\f]

The combined Kraus operators are:

\f[ 
K_1 = K_{1_{\text{damping}}} K_{1_{\text{dephasing}}}, \quad
K_2 = K_{1_{\text{damping}}} K_{2_{\text{dephasing}}}, \quad
K_3 = K_{2_{\text{damping}}} K_{1_{\text{dephasing}}}, \quad
K_4 = K_{2_{\text{damping}}} K_{2_{\text{dephasing}}} 
\f]

This model requires three noise parameters.

- depolarizing_error

The [depolarizing_error](@ref pyqpanda3.core.core.depolarizing_error) represents depolarizing noise, where a single qubit may collapse into a completely mixed state (I/2) with a certain probability. The Kraus operators are:

\f[ 
K_1 = \sqrt{1 - 3p/4} \cdot I, \quad
K_2 = \sqrt{p}/2 \cdot X, \quad
K_3 = \sqrt{p}/2 \cdot Y, \quad
K_4 = \sqrt{p}/2 \cdot Z 
\f]

Here, \f$I, X, Y, Z \f$ represent the quantum logic gate matrices.

This model requires a single noise parameter.

- pauli_x_error

The [pauli_x_error](@ref pyqpanda3.core.core.pauli_x_error) represents pauli_x_error noise. Its Kraus operators are defined as follows:

\f[
K_1 = \begin{bmatrix} \sqrt{1 - p} & 0 \\ 0 & \sqrt{1 - p} \end{bmatrix}, \quad
K_2 = \begin{bmatrix} 0 & \sqrt{p} \\ \sqrt{p} & 0 \end{bmatrix} 
\f]

This model requires a single noise parameter.

- pauli_y_error

The [pauli_y_error](@ref pyqpanda3.core.core.pauli_y_error) represents bit-phase flip noise. Its Kraus operators are:

\f[
K_1 = \begin{bmatrix} \sqrt{1 - p} & 0 \\ 0 & \sqrt{1 - p} \end{bmatrix}, \quad
K_2 = \begin{bmatrix} 0 & -i \times \sqrt{p} \\ i \times \sqrt{p} & 0 \end{bmatrix} 
\f]

This model requires a single noise parameter.

- phase_damping_error

The [phase_damping_error](@ref pyqpanda3.core.core.phase_damping_error) represents phase damping noise. Its Kraus operators are:

\f[ 
K_1 = \begin{bmatrix} 1 & 0 \\ 0 & \sqrt{1 - p} \end{bmatrix}, \quad
K_2 = \begin{bmatrix} 0 & 0 \\ 0 & \sqrt{p} \end{bmatrix} 
\f]

This model requires a single noise parameter.

### Noise Error

The noise errors and noise models in pyqpanda3 are separated. Firstly, we need to construct the noise errors, and then set them through the noise model. In the morning, the model can be set to support which quantum bits and specific quantum logic gates

```python

    import os
    import sys

    from pyqpanda3.core import *

    # pauli x error , There is a probability of 0.5 that flipping qubit
    x_error = pauli_x_error(0.5)

    # pauli x error , There is a probability of 0.5 that an additional Y gate will be applied
    y_error = pauli_y_error(0.5)

    # pauli z error , There is a probability of 0.5 that an additional Z gate will be applied
    z_error = pauli_z_error(0.5)

    # kraus Error
    amplitude_damping_error = amplitude_damping_error(0.5)
    decoherence_error = decoherence_error(5, 5, 0.2)
    depolarizing_error = depolarizing_error(0.5)
```

The [NoiseModel](@ref pyqpanda3.core.core.NoiseModel) can be used to set noise errors, including specific applications to quantum bits and quantum logic gates

```python
from pyqpanda3.core import QCircuit,H,RX,CNOT,QProg,CPUQVM,measure
from pyqpanda3.core import NoiseModel,pauli_x_error,depolarizing_error,GateType

circuit = QCircuit(2)
circuit << H(0)
circuit << CNOT(0, 1)
circuit << RX(0, 3.6)

prog = QProg()
prog << circuit
prog << measure([0,1,2],[0,1,2])

model = NoiseModel()

# Set noise to apply to all qubits
model.add_all_qubit_quantum_error(pauli_x_error(0.5),GateType.RX)
model.add_all_qubit_quantum_error(pauli_x_error(0.9),GateType.H)

# Set noise to apply to select qubits
model.add_quantum_error(depolarizing_error(0.5),GateType.H,[0,1,2])

machine = CPUQVM()
machine.run(prog, 1000, model)

print(machine.result().get_counts())
```

The output is as follows.

```python
{'001': 468, '010': 479, '011': 24, '000': 29}
```
