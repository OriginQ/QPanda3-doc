v0.1.2 {#changelog_0_1_2}
===========================

## Fixed bug
1.Fix the issue where the control semantics display an illegal qubit error when continuously applying control to a circuit.

2.Fix the logical error that occurs after applying the dagger semantics to the original circuit, then adding quantum gates, and subsequently applying the dagger semantics again to the circuit.

3.Resolve the issue of incomplete initial state preparation interface, which can lead to abnormal operator class noise simulation([decoherence_error](@ref pyqpanda3.core.core.decoherence_error)、[amplitude_damping_error](@ref pyqpanda3.core.core.amplitude_damping_error)
).

4.Update the issue where modifications to the cbits function in quantum program and quantum measurement nodes were not updated in a timely manner.


