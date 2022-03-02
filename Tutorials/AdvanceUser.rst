
Advance User Tutorials
++++++++++++++++++++++

Generic Monte Carlo - Protein Simulations
=========================================

SIMONA pre-processor is able to take Gromacs files inputs (.gro, .top and .itp) to create a XML file that contains the coordinates and parameters needed for the MC simulation. 
For protein systems, we will use amber99sb-star-ildn force field (FF) (if it is not available, you could still use the amber99sb-ildn implemented in Gromacs. 

Protein force field
~~~~~~~~~~~~~~~~~~~

Please copy the folder amber99sb-star-ildn provided in the test into the default GROMACS force field directory::

    cp -r amber99sb-star-ildn  PATH_TO/gromacs/share/gromacs/top

This will allowed Gromacs to use the special parameters (Custom FF) needed for the simulation.

In this tutorial we will use a protein from the Protein Data Bank (PDBcode :1l2y). Now in your working directory perform the following steps:

#. Generated .gro and .top (GROMACS coordinates and topology) files from PDB file 1l2y.pdb using GROMACS::

    gmx pdb2gmx -f 1l2y.pdb -o test.gro -ignh -water none -ff amber99sb-star-ildn

SIMONA XML inputs
~~~~~~~~~~~~~~~~~

#. Generate your first XML SIMONA input. 

We will perform a simulation protocol of two steps. The first step is a constant temperature simulation at 360K. For that, a conf.sml file is required.

First, you will create a .xml input file with the following line using the .sml configuration file and the .gro and .top files::
    
    SIMGenerateIdenticalRun.py step1_ConstantTemperature.sml test.gro topol.top run_step1.xml

and then initiate the simulation::

    poempp run_step1.xml > run_step1.out

Once the simulation is finished, output files will be generated and we could continue with the second step.

#. Generate a XML SIMONA input from previous simulation.

The second step is a simulated annealing simulation increasing the temperature from 360K to 500K using the starting point coordinates the last frame of the previous simulation.

We will use the best.pdb file from our first MC simulation converting it to a .gro file::

    obabel best_step1.pdb -o gro -O best_step1.gro

and then obtain the new XML file::

    SIMGenerateIdenticalRun.py step2_SimulatedAnnealing.sml best_step1.gro topol.top run_step2.xml

Finally, we perform the simulation::

    poempp run_step2.xml > run_step2.out

.. note::
    We recommend to open trajectory.pdb with VMD to check the trajectory of the simulation.



Generic Monte Carlo - Organic Molecules
=======================================

To obtain the parameter files compatible with SIMONA for a new organic molecule
gromacs files can be used as starting point.The easies way to get parameters for 
new organic molecules with GAFF is using ambertools or acpype.

Gromacs files sources (Alternatives)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
**Source 1:** Using amber parameters files .top and .crd using ambertools first and then 
create the gromacs files with acpype.

Example::

    acpype -p amberfile.top -x amberfile.crd

**Source 2:** Gromacs files using directly acpype.

Example::

    acpype -i coordinates.pdb

.. note::
    If you will use acpype to parametrize your molecule consider the right options (net charge, charge method, etc) given by the acpype manual.

GROMACS files to SIMONA input files
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Once you get the .gro and .top files, then you can use the gromacs_to_epqr.py script.

This script is able to create epqr or pqr files, which contains parameters such as 
coordinates, epsilon, gamma and radii values.

.. note::
    If you want to include the radii in your parameters you have to provide custom_radii.itp.

Here you can read the options of this script::

    usage: ./gromacs_to_epqr.py [--options] <topol.top> <conf.gro> [outname.epqr [custom_radii.itp]]

    arguments in [] are optional
    topol.top: a topology file from gromacs
    conf.gro: the corresponding conformation file
    outname.epqr: the output epqr/pqr file
    custom_radii.itp: a file with custom radii (ignored if option --mbondi2 is used)
    
    options:
    --with-factor:     use HCT/OBC atomic radii instead of Still
    --pqr:             write a pqr instead of an epqr
    --mbondi2:         use mbondi2 radii instead of gromacs atomic radii
    --nodh:            do not create a dh file
    --rename-termini   rename N-Termins residue 'XYZ' to 'NXY' and C-Terminus to 'CXY'

for this tutorial we will execute::

    gromacs_to_epqr.py *_GMX.top *_GMX.gro outname.epqr custom_radii.itp

Then a coordinates file in .mol2 extension is needed to provide SYBIL atom types::

    obabel -i pqr outname.epqr -o mol2 topol.mol2

Finally, you can create the parameter file topol.spf to be used in the SIMONA pre-processor (GUI or command line)

The script mol2spf.py will be used::

    mol2spf.py topol.mol2 outname.epqr topol.spf

Generating XML input file and run MC simulation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Once the files topol.spf file and a coordinates.pdb file 
is obtained, generate a SIMONA XML file can be accomplished by two ways.

The first one is using SIMONA GUI explained in our Basic User tutorials and
the other one is using a conf.sml file containing all the settings needed
and execute SIMGenerateIdenticalRun.py.

 Example::

     SIMGenerateIdenticalRun.py conf.sml test.pdb run.xml

Now we can execute our MC simulation as::

    poempp run.xml > run.out


SIMONA Worflows - Using XML files 
=================================

To create a specific workflow in SIMONA it is necessary to
manually edit the Algorithm secction in our XML input file.

In this example we will only edit dihedral movements to generate
a dihedral scan on n-butane.


