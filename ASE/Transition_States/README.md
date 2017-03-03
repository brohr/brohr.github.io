---
layout: page
mathjax: false
permalink: /ASE/Transition_States/
---

Exercise 3: Transition State Energies

In this final exercise, you will be calculating the transition state energy for O-O coupling using the fixed bond length (FBL) method. The  nudged elastic band (NEB) method can more accurately determine the saddle point for the transition state, but it is more computationally intensive and we won't be using it for this course.


## Contents ##
1. [Required Files](#RequiredFiles)
2. [Fixed Bond Length Calculation Information](#fixed-bond-length-calculation)
3. [Analysis](#analysis)
<!-- 3. [Vibrational Frequencies and Free Energies](#vibrational-frequencies) -->
<!-- 4. [Reaction Rate](#reaction-rate) -->
<!-- 5. [Nudged Elastic Band Calculation (Optional)](#nudged-elastic-band-calculation) -->

<a name='RequiredFiles'></a>

### Required Files ###

Obtain the required files by running:

on Sherlock:

```bash
cp -r /scratch/users/brohr/TA_CHE444/Exercise_3_Transition_States .
```

This will create a folder called `Exercise_3_Transition_States`. In that folder, there are two sub-folders: `close`, and `far`.

There are only two calculations that need to be submitted this week: one in the `close` folder and one in the `far` folder. For the "close" geometry:

<br>1) Go into the `close` folder.
<br>2) Copy the final state `qn.traj`file from your O_O calculation from exercise 2 into the Exercise 3 close folder with a command that looks like this:

```bash
cp /scratch/users/brohr/Exercise_2_Adsorption_Energies/1-metal/close/O_O/qn.traj .
```

<br>3) Rename the file fblstart.traj with this command: `mv qn.traj fblstart.traj`.
<br>4) A script called `fbl.py` will already be in the folder. Open the fbl.py script with `vim fbl.py`, scroll to the "your settings here" section, and make sure that the indices `atom1` and `atom2` correspond to the indices of the O atoms in fblstart.traj. You can determine the indices of the O atoms using the ASE GUI. After opening the gui (with `ase-gui fblstart.traj`), simply click each O atom. At the bottom of the ASE GUI window, it will say something like "#32 Oxygen (O)...". In this case, the indices of your O atoms wil likely be 32 and 33.
<br>5) Submit the job with `sbatch --job-name=$(pwd) fbl.py`.
<br>6) Repeat steps 1-5 for the far configuration.


<a name='fixed-bond-length-calculation'></a>

### Fixed Bond Length Calculation Information ###

The fixed bond length (FBL) method is a much faster but cruder way to approximate the minimum energy path and determine the transition state energy. It doesn’t require parallelization over different nodes but may not give you the exact transition state. Generally, one could perform a fixed bond length calculation first and determine if a transition state was found (by checking the vibrational modes). If the transition state is poorly described, then a NEB calculation can be performed based on the fixed bond length results as the inputs.

In a FBL calculation, you provide an initial state, then, you iteratively decrease the distance between the two atoms and optimize the geometry of the entire structure while keeping the bond length fixed. This will approximate the minimum energy pathway (MEP) between the initial and final states. Since we are iteratively decreasing the distance, our input in this case would correspond to the *initial* state in our O-O coupling reaction. We then fix the bond length between the two O\* atoms that are required to come together and form a bond. We are thus determining the minimum energy pathway for the reaction: 2O\* → O<sub>2</sub> + 2\*. Follow the `fbl.py` script to determine the transition state for the O-O coupling reaction on your metal. The script requires an initial state and a specification of the two atoms whose distance is to be fixed (the two O* atoms).

```python
atom1=32
atom2=33
```
This specifies the atoms that will be forced to be pulled together. Then for each fixed O-O distance, a structural relaxation is performed.


The fixed bond length calculation can continue beyond the final state and start to give you structures with unrealistically high energies. These would not be relevant for the reaction path and you should select only the trajectory images you want to view. To do this, you can combine the `.traj` files from each step first,

```bash
ase-gui i?.traj i??.traj -n -1 -o combined.traj
```

where `ase-gui` will read all files of the form `i?.traj` followed by `i??.traj`, combine their final steps (using `-n -1`), and then write out a combined file called `combined.traj`.

Then, to select the range of images within `combined.traj`, you can use the @ symbol followed by a range, such as:

