Dynamic Quantum Circuits {#tutorial_dynamic_circuit}  
=============================================================  

[TOC]  

@prev_tutorial{tutorial_quantum_program}  
@next_tutorial{tutorial_circuit_and_program}  

-------------------------------------------------------------------------------------------------------------------------------

# Brief

Dynamic Quantum Circuits represent a novel circuit design in quantum computing, with the core idea of introducing intermediate measurements during circuit execution and providing real-time feedback of the measurement results (classical information) to subsequent operations. This design breaks through the fixed structure of traditional static quantum circuits, enabling quantum computing to dynamically adjust logic gate operations based on intermediate results, thereby optimizing resource utilization and enhancing algorithmic efficiency.

**The dynamic circuit features currently supported by QPanda3 include:**
- Use a concise list of classical bit indices as the evaluation condition for dynamic flow control (the condition is deemed true when the values of all classical bits corresponding to the indices in the list are 1).
- It allows for the selection of subsequent [branch](#branch_qprog_id) circuits to continue execution based on the results of intermediate measurements.
- It allows for [looping](#loop_qprog_id) a certain sub-circuit based on the results of intermediate measurements.

# Branch-based dynamic circuit{#branch_qprog_id}

## Conditional execution

```python
from pyqpanda3.core import QProg,X,Y,Z,S,T,H,measure,qif,CPUQVM

prog = QProg()
prog << X(0) << measure(0,0)  # The value of the cbit with index 0 will ultimately be 1
branch_prog = QProg()
branch_prog << X(2)
branch_prog2 = QProg()
branch_prog2 << X(3)
prog << (
    qif([0]).then(branch_prog)
        .qendif()  # This section of the circuit will be executed
)
prog << measure(1,1)  # The value of the cbit with index 1 will ultimately be 0
prog <<(
    qif([1]).then(branch_prog2)
        .qendif()  # This section of the circuit will not be executed
)
# Theoretical evolution result,
# q0 is flipped to 1, q1 remains 0, q2 is flipped to 1, q3 remains 0, the final state is 0101, i.e., the element with index 5 in the state vector has a value of 1
#### Perform circuit execution verification
qvm = CPUQVM()
qvm.run(prog,1)
stv = qvm.result().get_state_vector()
print('stv:',stv)
```
Output result
```bash
stv: [0j, 0j, 0j, 0j, 0j, (1+0j), 0j, 0j, 0j, 0j, 0j, 0j, 0j, 0j, 0j, 0j]
```

## Binary branch

```python
from pyqpanda3.core import QProg,X,Y,Z,S,T,H,measure,qif,CPUQVM

prog = QProg()
prog << X(0) << measure(0,0)  # The value of the cbit with index 0 will ultimately be 1
branch_prog = QProg()
branch_prog << X(2)
branch_prog2 = QProg()
branch_prog2 << X(3)
prog << (
    qif([0]).then(branch_prog)  # The sub-circuit of this branch will be executed
        .qelse(branch_prog2)    # The sub-circuit of this branch will not be executed
)

# Theoretical evolution result,
# q0 is flipped to 1, q1 remains 0, q2 is flipped to 1, q3 remains 0, the final state is 0101, i.e., the element with index 5 in the state vector has a value of 1
#### Perform circuit execution verification
qvm = CPUQVM()
qvm.run(prog,1)
stv = qvm.result().get_state_vector()
print('stv:',stv)
```
Output result
```bash
stv: [0j, 0j, 0j, 0j, 0j, (1+0j), 0j, 0j, 0j, 0j, 0j, 0j, 0j, 0j, 0j, 0j]
```

## Multi-branch

```python
from pyqpanda3.core import QProg,X,Y,I,Z,S,T,H,measure,qif,CPUQVM

prog = QProg()
prog << X(1)
branch_prog01 = QProg()
branch_prog01 << X(2)<<X(3)
branch_prog0 = QProg()
branch_prog0 << X(2)
branch_prog1 = QProg()
branch_prog1 << X(3)
branch_prog = QProg()
branch_prog <<I(0)
prog << measure(0,0)<<measure(1,1)<<measure(2,2)<<measure(3,3)
prog << (
    qif([0,1]).then(branch_prog01)
        .qelseif([0]).then(branch_prog0)
        .qelseif([1]).then(branch_prog1)  # The sub-circuit of this branch will be executed
        .qelse(branch_prog)
)

# Theoretical evolution result,
# q0 remains 0, q1 is flipped to 1, q2 remains 0, q3 is flipped to 1, the final state is 1010, i.e., the element value at index 10 in the state vector is 1
#### Perform circuit execution verification
qvm = CPUQVM()
qvm.run(prog,1)
stv = qvm.result().get_state_vector()
print('stv:',stv)
```
Output result
```bash
stv: [0j, 0j, 0j, 0j, 0j, 0j, 0j, 0j, 0j, 0j, (1+0j), 0j, 0j, 0j, 0j, 0j]
```
# Loop-based dynamic circuit{#loop_qprog_id}

```python
from pyqpanda3.core import QProg,X,Y,I,Z,S,T,H,measure,qwhile,CPUQVM

prog = QProg()
prog << X(0)
loop_prog = QProg()
loop_prog <<H(0)<<measure(0,0)
prog << measure(0,0)
prog << qwhile([0]).loop(loop_prog)#The loop will definitely execute for the first time, and it will terminate when qubit 0 probabilistically takes on a value of 0.

for i in range(10):
    qvm = CPUQVM()
    qvm.run(prog,1)
    res = qvm.result().get_state_vector()
    print(f'#{i} res:',res)
```
Output result(The measurement results are probabilistic, and the outcomes below may not be the same every time.)
```bash
#0 res: [(1+0j), 0j]
#1 res: [(1+0j), 0j]
#2 res: [(1+0j), 0j]
#3 res: [(-1+0j), 0j]
#4 res: [(1+0j), 0j]
#5 res: [(1+0j), 0j]
#6 res: [(-1+0j), 0j]
#7 res: [(-1+0j), 0j]
#8 res: [(1+0j), 0j]
#9 res: [(1+0j), 0j]
```



