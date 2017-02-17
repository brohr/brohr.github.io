---
layout: page
mathjax: false 
permalink: /ASE/Formation_Energies/
---

## Contents ##

1. [Lattice Constant Determination and Bulk Energy Calculation](#lattice-constant-determination)
2. MOF Energy Calculation
<br>2a. [One Metal](#MOF1)
<br>2b. [Two Metals](#MOF2)
4. [Analysis](#analysis)

<a name='intro'></a>

### Introduction ###

In this exercise, you will be running all of the calculations necessary to calculate the formation energies of the metal organic frameworks we'll be working with for this project.

The formation energy is very simply the reaction energy of the following reaction:

<center>Empty Organic Framework + Bulk Metal $$\mathrm{\rightarrow}$$ Metal Organic Framework</center>

So, you will calculate all three of those energies this week using density functional theory (DFT).

### Required Files ###

Obtain the required files by running:

on Sherlock:

```bash
cd $SCRATCH
cp -r /scratch/users/brohr/TA_CHE444/Exercise_1_Formation_Energies .
```

This should create a folder called `Exercise_1_Formation_Energies/` containing subfolders with all the starter scripts you will need. By default, The output for your calculations will be written into the folder from where you submitted the script. To perform new calculations, you will generally be copying the scripts from these tutorials into new folders, modifying them, and submitting them.



<a name='lattice-constant-determination'></a>

#### Lattice Constant Determination and Bulk Metal Energies ####

Some of you are assigned one metal, and others are assigned two metals.
For each metal, do the following steps:

<br>1) You will need to use some basic UNIX commands for this. You can use <a href="https://brohr.github.io/UNIX/">our notes</a> as a reference or find other resources online if you prefer.
<br>2) Go to the `Exercise_1_Formation_Energies/Bulk/` folder.
<br>3) Make a directory for the lattice optimization calculation for the metal.
<br>4) Look up the <a href="http://periodictable.com/Properties/A/LatticeConstants.html">crystal structure and lattice constant(s)</a> of your material. BCC and FCC structures have only one lattice constant. HCP structures have two.
<br>5) Copy the lattice optimization script associated with your crystal structure into the directory you created.
<br>6) Edit the first two lines of the script - edit the name of the metal to be the name of your metal, and use the lattice constant(s) you looked up as the initial guess. You can <a href="https://brohr.github.io/UNIX/#text-editors">edit the file using the vim text editor</a>.
<br>7) Submit the lattice optimization script to the supercomputer cluster by running

```bash
sbatch --job-name=$PWD lattice_opt_CRYSTALSTRUCTURE.py
```
Here, `--job-name=$PWD` sets the current working directory as the job name.

If you need to cancel a job, see the instructions <a href="https://brohr.github.io/UNIX/#submitting-jobs">here</a>.



<a name='MOF1'></a>

### Metal Organic Framework (One Metal) ###

Go to the `Exercise_1_Formation_Energies/MOF/` folder.
You will be calculating the energy for:
<br>1) The empty organic framework
<br>2) The organic framework with one metal atom in the ring
<br>3) The organic framework with two metal atoms in the ring
<br>4) The organic framework with three metal atom  in the ring

For each calculation:
<br>1) Make a new directory for the calculation. Things get very confusing at best and impossible to detangle at worst if multiple calculations are run in the same directory.
<br>2) Copy `opt.py` into the directory
<br>3) Copy `empty_organic_framework.traj` into the directory.
<br>4) Go into the directory, and use the `ase-gui` to edit the .traj file. Press `ctrl + S` to save your edits to the .traj file.
<br>5) Rename the .traj file `init.traj`.
<br>6) Submit `opt.py` to the supercomputer cluster.

Note: for the empty organic framework, you don't need to edit the .traj file at all (skip steps 4-5).
For the others, your init.traj files should look something like this (but with your metal):

<center><img src="/ASE/Formation_Energies/Images/1-atom.png"/>
<br>
<br><img src="/ASE/Formation_Energies/Images/2-atom.png"/>
<br>
<br><img src="/ASE/Formation_Energies/Images/3-atom.png"/>
</center>

<a name='MOF2'></a>

### Metal Organic Framework (Two Metals) ###

Go to the `Exercise_1_Formation_Energies/MOF/` folder.
You will be calculating the energy for:
<br>1) The empty organic framework
<br>2) The organic framework with two metal atoms in the ring
<br>3) The organic framework with three (metal A, metal A, metal B) metal atoms in the ring
<br>4) The organic framework with three (metal A, metal B, metal B) metal atom  in the ring

For each calculation:
<br>1) Make a new directory for the calculation. Things get very confusing at best and impossible to detangle at worst if multiple calculations are run in the same directory.
<br>2) Copy `opt.py` into the directory
<br>3) Copy `empty_organic_framework.traj` into the directory.
<br>4) Go into the directory, and use the `ase-gui` to edit the .traj file. Press `ctrl + S` to save your edits to the .traj file.
<br>5) Rename the .traj file `init.traj`.
<br>6) Submit `opt.py` to the supercomputer cluster.

Note: for the empty organic framework, you don't need to edit the .traj file at all (skip steps 4-5).
For the others, your init.traj files should look something like this (but with your metal):

<center><img src="/ASE/Formation_Energies/Images/AB.png"/>
<br>
<br><img src="/ASE/Formation_Energies/Images/AAB.png"/>
<br>
<br><img src="/ASE/Formation_Energies/Images/ABB.png"/>
</center>


In case you didn't specify a long enough wall-time, the calculation can be continued by using your current output. Simply submit the script again, and it will pick up where it left off.

<a name='analysis'></a>

### Analysis ###

When the lattice optimization calculation is finished, the energy of the system is the last number in the first column of `lattice_opt.log`. You can view that energy by navigating to the directory that the calculation was run in and typing `cat lattice_opt.log` The energy per atom of the bulk metal ($$\mathrm{E_{Bulk Metal}}$$) is simply this total energy divided by the number of atoms in the unit cell.

When the optimization calculations are finished, the energy of each system will be stored in a file called `out.energy` in the directory that the calculation was run in. You can view that energy by navigating to the directory that the calculation was run in and typing `cat out.energy` 

The formation energy is given by:

$$\mathrm{E_{MOF} - E_{Empty Framework} - N*E_{Bulk Metal}}$$

where N is the number of metal atoms in the MOF.
