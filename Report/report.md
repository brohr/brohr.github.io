---
layout: page
mathjax: true
permalink: /report/
---

## Contents ##

1. [Free Energy Corrections](#entropy)
2. [Free Energy Diagram](#analysis)


<a name='entropy'></a>

## Free Energy Corrections ##

The binding energies that you have calculated are electronic binding energies. They do not incluide zero point energy, heat capacity, or entropy. The electronic energy is a useful physical quantity, but we are also interested in binding free energies. We can convert electronic energies to free energies by adding the following corrections. Note that the corrections are only valid at 1 bar H2 and the vapor pressure of water at room temperature.

Free energy correction for O: +0.07 eV
Free energy correction for H: +0.3 eV

For every adsorbed species, add the above correction to the electronic energy. For example, OOH's binding energy should be increased by 2*0.07 + 0.3 eV. When applying the free energy correction to the barrier, 


<a name='analysis'></a>

## Free Energy Diagram ##

## OER ##

We will begin by creating a free energy diagram at 0V. [Here](/LimitingPotentialToyExample.xlsx) is an example of a toy free energy diagram. If you change the voltage to 0V, you can see what a normal free energy diagram looks like.

At each step in OER, there reaction may only proceed in one of two ways. They are 1) addition of an OH (really the adsorption of a water molecule with one H splitting into a proton, which is released into solution, and an electron, which flows into the anode), or 2) the desorption of an adsorbed hydrogen (really the splitting of an adsorbed hydrogen into a proton, which is released into solution, and an electron, which flows into the anode).

On the monomer:
The monomer is quite easy to analyze because there is only one possibility at each step. The reaction must procede through the following pathway:
1) clean (this state really has two waters above it. Since the clean catalyst with water is our reference, we can call this state 0  eV)
2) adsorption energy of OH*
3) adsorption energy of O*
4) adsorption energy of OOH*
5) clean (this state has an O2 gas molecule above it. Since O2 gas is 4.92 eV uphill from water and hydrogen, our reference, you need to add 4.92 eV to this energy)

On the dimer (both the close and the far configurations):
Again, start with the clean catalyst. Again, we can call this 0 eV.

The first step must be the addition of OH* (going to clean_OH)
The second step has two possibilities: a) addition of OH to the other metal (going to OH_OH) or b) the removal of a hydrogen (going to clean_O). You should choose the one that is more stable (more downhill in free energy) for your catalyst.

At each step, choose the lowest energy option. The flowchart below should help guide your thinking. Eventually (in fewer than 7 steps), you should arrive at an intermediate that you've already seen. At this point, you have found your catalyst's OER loop. For now, ignore the O-O coupling pathway.


## ORR ##






