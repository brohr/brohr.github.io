---
layout: page
mathjax: true
permalink: /Motivation/
---

## Course Project ##
1. [Introduction](#intro)
<!-- 3. [Calculations](#calcs) -->
<!-- 3. [Analysis](#analysis) -->
<!-- 2. [Report](#report) -->
2. [Research Paper](#paper)
<!-- 4. [Grading](#grading) -->
<!-- 5. [Summary of Requirements](#reqs) -->


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

## OER ##

In the electrochemical oxygen evolution reaction (OER), water is oxidized at the anode to form oxygen gas and proton-electron pairs (protons are released into solution and electrons are conducted away into the anode).

$$
\mathrm 2H_2O \rightarrow {2O_{2\,(aq)} + 4H^+_{(aq)} + 4e^-}
$$

The elementary reaction steps are:

$$
\begin{align}
\mathrm{* + H_2O\rightarrow OH* + H^+ + e^- }\\
\mathrm{OH* \rightarrow O* + H^+ + e^-}\\
\mathrm{O* + H_2O \rightarrow OOH* + H^+ + e^-}\\
\mathrm{OOH* \rightarrow O_{2} + H^+ + e^- + *}\\
\end{align}
$$


A free energy diagram is illustrated below:

<center><img src="../Images/OER_FED.png" alt="OER" style="width: 450px;"/>
<br>Nørskov, Rossmeisl, Logadottir, Lindqvist, Kitchin, Bligaard, Jonsson, J. Phys. Chem. 108, 17886 (2004)</center>

This is a very important reaction because it generates proton-electron pairs, which are required for any electrochemical reduction reaction (e.g. nitrogen reduction to ammonia, carbon dioxide reduction to alcohols or other fuels). OER is the most commonly used anode reaction in aqueous electrochemical cells. Therefore, it is of great interest to make this process as efficient as possible.

The overall reaction is endergonic by 4.92 eV when no external potential is applied (at 0V), so an oxidizing potential of at least 1.23 V (4.92 eV / 4 proton-electron transfers) must be applied to drive the reaction forward. However, the reaction will not procede if any of the individual elementary reaction steps is uphill. The ideal scenario is to have each of the four elementary steps have an equal delta G of 1.23 eV at 0V. In this ideal scenario, the reaction could be driven forward by applying the theoretical minimum 1.23 V. However, in practice, no catalyst has this property. Instead, the third step is usually the most uphill in free energy (most endergonic). For example, if this step is uphill by 1.53 eV, then we must apply a 1.53 V potential to drive this step downhill, and the overall reaction will not occur unless a 1.53 V potential is applied. This is 0.2 V more wasteful than the ideal 1.23 V.

A few definitions:
For overall reactions that are endergonic at 0V, the voltage required to overcome the most endergonic elementary step in the pathway is called the limiting potential. In the above example, the limiting potential is 1.53 V.
The limiting potential minus the theoretical ideal potential is called the overpotential. In this example, the overpotential is 0.3 V (1.53 V - 1.23 V).

[Here](/LimitingPotentialToyExample.xlsx) is a toy example that may help you to understand the concepts.

One solution you may think of is to tune the catalyst so that the binding energies of each intermediate are such that each elementary step has the same free energy. Indeed, changing to a different catalyst will change the binding energy of a surface species (intermediate). Therefore, one can change the delta G of an elementary reaction step by changing catalysts. The trouble is, changing catalysts changes the binding energies of all intermediates, not just one. In other words, changing catalysts can not be used to change the delta G of one elementary reaction independent of the others. There is a very good physical reason for that, and it will be discussed in more detail in CHEMENG 242 in the spring. Looking at the free energy diagram, you can see that in order to achieve the ideal scenario where each elementary step has the same delta G, we need to stabilize OOH* without stabilizing OH* or O*. Changing to a more reactive catalyst will indeed stabilize OOH*, but it will also stabilize O* and OH*.

In this project, you will be working with systems with an unusual geometry, and you will determine whether or not your catalyst stabilizes OOH* relative to OH*.

One way to avoid this problem is to use a reaction mechanism that does not involve the OOH* intermediate. This pathway is called the O-O coupling pathway. The elementary reaction steps are:

$$
\begin{align}
\mathrm{* + H_2O\rightarrow OH* + H^+ + e^- }\\
\mathrm{OH* \rightarrow O* + H^+ + e^-}\\
\mathrm{2O* \rightarrow O_{2} + 2*}\\
\end{align}
$$

where the first and second step occur twice per overall reaction.

The chief problem with this pathway is that the last step has a barrier that is insurmountably high at room temperature. In this project, you will determine whether or not your catalyst provides a sufficiently low barrier for the thrid step.

## ORR ##

The electrochemical oxygen reduction reaction (ORR) suffers from the same problem. In ORR, dissolved oxygen reacts at the cathode with protons from the electrolyte solution and electrons from the cathode via:

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

The O-O coupling pathway is:

$$
\begin{align}
\mathrm{O_{2} + 2* \rightarrow 2O*}\\
\mathrm{O* + H^+ + e^- \rightarrow OH*}\\
\mathrm{OH* + H^+ + e^- \rightarrow * + H_2O}\\
\end{align}
$$

where the second and third step occur twice per overall reaction.


There is a substantial chemical driving force to reduce oxygen to and form water: the reaction above is exergonic by 4.92 eV when no external potential is applied. In ORR, this driving force is harnessed to do useful work - in other words, the chemical driving force works against an oxidizing potential to do work. We would like to apply the largest oxidizing potential possible in order to do the most work, but as the oxidizing potential increases, the reduction of oxygen to water becomes less and less exergonic (less downhill in free energy). If the applied potential is above 1.23 V (4.92 eV / 4 proton electron transfers), then ORR is uphill, and OER would happen instead. Therefore, the theoretical highest voltage that can be applied is again 1.23 V. However, ORR will not procede unless each of the four elementary steps above are downhill in free energy. The theoretical best 1.23 V potential can only be realized if the delta G of each elementary step is identical. Again, this is not the case on any known catalyst.

## Your Project ##

In this project, you will be working with metal organic frameworks. An example of one is shown below.

<br>
<center><img src="/Images/MOF.png" alt="OER" style="width: 450px;"/></center>
<br><br>

In nitrogen reduction research, similar systems have shown a very low N-N transition state energy compared to the N* binding energy. We hope that, by analogy, these systems will have a low O-O transition state energy compared to the O* binding energy.

<br>
<center><img src="/Images/MOF_TS.png" alt="OER" style="width: 450px;"/></center>
<br><br>

Since OER and ORR share all of the same intermediates, after calculating the binding energies of these intermediates on the catalyst that will be assigned to you, you will be able to comment on your catalyst's utility as an OER catalyst or an ORR catalyst. A decreased O-O coupling barrier would be very beneficial to both processes.


<!--

<a name='calcs'></a>

## Calculations ##

Create a `CHE444Project` folder in your `$SCRATCH` directory by running:

```bash
mkdir $SCRATCH/CHE444Project/
```

You may run the exercises in any directory (as long as it is under `$SCRATCH`), but keep all the final files for the project organized.

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

We will provide more details once we get started.

-->

<a name='paper'></a>

## Research Paper ##

We expect that your results will form the basis of a manuscript that we will be putting together as a class. For previous class papers:

* [(2011) Finite-Size Effects in O and CO Adsorption for the Late Transition Metals, *Topics in Catalysis*](http://dx.doi.org/10.1007/s11244-012-9908-x)
* [(2015) Direct Water Decomposition on Transition Metal Surfaces: Structural Dependence and Catalytic Screening, *Catalysis Letters*](http://dx.doi.org/10.1007/s10562-016-1708-7).
 
<!--

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

-->
