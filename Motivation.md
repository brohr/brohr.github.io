---
layout: page
mathjax: true
permalink: /Motivation/
---

## Course Project ##
1. [Introduction](#intro)
3. [Calculations](#calcs)
3. [Analysis](#analysis)
4. [Report](#report)
5. [Research Paper](#paper)
6. [Grading](#grading)
7. [Summary of Requirements](#reqs)


In this project we will be trying to overcome one of the fundamental challenges in two related electrochemical reactions:
- The oxygen evolution reaction (OER), also known as "water oxidation"
- The oxygen reduction reaction (ORR)
The due date is <font color="red">3/17 at 11:59 PM (hard deadline)</font>.

Turn in your final report by emailing a PDF file to all TA's:

```
brohr@stanford.edu, aayush@stanford.edu
```

<a name='intro'></a>

## Introduction ##

In the electrochemical oxygen reduction reaction (ORR), dissolved oxygen reacts at the cathode with protons from the electrolyte solution and electrons from the cathode via:

$$
\mathrm{2O_{2\,(aq)} + 4H^+_{(aq)} + 4e^- \rightarrow 2H_2O}
$$

This is the most common cathodic reaction used in fuel cells, so making it as efficient as possible is of great interest to industry. In fact, Toyota has recently invested a great deal of money in ORR catalyst research. Making this process more efficient would make carbon-free energy storage more economically viable.

$$
\begin{align}
\mathrm{O_{2} + H^+ + e^- + * \rightarrow OOH*}\\
\mathrm{OOH* + H^+ + e^- \rightarrow O* + H_2O}\\
\mathrm{O* + H^+ + e^- \rightarrow OH*}\\
\mathrm{OH* + H^+ + e^- \rightarrow * + H_2O}\\
\end{align}
$$

A free energy diagram is illustrated below:

<center><img src="../Images/OER_FED.png" alt="OER" style="width: 450px;"/>
<br>Nørskov, Rossmeisl, Logadottir, Lindqvist, Kitchin, Bligaard, Jonsson, J. Phys. Chem. 108, 17886 (2004)</center>

There is a substantial chemical driving force to reduce oxygen to and form water. The reaction above is exergonic by 4.92 eV when no external potential is applied. In ORR, this driving force is harnessed to do useful work - in other words, the chemical driving force works against an oxidizing potential to do work. We would like to apply the largest oxidizing potential possible in order to do the most work, but as the oxidizing potential increases, the reduction of oxygen to water becomes less and less exergonic (less downhill in free energy). If the applied potential is above 1.23 V (4.92 eV / 4 proton electron transfers), then ORR is uphill, and water would split into oxygen and proton electron pairs instead. Therefore, the theoretical highest voltage that can be applied is 1.23 V. However, ORR will not procede unless each of the four elementary steps above are downhill in free energy. The theoretical best 1.23 V potential can only be realized if the delta G of each elementary step is identical. This is not the case on any known catalyst.

One solution you may think of is to tune the catalyst so that the binding energies of each intermediate are such that each elementary step has the same free energy. Indeed, changing to a different catalyst will change the binding energy of a surface species (intermediate). Therefore, one can change the delta G of an elementary reaction step by changing catalysts. The trouble is, changing catalysts changes the binding energies of all intermediates, not just one. In other words, changing catalysts can not be used to change the delta G of one elementary reaction independent of the others. There is a very good physical reason for that, and it will be discussed in more detail in CHEMENG 242 in the spring. Looking at the free energy diagram, you can see that in order to achieve the ideal scenario where each elementary step has the same delta G, we need to stabilize OOH* without stabilizing OH* or O*. Changing to a more reactive catalyst will indeed stabilize OOH*, but it will also stabilize O* and OH*.


Since the last two steps of the pathway are quite ideal (have approximately the same delta G), one way to avoid the OOH* problem is simply to run the reaction by a pathway that does not include OOH*. If O2 adsorbs onto the surface and thermally overcomes the barrier to split into 2O*, then the rest of the ORR pathway could be run at high efficiency, circumventing OOH* entirely. On all known good ORR catalysts, the barrier for this process is insurmountably high. However, the barrier is hypothesized to be lower on the systems we'll be working with. In this project, you will analyze this pathway.

<a name='calcs'></a>

## Calculations ##

For Sherlock, create a `CHE444Project` folder in your `$SCRATCH` directory by running:

```bash
mkdir $SCRATCH/CHE444Project/
```

for CEES:

```bash
mkdir /data/cees/$USER/CHE444Project
```

You may run the exercises in any directory (as long as it is under `$SCRATCH` for Sherlock and `/data/cees/$USER` for CEES), but keep all the final files for the project organized.

For the first step, N<sub>2</sub> → 2N\*, you *are required* to calculate the transition state. To do this, you will need to calculate the final adsorbed state with two nitrogen atoms (2N\*).

To describe the full reaction on your catalytic system, you will need to calculate the adsorption energies of all intermediates, in their most stable configuration (N\*, NH\*, NH<sub>2</sub>\*, NH<sub>3</sub>\*, H\*). A mean field approximation can be used in the analysis (*e.g.* ∆*E*<sub>2NH</sub> = 2∆*E*<sub>NH</sub>). You are not required to calculate the transition states for these steps, though you are welcome to.

In summary:

1. Structural relaxations on both your assigned M<sub>13</sub> cluster and a (111) surface for the same metals. [Project Part 1](../ASE/Getting_Started)
2. **For the M<sub>13</sub> cluster only**: Adsorption energies for the remaining intermediates in the adsorbed state (N\*, NH\*, NH<sub>2</sub>\*, NH<sub>3</sub>\*, H\*). Check all possible sites in order to determine optimal adsorption configurations. [Project Part 2](../ASE/Adsorption)
3. **For both M<sub>13</sub> cluster and metal surface**: Fixed bond length (FBL) calculation for the activation barrier for N<sub>2</sub> → 2N\*. You will need to perform an adsorption energy calculation for 2N\*, which will serve as the starting point for the FBL. [Project Part 3](../ASE/Transition_States)
4. Vibrational analysis for the transition state **and** the adsorbed states. Calculation of the reaction rate and also a free energy diagram with some temperature and pressure dependence. [Project Part 3](../ASE/Transition_States)

Once you have finished the required calculations, you are free to explore other features of the reaction as you see fit.

**IMPORTANT:**

When you have finished all your calculations. Confirm that your results are organized in the following way:

```bash
../CHE444Project/Adsorption/
../CHE444Project/Adsorption/Surface/
../CHE444Project/Adsorption/Surface/2N/
../CHE444Project/Adsorption/Surface/2N/config1
...
../CHE444Project/Adsorption/Cluster/
../CHE444Project/Adsorption/Cluster/2N/
../CHE444Project/Adsorption/Cluster/NH/
../CHE444Project/Adsorption/Cluster/NH/config1
../CHE444Project/Adsorption/Cluster/NH/config2
...
../CHE444Project/TransitionStates/Cluster/2N_to_N2/
../CHE444Project/TransitionStates/Cluster/2N_to_N2/config1
...
../CHE444Project/Vibrations/N/
../CHE444Project/Vibrations/2N/
...
```

where `CHEMENG444Project` is your **project directory**. You should rename `config1` to something that describes the binding configuration, such as `BrBr` for two bridging sites. You should have one calculation per directory. Run the following to copy all your files into the shared course directory, so your classmates may access the results.

On Sherlock, from your **project directory**, run:

```bash
/scratch/PI/suncat/chemeng444_2016/submit
```

On CEES, from your **project directory**, run:

```bash
/data/cees/cheme444/submit
```

<a name='analysis'></a>

## Analysis ##

Your analysis should include the following:

* Discussion of the optimal binding configurations on the surface
* Comparison between the (111) surface and the M<sub>13</sub> cluster
* Analysis of rate as a function of the temperature

Beyond these points, you may discuss anything you find interesting. Here are some ideas:

* Do the reaction intermediates (adsorbates) show the same dependence on surface site or surface termination?
* What are other factors that can affect adsorbate-metal interaction?
* How important is the coordination number of the metal? Compare the metal cluster to the metal surface.

You are welcome to share data amongst your peers to discuss broader trends. (We encourage you to use [Piazza](http://piazza.com/class/ij0k0xrcxrz5pa) for questions and discussions so you can help each other troubleshoot and share data. You may also include additional calculations into your project (scripts available), but this is not required:

* Density of states calculations on the surfaces
* Error analysis using the BEEF ensembles

**If you need the energy of the fixed clusters, they are available [here](../Fixed_Lattice_Clusters/energies.txt).**

<a name='report'></a>

## Report ##

Your report should be between 3 to 5 pages long including figures and tables. Please be succinct and organize it in the following way:

* Introduction (brief) - don't write too much
* Calculation details
* Results and discussion
* Conclusion (brief)

<a name='paper'></a>

## Research Paper ##

We expect that your results will form the basis of a manuscript that we will be putting together as a class. For previous class papers:

* [(2011) Finite-Size Effects in O and CO Adsorption for the Late Transition Metals, *Topics in Catalysis*](http://dx.doi.org/10.1007/s11244-012-9908-x)
* [(2015) Direct Water Decomposition on Transition Metal Surfaces: Structural Dependence and Catalytic Screening, *Catalysis Letters*](http://dx.doi.org/10.1007/s10562-016-1708-7).
 
Notice how much the class size has grown!

<a name='grading'></a>

## Grading ##

* 30% exercises
* 20% write-up
* 20% kinetics
* 30% calculations

<a name='reqs'></a>

## Summary of Requirements ##

At a minimum you should accomplish the following:

1. Complete the [three exercises](../ASE/).
2. Setup a M<sub>13</sub> cluster and a (111) surface and calculate adsorption energies for all intermediates.
3. Calculate transition states for the first step N<sub>2</sub> dissociation) using the fixed bond-length method. Extra credit for calculating the hydrogenation barriers.
4. Vibrational frequency and free energy calculations (initial, transition, and final states, and all adsorbed intermediates). 
5. Analysis
    1. Optimal adsorption sites (relation to transition states)
    2. Kinetic rate analysis
6. Report (3~5 pages maximum)

Email your final report as a PDF document to:

`ctsai89@stanford.edu, aayush@stanford.edu, shaama@stanford.edu, ambarish@stanford.edu`
