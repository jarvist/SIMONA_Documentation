SIMONA Settings Markup Language file (SML)
++++++++++++++++++++++++++++++++++++++++++

Configuration file needed to create SIMONA XML input file and run simona simulations from a previous 
simulation or comand line pre-processor. This file is written in yaml language.

In some cases, the file ending .yml (SIMONA YML) might be used instead of .sml.
These two file endings are interchangeable.

Structure of the SML file
~~~~~~~~~~~~~~~~~~~~~~~~~

Normally, you could get this file saving it after finishing the set up of
any MC simulation using the GUI. It can be obtained manually, but it is 
recomended to obtain a template from GUI pre-processor and edit its content.
 
Since the 'default flow style' setting might differ, SML files could look a bit 
different from below example.

In this example python tuples are used, which will be deprecated in the future.
Instead of::

    - !!python/tuple
    - <first value>
    - <second value>

it is recommended to use a simple YAML list::

   - <first value>
   - <second value>


The SML file is sub-divided by 5 sections:

Force field terms
~~~~~~~~~~~~~~~~~

Force field terms start as::

    forcefield_spec:
    - !!python/tuple
      - <NAME OF THE FF>
      - <FF settings>

Example 1 ::

    forcefield_spec:
    - !!python/tuple
      - NonbondedVacuum
      - depth: 3
        e_in: 2.6

Example 2::

    forcefield_spec:
    - !!python/tuple
      - SIMONAGBTerm
      - depth: 0
        e_in: 1.0
        e_out: 4.0
        scale: 1.0
    - !!python/tuple
      - CoulombElectrostatics
      - epsilon_r: 1.0
        scale: 1.0
        use_OpenCL: false
    - !!python/tuple
      - MartinLennardJones
      - depth: 2
        scale: 1.0

Each force field term has their own settings, but in general
you should set in each one the scale and sometimes the depth:

    “Depth is usually the number of explicit LJ atoms it skips 
    in the bonded interaction. Scale should be left at default and 
    can sometimes be used to artificially increase the strength of 
    the interaction.”

MC moves
~~~~~~~~

MC moves are the perturbations or movements you will perform in 
the simulations. It is necessary to set which Dihedral angles are 
allowed to move… or other kind of movement.

Take into account, SIMONA will move only the dihedral angles is 
able to recognize. If you want it to recognize a special angle, 
you should modify the SPF file and set the right parameter for 
this purpose.

For example, moves for output are::

    moves:
      analysis_moves:
      - - print_energy
        - {begin_step: 0, last_step: 0, step_mod: 1000}
      - - BestConfigurationOutput
        - {begin_step: 0, data_type: pdb, fname: best.pdb, last_step: 0, step_mod: 1}
      - - trajectory
        - {begin_step: 0, fname: trajectory.pdb, last_step: 0, only_new: 0, step_mod: 10000}
      - - boinc_checkpoint
        - {begin_step: 0, last_step: 0, step_mod: 10000000}


Then, after the flag initial you can set the structural movementes needed.

Example 1 (all dihedrals are moving)::

    initial: []
      list:
      - - new_dihedrals
        - allow:
          - all
          angles: '*'
          delta_phi_max: 0.5235987755982988

Example 2 (protein dihedrals moving)::

    - - new_dihedrals
      - allow:
          chi:
            distribution: {name: gaussian, sigma: 0.3490658503988659}
            mode: RelativeRandom
            weight: 1.0
          phi:
            distribution: {name: gaussian, sigma: 0.3490658503988659}
            mode: RelativeRandom
            weight: 1.0
          psi:
            distribution: {name: gaussian, sigma: 0.3490658503988659}
            mode: RelativeRandom
            weight: 1.0
    - - rigid_trans
      - {delta: 1.4}
    - - rigid_rot
      - {delta: 0.08726646259971647}

SIMONA Output
~~~~~~~~~~~~~

The output movements or analysis_moves are useful 
to get more information about the simulation on the fly.
We advise to use not only the stdout (standard output, 
only energy contributions by step) and use the BestConfigurationOutput::

    HERE THE Example



Simulation steps
~~~~~~~~~~~~~~~~

The total number of simulation steps can be modified in::

    nsteps: 100000

Preprocessor
~~~~~~~~~~~~~
In this section MC criteria and settings must be specified and the source of parameters.

Example::

    preprocessor:
      algorithm: {name: metropolis}
      atom_params: topol.spf
      name: nano
      use_simona_pdb_parser: true
    print_level: 1
    seed: random
    sourceFormat: 5

Temperature regime
~~~~~~~~~~~~~~~~~~

Here the temperature regime of the simulation can be set. The flags tstart 
and tend are initial temperature and final temperature, respectively.
The values are specified as follow::

    tend: 300.0
    tstart: 300.0
    verboseDihedral: false