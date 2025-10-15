Hellinger Fidelity  {#tutorial_table_of_content_QI_HellingerFidelity}
=============================================================================

[TOC]

@prev_tutorial{tutorial_table_of_content_QI_HellingerDistance}
@next_tutorial{tutorial_table_of_content_QI_KL_divergence}

-------------------------------------------------------------------------------------------------------------------------------

# Introduction

Computes the Hellinger fidelity between two counts distributions.

The fidelity is defined as 
\f[
  \left(1-H^{2}\right)^{2}
\f]
where H is the Hellinger distance Please see Please see [Hellinger distance](#tutorial_table_of_content_QI_HellingerDistance).. This value is bounded in the range \f$[0, 1]\f$.

This is equivalent to the standard classical fidelity 
\f[
F(Q, P)=\left(\sum_{i} \sqrt{p_{i} q_{i}}\right)^{2}
\f]
that in turn is equal to the quantum state fidelity for diagonal density matricess.

# In QPanda3

[Here is API doc for hellinger_fidelity](@ref pyqpanda3.quantum_info.quantum_info.hellinger_fidelity)

## Using integer as the random variable value

@add_toggle_python
    @snippet samples\python\QuantumInformation\Analysis\hellinger_fidelity.py Using integer as the random variable value
@end_toggle

Output
```bash
test_hellinger_fidelity_1()
sum(p_prob_val): 0.9999999999999999
sum(q_prob_val): 0.9999999999999999
p_prob: {5: 0.04854810353110398, 1: 0.009079698162282953, -16: 0.03646928990147027, -26: 0.024478635979747115, -37: 0.012346440874469804, -24: 0.05275981503348704, 23: 0.025130767321854923, 7: 0.01782340035264664, 34: 0.004729714212215611, -14: 0.10819617572813868, 8: 0.051803218470062015, 27: 0.011460093215183802, -1: 0.005134603298097388, 24: 0.009083772900411497, -15: 0.1603100882346676, -18: 0.047817616053414884, -34: 0.036364876495667164, -40: 0.22340527372012, -33: 0.056140353466916704, 26: 0.058918063048041855}
q_prob: {5: 0.09340420649225029, 1: 6.87096659272463e-05, -16: 0.03862071915991972, -26: 0.05399225928912998, -37: 0.07099124608467228, -24: 0.047144812840227035, 23: 0.09267654048044585, 7: 0.04441827322501288, 34: 0.08626934864863559, -14: 0.03915518088648987, 8: 0.042059983612428936, 27: 0.03582921175475073, -1: 0.015179192334639068, 24: 0.09061653931041332, -15: 0.006064973994523014, -18: 0.0738095483130895, -34: 0.04741652216211396, -40: 0.026438659880627324, -33: 0.015171225507386716, 26: 0.0806728463573166}
hd: 0.6316001932101935
```

## Using large integer as the random variable value

@add_toggle_python
    @snippet samples\python\QuantumInformation\Analysis\hellinger_fidelity.py Using integer as the random variable value
@end_toggle

Output
```bash
test_hellinger_fidelity_2()
p_prob: {1000000000001: 0.12022119026479423, 1000000000002: 0.2806992297584853, 1000000000003: 0.5990795799767205}
q_prob: {1000000000001: 0.09914339060260904, 1000000000002: 0.5537924049116996, 1000000000003: 0.3470642044856914}
hd: 0.9204993678602602
```

## Using string as random variable value

@add_toggle_python
    @snippet samples\python\QuantumInformation\Analysis\hellinger_fidelity.py Using integer as the random variable value
@end_toggle

Output
```bash
test_hellinger_fidelity_3()
p_prob: {'ab': 0.8238217892709107, 'bc': 0.17617821072908926}
q_prob: {'ab': 0.5325124974169014, 'bc': 0.46748750258309846}
hd: 0.9012219516940595
```