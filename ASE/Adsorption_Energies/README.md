---
layout: page
mathjax: false 
permalink: /ASE/Adsorption_Energies/
---

## Contents ##

1. Adsorbate Energy Calculations
<br>1a. [One Metal](#MOF1)
<br>1b. [Two Metals](#MOF2)
2. [Gas Phase Calculations](#gasphase)
2. [Analysis](#analysis)

<a name='intro'></a>

### Introduction ###

In this exercise, you will be running all of the calculations necessary to calculate the binding energy of the OER/ORR adsorbates on your catalysts.

### Required Files ###

Obtain the required files by running:

on Sherlock:

```bash
cd $SCRATCH
cp -r /scratch/users/brohr/TA_CHE444/Exercise_2_Adorption_Energies .
```

This should create a folder called `Exercise_2_Adsoprtion_Energies/` containing subfolders with all the starter scripts you will need. 

<a name='MOF1'></a>

### Adsorbate Energy Calculations (One Metal) ###

Go to the `Exercise_2_Adsorption_Energies/` folder. You can ignore the 2-metal folder. Simply go into the 1-metal folder `cd 1-metal`.
For each configuration (close, far, and monomer), you will be determining the energy of each intermediate and each combination of intermediates on your catalyst.

Start by going to the monomer folder. There is already an empty folder created for each calculation you will need to do.

For each calculation:
<br>1) Go into the folder for your calculation
<br>2) Copy the opimization script into the folder:
```bash
cp /scratch/users/brohr/TA_CHE444/Exercise_2_Adsorption_Energies/opt.py .
```
<br>3) Copy your clean (meaning empty, no adsorbate bonded) catalyst from the execise 1 folder into the current folder with a command that looks like this:
```bash
cp /scratch/users/brohr/TA_CHE444/Exercise_1_Formation_Energies/Graphene/monomer/YOURMETAL/clean/qn.traj ./init.traj
```.
If you type `ls` or `ll`, you should see 2 files: `init.traj` and `opt.py`. 
<br>4) Use the ASE gui (`ase-gui init.traj`) to add the adsorbate to the catalyst. The adsorbate you add should, of course, correspond to the name of the folder you are working in
<br>6) Submit `opt.py` to the supercomputer cluster with `sbatch --job-name=$(pwd) opt.py`.

Repeat these steps for each adsorbate folder in the monomer folder. Below are images showing what initial guesses for OOH and OOH-horizontal should look like.

<center><img src="/ASE/Adsorption_Energies/Images/OOH.png"/>
<br>
<br><img src="/ASE/Adsorption_Energies/Images/OOH-horizontal.png"/>
</center>

Repeat the above procedure for the folders within the `Exercise_2_Adsorption_Energies/1-metal/close` and `Exercise_2_Adsorption_Energies/1-metal/far` folders. You will see folders with names like `OH_O`. `OH_O` is meant to describe an OH on your first metal atom and an O on your second metal atom.

Below are images showing what initial guesses for OOH_OH and OOH-bidentate should look like.

<center><img src="/ASE/Adsorption_Energies/Images/OOH_OH.png"/>
<br>
<br><img src="/ASE/Adsorption_Energies/Images/OOH-bidentate.png"/>
</center>

<a name='MOF2'></a>

### Adsorbate Energy Calculations (Two Metals) ###

Go to the `Exercise_2_Adsorption_Energies/` folder. You can ignore the 1-metal folder. Simply go into the 2-metal folder `cd 2-metal`.
For each configuration (close and far), you will be determining the energy of each intermediate and each combination of intermediates on your catalyst.

Start by going into either the `close` folder or the `far` folder. Within those folders, there is already an empty folder created for each calculation you will need to do. `OH_O` is meant to describe an OH on your first metal atom and an O on your second metal atom. Since you have two metal atoms of different types, `OH_O` is different than `O_OH`.

For each calculation:
<br>1) Go into the folder for your calculation
<br>2) Copy the opimization script into that folder:
```bash
cp /scratch/users/brohr/TA_CHE444/Exercise_2_Adsorption_Energies/opt.py .
```

<br>3) Copy your clean (meaning empty, no adsorbate bonded) catalyst from the execise 1 folder into the current folder with a command that looks like this:
```bash
cp /scratch/users/brohr/TA_CHE444/Exercise_1_Formation_Energies/Graphene/monomer/YOURMETAL/clean/qn.traj ./init.traj
```.
If you type `ls` or `ll`, you should see 2 files: `init.traj` and `opt.py`. 
<br>4) Use the ASE gui (`ase-gui init.traj`) to add the adsorbate to the catalyst. The adsorbate you add should, of course, correspond to the name of the folder you are working in
<br>6) Submit `opt.py` to the supercomputer cluster with `sbatch --job-name=$(pwd) opt.py`.


Repeat the above procedure for the folders within the `Exercise_2_Adsorption_Energies/2-metal/close` and `Exercise_2_Adsorption_Energies/2-metal/far` folders.

Below are images showing what initial guesses for OOH_OH and OOH-bidentate should look like. Unlike the images below, you will, of course, have two metal atoms of different types.

<center><img src="/ASE/Adsorption_Energies/Images/OOH_OH.png"/>
<br>
<br><img src="/ASE/Adsorption_Energies/Images/OOH-bidentate.png"/>
</center>


<a name='gasphase'></a>

#### Gas Phase Calculations ####

The gas phase calculations are done for you. The results are in the `Exercise_2_Adsorption_Energies/gas-phase` folder. The electronic energy is stored in the file called `e_energy.out`.

<a name='analysis'></a>

### Analysis ###

When the optimization calculations are finished, the energy of each system will be stored in a file called `out.energy` in the directory that the calculation was run in. You can view that energy by navigating to the directory that the calculation was run in and typing `cat out.energy`

The electronic reference energy for O and H are given by:

$$\mathrm{E_{O,reference} - E_{H_2O} - E_{H_2}}$$

$$\mathrm{E_{H,reference} - E_{H_2}/2}$$

The adsorption energy is given by:

$$\mathrm{E_{catalyst-with-adsorbate} - E_{clean_catalyst} - N_O*E_{O,reference} - N_H*E_{H,reference}$$

where $$\mathrm{N_O}$$ is the number of O atoms in the adsorbate, and $$\mathrm{N_O}$$ is the number of H atoms in the adsorbate.
