#GBMC

GBMC stands for Grain Boundary Monte-Carlo simulation. 


##Introduction
This software is the implementation of the Monte-Carlo (random insertion-removal) technique for Grain Boundary simulation that has been presented in the following article:

Banadaki A.D., Tschopp M.A., Patala S., An efficient Monte Carlo algorithm for determining the minimum energy structures of metallic grain boundaries, Computational Materials Science, v.155, p.466-475, https://doi.org/10.1016/j.commatsci.2018.09.017



##Inputs:

1.  ```gb_folder```      : create a folder with the name pattern shown in the provided example (e.g. ```15_al_S5_0_N1_1_-2_1_N2_-1_1_-2```). The boundary plane normal (bpn) indices are read from the folder name (can be changed later on). Indices followed by the N1 indicate the bpn indices of the lower crystal and those following the N2 indicate the indices of the upper crystal.
2.  ```folder_contents```: the gb_folder must contain the following two files:
	
	
	2.1.  ```dump.*```      : The ```gb_folder``` must contain a file with pattern ```dump.*```.
	
	2.2.  ```gb_area```        : A file named ```gb_area``` that contains the area of the GB plane.

3.  ```gb_main_folder``` path  : full path to ```gb_folder```.
4.  parameter defined in ```GBMC_RUN.m```: ```GBMC_RUN.m``` is a matlab script that runs the simulation and contains the following parameters that are hard-coded in it:

	4.1.  ```lammps_exe_path``` : full path to the ```lammps``` executable on your system (e.g. '/usr/bin/lmp_daily').

	4.2.  ```max_steps```       : maximum number of Monte-Carlo steps (includes the rejected steps).

	4.3.  ```T```               : psuedo temperture in Kelvin for computing the insertion/removal probabilities (```0.5 x T_melting```).

	4.4.  ```elem```            : a string representing the name of the species that is simulated e.g. 'al', 'ni', 'cu', etc.

	4.5.  ```lat_param```       : lattice parameter in __angstroms__ e.g. 4.05 for _Al_.

	4.6.  ```lat_type```        : a string indicating the lattice type e.g. ```fcc```, ```bcc```, etc.

	4.7.  ```csm_crit```        : the critical centro-symmetry value than can identify the GB atoms (0.1 is enough for fcc). Can be easily expanded to other crteria for complicated system where csm does not work.
	


##Dump-file requirements:
It is impossible to come up with a general rule for how simulation boxes are setup.
Different groups have different conventions. I have set up the code such that it matches the convention that is followed in [Banadaki et. al's article](https://doi.org/10.1016/j.commatsci.2015.09.062)

The following assumptions are made:

1. The file format should be a lammps dump-file (and not a lammps data file)
2. GB plane normal is along the Y direction.
3. The simulation is setup such that GB plane remains periodic.
4. The following columns must be present in the dump-file with the specified order.
   __id, type, x, y, z, c_eng, Centrosymmetry__

>Note that your dump-files do not usually have the ```Centrosymmetry``` column. You must compute this parameter for each atom and store it as the last column in the dump-file

##How to submit your MC jobs:
```GBMC_RUN.m``` is a matlab function that takes the path of your ```gb_folder``` with the above properties and automatically runs your MC simulation, stores the results and cleanup and compresses the files associated with the intermediate steps your MC simulation (e.g. rejected steps etc.). I have prepared the the follwoing two shell scripts for batch submiting you jobs.

- ```run_mc_local.sh```: to run a job locally on a machine that has matlab installed
- ```run_mc_on_cluster.sh```: to run a job through a job scheduler. The provided command only apply to IBM's LSF job scheduler. To run it on slum or other job schedulers you must change the commands.

##How to Cite:
Banadaki A.D., Tschopp M.A., Patala S., An efficient Monte Carlo algorithm for determining the minimum energy structures of metallic grain boundaries, Computational Materials Science, v.155, p.466-475, https://doi.org/10.1016/j.commatsci.2018.09.017

**Authors**: [Arash D. Banadaki](adehgha@ncsu.edu), [Srikanth Patala](spatala@ncsu.edu)

Copyright (c) 2018 , North Carolina State University 