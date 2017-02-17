---
layout: page
mathjax: false 
permalink: /ASE/Formation_Energies/
---

## Getting Started ##

In the first exercise, we will be looking at bulk metals and how to determine lattice constants, then we will be setting up metal surfaces and metal clusters. The example scripts use Pt by default. You should change this to the system you have been assigned for the project so you can start making progress.

## Contents ##

1. [Introduction](#intro)
2. [A Typical ASE Script](#a-typical-ase-script)
3. [Lattice Constant Determination and Bulk Energy Calculation](#lattice-constant-determination)
4. MOF Energy Calculation
<br>4a. [One Metal](#MOF1)
<br>4b. [Two Metals](#MOF2)
4. [Restarting Calculations](#restarting)
5. [Analysis](#analysis)

<a name='intro'></a>

### Introduction ###

In this exercise, you will be running all of the calculations necessary to calculate the formation energies of the metal organic frameworks we'll be working with for this project.

The formation energy is very simply the reaction energy of the following reaction:

$$\mathrm{Empty Organic Framework + Bulk Metal \rightarrow Metal Organic Framework }$$

So, you will calculate all three of those energies this week using density functional theory (DFT).

### Required Files ###

Obtain the required files by running:

on Sherlock:

```bash
cd $SCRATCH
cp -r /scratch/users/brohr/TA_CHE444/Exercise_1_Formation_Energies .
```

This should create a folder called `Exercise_1_Formation_Energies/` containing subfolders with all the starter scripts you will need. By default, The output for your calculations will be written into the folder from where you submitted the script. To perform new calculations, you will generally be copying the scripts from these tutorials into new folders, modifying them, and submitting them.

<a name='a-typical-ase-script'></a>

### A Typical ASE Script ###

ASE scripts can be run directly in the terminal (in the login node) or submitting to external nodes. Generally, you will be submitting jobs to external nodes and only small scripts will be run on the login node. By default, all output from any submitted script will be written *from the directory where the submission command was executed*, so make sure you are inside the calculation folder before running the submission command.

Let's look at how a typical ASE script is written. Open the [`run_surf.py`](run_surf.py) script in the `Surface` folder. We will be showing the Sherlock versions of the script for brevity, but the CEES versions are analogous.

```bash
vi opt.py
```

The first line,

```python
#!/usr/bin/env /home/vossj/suncat/bin/python
```

will ensure that the version of Python that is being used is the one that has all the software from SUNCAT installed.

Next, notice the comments in the beginning. These lines will be ignored by Python, but will be read by the job submission system. These include information such as how much time to allocate, the number of nodes required, what the names of the output and error files are, what the name of the job should be, and what your email is. Most of the settings will be the same regardless of the job you submit. You will mostly just be changing the amount of allocated time and the number of nodes, for jobs that require parallelization (not required for this project).

```python
#above line selects special python interpreter needed to run espresso
#SBATCH -p iric
#################
#set a job name
#you can also use --job-name=$PWD when submitting
#SBATCH --job-name=myjob
#################
#a file for job output, you can check job progress
#SBATCH --output=myjob.out
#################
# a file for errors from the job
#SBATCH --error=myjob.err
#################
#time you think you need; default is 20 hours
#SBATCH --time=48:00:00
#################
#number of nodes you are requesting
#SBATCH --nodes=1
#################
#SBATCH --mem-per-cpu=4000
#################
#get emailed about job BEGIN, END, and FAIL
#SBATCH --mail-type=ALL
#################
#who to send email to; please change to your email
#SBATCH  --mail-user=YOUR_SUNETID@stanford.edu
#################
#task to run per node; each node has 16 cores
#SBATCH --ntasks-per-node=16
```

To change the allocated time for the jobs, modify (MM:SS):

```python
#SBATCH --time=1200:00
```

Next, we import all the relevant ASE modules in for this calculation

```python
from ase import *
from ase.lattice.surface import *
from ase.optimize import *
from ase.constraints import *
from espresso import espresso
```

The asterisks `*` indicates that all methods and classes should be imported. You can also specify the ones you need. `from ase import *` imports all the basic functionality in ase, `from ase.lattice.surface import *` import methods and classes related to solid surfaces, `from ase.optimize import *` imports the optimization methods, `from ase.constraints import *` imports the constraint methods, and most importantly `from espresso import espresso` import the Quantum ESPRESSO calculator for the ASE interface.

We read in a .traj file, which specifies the location of the nuclei and unit cell boundaries.

```python
try:
    atoms = read('qn.traj')
except:
    try:
        atoms = read('qn.traj.bak')
  os.system('cp qn.traj.bak qn.traj')
    except:
        atoms = read('init.traj')
```
which we can easily modify and use for naming output files.

An existing trajectory can be read in:

```python
# read in the slab
slab = io.read('slab.traj')
```


Then, the Quantum ESPRESSO calculator is set up. All parameters related to the electronic structure calculation are included here. The following example shows typical parameters that we use in the group for metal calculations. Typically, the number of k-points is determined using 24 Å per lattice vector for transition metals.

```python
#espresso calculator setup
calc = espresso(pw=600,             #plane-wave cutoff
                dw=6000,        #density cutoff
                xc='BEEF',    #exchange-correlation functional
                kpts=(6,6,1),       #k-point sampling;
                nbands=-60,         #20 extra bands besides the bands needed to hold
                          #the valence electrons
                smearing='gaussian',
                sigma=0.1,
                convergence= {'energy':1e-5,
                         'mixing':0.1,
                         'nmix':10,
                         'mix':4,
                         'maxsteps':500,
                         'diag':'david',
                                   'mixing_mode':'local-TF'
                          },  #convergence parameters
                dipole={'status':True}, #dipole correction to account for periodicity in z
                spinpol=False,
                outdir='calcdir',
                output = {'removesave':True},
                psppath = '/scratch/users/aayush/psp/gbrv/')  #new vanderbilt psp's
```


Finally, the Quantum ESPRESSO calculator is attached to the `slab` Atoms object, and the optimizer is defined.

```python
atoms.set_calculator(calc)
```

To perform structural optimizations, an optimizer needs to be defined. We will be using the BFGS Line Search, which is implemented in `QuasiNewton`. For more details about optimizations in ASE, look at [this page](https://wiki.fysik.dtu.dk/ase/ase/optimize.html). `QuasiNewton()` is an object for the [structural optimization](https://wiki.fysik.dtu.dk/ase/ase/optimize.html), which takes an Atoms object as an input. A convergence criteria is set and `qn.run()` initiates the optimization.

```python
qn = QuasiNewton(atoms, trajectory='qn.traj', logfile='qn.log')
qn.run(fmax=0.05)                              #until max force<=0.05 eV/AA
```

The `logfile=` argument is optional. If it's not specified, then the output will be written to the system output, e.g. `myjob.out`. If it is specified, then the output for the optimization will be written to `name+'.log'`, e.g. `Pt111.log`. You should see the following results:

```bash
BFGSLineSearch:   0[  0]  12:50:46   -28144.460970       1.7496
BFGSLineSearch:   1[  1]  12:59:45   -28145.528706       0.4792
BFGSLineSearch:   2[  2]  13:07:35   -28145.571393       0.3625
BFGSLineSearch:   3[  3]  13:14:49   -28145.600615       0.1408
BFGSLineSearch:   4[  4]  13:21:30   -28145.608312       0.0994
BFGSLineSearch:   5[  5]  13:27:02   -28145.610934       0.0540
BFGSLineSearch:   6[  6]  13:31:28   -28145.611948       0.0245
```

The column names for the results are:

```bash
optimizer         step    time       total energy (eV)   forces (eV/Å)
```

The final energy output is also stored in a file called `out.energy`. You can see its contents by typing the bash command:

```bash
cat out.energy
```

The trajectory for all optimization steps are stored in `'qn.traj'`. You can read the output trajectory using `ase-gui qn.traj` and view all steps. The final energy is also stored in the `.traj` file and can be retrieved by reading in the `.traj` file. Using Python in interactive mode (i.e., by running `python`):

```python
>>> from ase.io import read
>>> atoms = read('output.traj')
>>> atoms.get_potential_energy()
-28436.147150825487
```


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
sbatch --job-name=$PWD bulk_metal.py
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



<a name='restarting'></a>

### Restarting Calculations ###

In case you didn't specify a long enough wall-time, the calculation can be continued by using your current output. Simply submit the script again, and it will pick up where it left off.

<a name='analysis'></a>

### Analysis ###

When the lattice optimization calculation is finished, the energy of the system is the last number in the first column of `lattice_opt.log`. You can view that energy by navigating to the directory that the calculation was run in and typing `cat lattice_opt.log` The energy per atom of the bulk metal ($$\mathrm{E_{Bulk Metal}}$$) is simply this total energy divided by the number of atoms in the unit cell.

When the optimization calculations are finished, the energy of each system will be stored in a file called `out.energy` in the directory that the calculation was run in. You can view that energy by navigating to the directory that the calculation was run in and typing `cat out.energy` 

The formation energy is given by:

$$\mathrm{E_{MOF} - E_{Empty Framework} - N*E_{Bulk Metal}}$$

where N is the number of metal atoms in the MOF.