```bash
ase-gui combined.traj@:22
```

which will display images 1 through 22 within the combined trajectory file. The image range follows Python syntax. Choose the range where you get a clear view of the initial state, transition state, and final state. You can use `ase-gui` to view the reaction coordinate:

<center><img src="Images/NEB-menu.png" alt="NEB menu" style="width: 400px;"/>
<br>Tools > NEB</center>

Even though the `NEB` menu option was intended for viewing NEB trajectories, it will work for any transition state calculation. You should see a plot that looks like this:

<center><img src="Images/FBL-plot.png" alt="Reaction coordinate" style="width: 400px;"/><br>
Reaction Coordinate</center>

You should only pay attention to the peak of the plot, which is where the transition state is. The FBL calculation will not necessarily find the final state (in this case, gaseous O<sub>2</sub>), so the final state energy in the plot will not generally be correct.

If the bond length becomes unrealistically short, you will also see large spikes in the total energy towards the end of your calculation. You can safely ignore these images.

Note that on very reactive surfaces, the FBL calculation may not be able to find the desorbed O<sub>2</sub>\* state before the bond length becomes unrealistically short (the adsorption of O<sub>2</sub>\* is extremely favorable). This is okay because we would expect the relevant transition state to occur AFTER the adsorption step for such a reactive surface. You can therefore use the transition state calculation as normal in this case even though FBL does not recover the gaseous O<sub>2</sub> state.


<a name='analysis'></a>

### Analysis ###

