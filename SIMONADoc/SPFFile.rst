SIMONA Structure Parameter file (SPF)
+++++++++++++++++++++++++++++++++++++

Structure Parameter File (.spf) is the extension file for all the structural data required 
for the pre-processor of SIMONA to get and XML input file to perform generic MC simulations.
This file is written in the language 'yaml'. Since the 'default flow style' setting might 
differ, SPF files could look a bit different from below example.

In SPF files we can find two main secctions:

1. **Residue** section contains information about atom parameters:

* charge
* epsilon
* sigma
* radius
* SYBIL atom types

as well as bonded and dihedral atoms (intra-residue bonds and dihedrals) for each residue.

2. **Structure** section defines bonds and dihedrals between residues (inter-residue bonds).
This file can be created via mol2spf.py Python script, which can be found in the *simona/python/* 
folder.

Generic Example::

    residues:
        <residue_name>:
        atoms:
            <atom_name>:
            <parameter>: <value>
            <...>
        bonds:
        - - <bonded_atom_1>
          - <bonded_atom_2>
          - <bond_type ('1' for single bond, '2' for double bond; including the single quotes)>
            <...>
        moves:
            dihedral:
            - - - <dihedral_atom_1>
                - <dihedral_atom_2>
                - <dihedral_atom_3>
                - <dihedral_atom_4>
                - <bond_type ('single' -> no idea what this is for, but exists in every entry; without single quotes)
    structure:
        bonds:
        <same as residue bonds, but with atom name of format <residue_number>.<residue_name>.<atom_name> >
        moves:
            dihedral:
            <same as residue dihedrals, but with atom names as in structure bonds>
    parameters: {} 

Example for custom residue or monomer::

    residues:
        YRA:
          atoms:
            C1:
                charge: 0.0661
                epsilon: 0.1094
                mol2_atom_type: C.3
                radius: 1.7
                sigma: 3.3997
            C2:
                charge: 0.6833
                epsilon: 0.086
                mol2_atom_type: C.2
                radius: 1.7
                sigma: 3.3997
            H1:
                charge: 0.1008
                epsilon: 0.0157
                mol2_atom_type: H
                radius: 1.2
                sigma: 2.4714
            H2:
                charge: 0.1008
                epsilon: 0.0157
                mol2_atom_type: H
                radius: 1.2
                sigma: 2.4714
            O1:
                charge: -0.3936
                epsilon: 0.17
                mol2_atom_type: O.3
                radius: 1.5
                sigma: 3.0
            O2:
                charge: -0.5574
                epsilon: 0.21
                mol2_atom_type: O.2
                radius: 1.5
                sigma: 2.9599
        bonds:
        - - O1
          - C1
          - '1'
        - - C1
          - C2
          - '1'
        - - C1
          - H2
          - '1'
        - - C1
          - H1
          - '1'
        - - C2
          - O2
          - '2'
        moves:
          dihedral:
          - - - O1
              - C1
              - C2
              - O2
            - single


