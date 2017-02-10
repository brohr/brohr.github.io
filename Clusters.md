---
layout: page
mathjax: false 
permalink: /Clusters/
---

# Getting Started
1. [Logging Into the Computing Clusters](../Clusters/)
2. [Basic UNIX](../UNIX/)
3. [Python](../Python/)

____

## Logging Into the Computing Clusters

Half of the class have been assigned computing accounts on Sherlock and the other half have been assigned accounts on CEES. Follow the instructions for using the one you have been assigned to and follow the tests to make sure everything is set up properly and functional.

## Contents
1. [Installation](#installation)
2. [Logging On](#logging)
3. [First Time Logging On](#first-time)
    1. [Sherlock](#first-time-sherlock)
    2. [CEES](#first-time-cees)
4. [Making Sure Everything Works](#testing)

<a name='installation'></a>

## Installation

### Mac OSX
Download and install:

* [XQuartz](http://www.xquartz.org/)

To prevent X11 from timing out, open the terminal and type:

```bash
mkdir -p ~/.ssh
echo $'\nHost *\n ForwardX11Timeout 1000000\n' >>~/.ssh/config
```


### Windows

Download and install:

* [PuTTY](http://www.putty.org/)
* [Kerberos](http://uit.stanford.edu/service/kerberos) (needed for Sherlock only)
* [Xming](http://sourceforge.net/projects/xming/) (Note: disable automatic installation of PuTTY with Xming. The above installer is a newer version)


### Linux (Debian-based, e.g. Ubuntu)
From the terminal (needed for Sherlock only):

```bash
sudo apt-get install krb5-user
```

____

<a name='logging'></a>

## Logging onto the Clusters

For the [**Sherlock**](http://sherlock.stanford.edu) cluster, make sure to read through the login instructions [here](http://sherlock.stanford.edu/mediawiki/index.php/LogonCluster).

Login with your SUNetID and password.

Follow the instructions below for your system:

### Mac OSX

For **Sherlock** only, authenticate using Kerberos:

```bash
kinit sunetid@stanford.edu
```

Then,

```bash
ssh -K -X sunetid@sherlock.stanford.edu
```

to log onto Sherlock, where ```sunetid``` is your Stanford SUNET ID. ```kinit``` does not need to be rerun unless the Kererbos ticket is expired. On Mac OSX you can type ```klist``` to check the status of the ticket.

### Windows

Open Kerberos and authenticate using your SUNetID and password. Alternatively, ppen a “Command Prompt” (open a new one, if you have just installed Kerberos) and run:

```bash
kinit -5 sunetid@stanford.edu
```

Next, launch Xming. You will always need to have this open in order to forward graphical windows from the external clusters.

Start PuTTY, and:

* “Session” → “Host Name” `sunetid@sherlock.stanford.edu`.
* “Connection” → “SSH” → “X11” check “Enable X11 forwarding”
* Back in “Session”, you can **save these settings for next time**.

You can start putty several times, if you need several terminal windows; only one instance of kinit and Xming needed.


### Linux ###

In a terminal (Sherlock only):

```bash
kinit sunetid@stanford.edu
```

Then:

```bash
ssh -X sunetid@sherlock.stanford.edu
```

Open new terminals to run ssh again if you need several terminals on sherlock;
`kinit` only needs to be run once per boot (or as long as the Kerberos ticket remains valid). Type `klist` to check the ticket's status.

____

<a name='first-time'></a>

### First Time Logging in ###

<a name='first-time-sherlock'></a>

For the **first login** only, run the following command:

```bash
echo $'\nexport PATH=/home/vossj/suncat/bin:$PATH' >>~/.bashrc
echo 'export LD_LIBRARY_PATH=/home/vossj/suncat/lib:/home/vossj/suncat/lib64:$LD_LIBRARY_PATH' >>~/.bashrc
source ~/.bashrc
```

This will enable you to run SUNCAT specific software on the Sherlock cluster, including the ASE interface to Quantum ESPRESSO.

There are two file partitions, the `home` and the `scratch` partition. Go ahead and make a symbolic link to the `scratch` partition using:

```bash
ln -s $SCRATCH scratch
```

**Perform all your calculations from the scratch partition.**


____

<a name='testing'></a>

## Making Sure Everything Works ##

Once you are logged into the terminal, run:

```bash
ase-gui
```

and make sure a graphical interface appears. Next, run Python in interactive mode by typing:

```bash
python
```

and make sure the following commands work:

```python
import ase
import numpy
```