When the calculation is finished, an empty file named `done` will appear in the folder that the job ran in. Use the NEB tool (as pictured above) to find the peak of the energy path, disregarding any spike in energy that occurs at the end. Determine which point is the peak (the first point on the NEB plot is point #0, the second is point #1, etc). Then, view the contents of the `PES.dat` file with the command `cat PES.dat`. The energy of the transition state is the number in the second column of the line corresponding to your transition state. For example if the 14th point is your transition state, then the transition state energy is the number in the second column of the 14th line. You can check to make sure you're looking at the correct line by making sure that the energy in the line above and the energy in the line below are both lower (more negative) than the energy of the line that you think is the transition state. In other words, make sure you are actually selecting the local maximum energy. Let's call this energy $$\mathrm{E_{TS, raw}}$$.

Now, the transition state energy relative to our gas phase references, $$\mathrm{E_{TS}}$$ is given by:

$$\mathrm{E_{TS} = E_{TS, raw} - 2*E_{O,reference}} $$

Use the same gas phase reference energy as in exercise 2.

Finally, the activation is given by:

$$\mathrm{E_{a} = E_{TS} - E_{initial}} $$

where $$\mathrm{E_{initial}}$$ is the adsorption energy of the O_O system from exercise 2.

Calculate the activation energy for the O-O coupling reaction for the close and the far configuration.


<!--
<a name='vibrational-frequencies'></a>

### Vibrational Frequencies and Free Energies ###

Calculate the vibrational frequencies for transition state and the final state using the [`run_freq.py`](run_freq.py) script. See the [ASE page](https://wiki.fysik.dtu.dk/ase/ase/thermochemistry/thermochemistry.html) for a detailed explanation of how this is implemented. Use `ase-gui` to view the vibrational modes, which are written out as `vib*.traj` files. There should be \\(3N\\) vibrational modes for all adsorbed states, and \\(3N - 1\\) vibrational modes for the transition state.

Since the fixed bond-length method cannot guarantee a solution at the saddle point (the image with the highest energy might not be exactly at the transition state), you may encounter multiple imaginary modes when you calculate the vibrational frequencies. In this case, the following scheme can be used:

* Any adsorbate should have \\(3N\\) non-imaginary vibrational modes
* Transition states should have \\(3N – 1\\) non-imaginary vibrational modes and one imaginary mode
* If there are extra imaginary modes replace < 7 meV with 7 meV

For the gas phase species, we will provide the results for N<sub>2</sub>, H<sub>2</sub>, and NH<sub>3</sub> for you. You calculated the results for N<sub>2</sub> in the last exercise and you can use either your own results or the one provided. These are in the `Gaseous_Molecules\` folder.

**<font color="red">Requirement:</font>** Calculate the vibrational frequencies for:

* Initial state: 2N\*
* Transition state: N-N\*
* Intermediates: N\*, NH\*, NH<sub>2</sub>\*, NH<sub>3</sub>\*,H\*

<a name='reaction-rate'></a>

### Reaction Rate ###

Using the vibrational frequencies associated with initial, final and transition states, evaluate the rate constant of N<sub>2</sub> dissociation on the metal surface and cluster based on harmonic transition state theory (hTST). In addition, evaluate the rate constant based on the Arrhenius relationship by using the free energy associated with the initial, final, and transition states that were directly calculated from ASE Quantum Espresso. For the free energies, use the [`get_ads_free_energy.py`](get_ads_free_energy.py) script for adsorbed states and [`get_gas_free_energy.py`](get_gas_free_energy.py) for N<sub>2</sub>.

To calculate the rate of N<sub>2</sub> dissociation, one can assume that the first step is rate determining and all other steps are in equilibrium. This leads to two reactions:

$$
\begin{align}
&(1) \quad &\mathrm{N_2} + 2* \rightarrow \mathrm{2N}*\\
&(2) \quad &\mathrm{N}* + \frac{3}{2}\mathrm{H}_{2\,\mathrm{(g)}} \leftrightarrow \mathrm{NH}_{3\,\mathrm{(g)}} + *
\end{align}
$$

The rate constant and equilibrium constants are:

$$
k_1 = \frac{k_\mathrm{B} T}{h} \exp\left(-\frac{\Delta G_\mathrm{TS,1}}{k_\mathrm{b}T}\right)
$$

For the entire reaction:

$$
K_\mathrm{eq} = \exp\left(-\frac{\Delta G_\mathrm{rxn}}{k_\mathrm{B}T}\right)
$$

and for *only* the second reaction:

$$
K_\mathrm{2} = \exp\left(-\frac{\Delta G_\mathrm{rxn,2}}{k_\mathrm{B}T}\right)
$$

The reaction rate (turnover frequency, TOF) for the entire reaction is then given in a simply analytical form:

$$
\mathrm{TOF} = k_1 P_\mathrm{N_2} \left(1+\frac{P_\mathrm{NH_3}}{K_2 P_\mathrm{H_2}^{1.5}}\right)^{-2}\left(1 - \frac{P_\mathrm{NH_3}^2}{K_\mathrm{eq}P_\mathrm{H_2}^3 P_\mathrm{N_2}}\right)
$$


**<font color="red">Requirement:</font>** 

* Plot the log of the rate constant versus the inverse temperature.


<a name='nudged-elastic-band-calculation'></a>

### Nudged Elastic Band Calculation (Optional) ###

To perform a nudged elastic band (NEB) calculation, one needs to provide an initial and final state trajectory. A series of "images" between the initial and final states will then be used to determine the minimum energy path. This band of images will be relaxed. For a NEB calculation, you only need to provide the initial and final state and the number of images in between. Go through the [`neb.py`](neb.py) script. Typically 5~7 images between the initial and final states will be sufficient. Intermediate images will be generated using a linear interpretation of the initial and final trajectory. An odd number of images should be chosen so that the one image will be at the transition state. NEB calculations can take a long time, and the [`neb_restart.py`](neb_restart.py) should be used to read in the previous images. You can also start at with k-points to speed up the calculation, and then restart the calculation with higher k-points.

In the `neb.py` file make sure the line specifying the number of nodes.

```python
#SBATCH [two hyphens]nodes=5
```
corresponds to the number of _intermediate_ images. Check that `intermediate_images = 5` matches. 

Both `neb.py` and `neb_restart.py` scripts require the initial and final states of the reaction path to be provided. This is specified in the lines:

```python
initial = io.read('neb0.traj')
final = io.read('neb6.traj')
```

make sure that the trajectory files are in the directory and are named in the same manner. For the `neb_restart.py` script, the initial and final trajectories must be named in the `neb*.traj` format, where `*` is a number. The script will read in all intermediate images based on the number in the initial and final trajectory.

To view all the trajectory files, run the following command

```bash
ase-gui neb*.traj –n -1
```

where all files of the form `neb*.traj` (with * referring to any number of characters) will be opened in ag. The `-n` flag specifies the image within each trajectory file. Since you are optimizing the entire reaction path, each step in the NEB will be stored in each image file. Specifying `-n -1` tells ase-gui to only read the last image of each file (i.e. the most current step).

-->