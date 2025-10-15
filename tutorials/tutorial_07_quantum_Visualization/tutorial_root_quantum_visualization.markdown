Quantum Visualization  {#tutorial_quantum_visualization}
=============================================================

[TOC]

@prev_tutorial{tutorial_quantum_compilation}
@next_tutorial{tutorial_operator}

-------------------------------------------------------------------------------------------------------------------------------
# Quantum Program Drawing Tutorial

This document provides a guide on how to use the [draw_qprog](@ref pyqpanda3.core.core.draw_qprog) function to generate a graphical representation of a quantum program object (`QProg`). 

## Function Overview

The [draw_qprog](@ref pyqpanda3.core.core.draw_qprog) function generates a graphical representation of a given quantum program. It allows customization of the drawing format and options for detailed visualization.

@note
- expend_map:
  A map used to control drawing expansion options.Default: { {"all", 1} } (expands all subprograms).
  The key represents the name of the circuit to be unfolded,Values can be selected from 0, 1, 2. 0 represents not expanding, 1 represents expanding the current circuit without expanding its sub circuits, and 2 represents expanding all circuits.
- param_show:
  Indicates whether to show parameters in the drawing.Default: false.

**Example Usage**

Here’s a basic example of how to use the `draw_qprog` function:

@add_toggle_python
@snippet samples/python/visualization.py visualization sample
@end_toggle

The result output of the program is as follows:
```python
q_1:  |0>────*── ────── 
          ┌──┴─┐        
q_2:  |0>─┤CNOT├ ───*── 
          └────┘ ┌──┴─┐ 
q_3:  |0>─────── ┤CNOT├ 
                 └────┘ 
 c :   / ═




          ┌─┐
q_0:  |0>─┤H├ ────── ────── 
          ├─┤
q_1:  |0>─┤H├ ───*── ────── 
          └─┘ ┌──┴─┐        
q_2:  |0>──── ┤CNOT├ ───*──
              └────┘ ┌──┴─┐
q_3:  |0>──── ────── ┤CNOT├
                     └────┘
 c :   / ═




          ┌─┐
q_0:  |0>─┤H├ ────────────────
          ├─┤ ┌──────────────┐
q_1:  |0>─┤H├ ┤0             ├
          └─┘ │              │
q_2:  |0>──── ┤1 QCircuit_0  ├
              │              │
q_3:  |0>──── ┤2             ├
              └──────────────┘
 c :   / ═
 ```

It is also possible to use print combined with [set_print_options](@ref pyqpanda3.core.core.set_print_options) to print prog and circuit, which is more convenient.
You can also set the display precision, whether to display parameters, and display length
 
 **Example Usage**

Here’s a basic example of how to use the `set_print_options` function:

@add_toggle_python
@snippet samples/python/visualization.py visualization sample2
@end_toggle

The result output of the program is as follows:
 ```python

 
          ┌─┐ ┌──────────────┐
q_0:  |0>─┤H├ ┤RX(3.14159260)├ ──────
          ├─┤ └──────────────┘
q_1:  |0>─┤H├ ───*──────────── ──────
          └─┘ ┌──┴─┐
q_2:  |0>──── ┤CNOT├────────── ───*──
              └────┘           ┌──┴─┐
q_3:  |0>──── ──────────────── ┤CNOT├
                               └────┘
 c :   / ═




          ┌─┐ ┌────────┐
q_0:  |0>─┤H├ ┤RX(3.14)├ ──────
          ├─┤ └────────┘
q_1:  |0>─┤H├ ───*────── ──────
          └─┘ ┌──┴─┐
q_2:  |0>──── ┤CNOT├──── ───*──
              └────┘     ┌──┴─┐
q_3:  |0>──── ────────── ┤CNOT├
                         └────┘
 c :   / ═




          ┌─┐ ┌──┐
q_0:  |0>─┤H├ ┤RX├── ──────
          ├─┤ └──┘
q_1:  |0>─┤H├ ───*── ──────
          └─┘ ┌──┴─┐
q_2:  |0>──── ┤CNOT├ ───*──
              └────┘ ┌──┴─┐
q_3:  |0>──── ────── ┤CNOT├
                     └────┘
 c :   / ═



 ```