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


For the course project, you will be studying thermo-chemical ammonia synthesis on metallic clusters and surfaces. Each student will be assigned one metal or bimetallic alloy. The due date is <font color="red">3/17 at 11:59 PM (hard deadline)</font>.

Turn in your final report by emailing a PDF file to all TA's:

```
brohr@stanford.edu, aayush@stanford.edu
```

<a name='intro'></a>

## Introduction ##

In the electrochemical oxygen reduction reactio (ORR), dissolved oxygen reacts at the cathode with protons from the electrolyte solution and electrons from the cathode via:

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

<center><img src="../Images/N2_path.jpg" alt="N2 path" style="width: 450px;"/>
<br>ORR Free Energy Diagram (<a href="http://dx.doi.org/10.1126/science.1106435">Honkala et. al. (2005)</a>)</center>

There is a substantial chemical driving force to reduce oxygen to and form water. In ORR, this driving force is harnessed to do useful work - in other words, the chemical driving force works against an oxidizing potential to do work. We would like to apply the largest oxidizing potential possible, but as the oxidizing potential increases, the reduction of oxygen to water becomes less and less downhill in free energy. If the applied potential is above 0.625 V (2.5eV / 4 proton electron transfers), then ORR is uphill, and water would split into oxygen and proton electron pairs instead. Therefore, the theoretical highest voltage that can be applied is about 0.6 V. However, ORR will not procede unless each of the four elementary steps above are downhill in free energy. The theoretical best 0.6 V potential can only be realized if the delta G of each elementary step is identical. In practice, this is not the case on any known catalyst. Changing to a different catalyst will change the binding energy of each surface species (all of the intermediates). Therefore, one can change the delta G of each elementary reaction step. The trouble is, when one binding energy changes, they all change. Looking at the free energy diagram, you can see that in order to achieve the ideal scenario where each elementary step has the same delta G, we need to stabilize OOH* without stabilizing OH* or O*. The "volcano plot" below shows the maximum potential that can be applied (the limiting potential) as a function of the OOH* and OH* binding energies. You can see that all known catalysts fall on a "scaling line." There is a very goof physical reason for that, and it will be discussed in more detail in CHEMENG 242 in the spring.

In this project, we will be working with reactive metals doped into graphene. The hypothesis is that if the metal atoms have a small space between them, OOH* will be able to form a favorable bond with both metal atoms, while OH, which is smaller, will not.

<center><img src="../Images/N2_volcano.png" alt="N2 volcano" style="width: 400px;"/>
<br>Filled contour plot for the turnover frequencies (Singh et. al. (2016))</center>

Another way to avoid the problem is to run the reaction by a pathway that does not include OOH. If O2 adsorbs onto the surface and thermally overcomes the barrier to split into 2O*, then the rest of the ORR pathway could be run, circumventing OOH* entirely. On all known good ORR catalysts, the barrier for this process is insurmountably high. However, the barrier may be lower on the systems we'll be working with. You will analyze this pathway as well in your report.

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
