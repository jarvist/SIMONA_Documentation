SIMONA Extensible Markup Language file (XML)
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

The XML files are the simulation input of SIMONA that will be used to perform the simulation 
with "poempp" module. It is written in a Extensible Markup Language file (.xml). 
This file collects all the parameters of your system and the set of rules 
that will be used in the MC simulation.

This file can be created with the SIMONAGUI (/PATH/TO/SIMONA/python/simonagui.py) 
or, if a SIMONA parameter file already exists (SML/YML), with the script
SIMGenerateIdenticalRun (/PATH/TO/SIMONA/SIMGenerateIdenticalRun.py).

The following description is valid for the XML input file which is necessary to 
start a SIMONA simulation with poempp.

There are 5 important sections in the XML file::

    <simona_input>
        <simona_configuration>
            ...
        </simona_configuration>

        <simona_moves>
            ...
        </simona_moves>

        <energy_models>
            ...
        </energy_models>
        <Algorithm>

            ...
        </Algorithm>
        <PDB>

            ...
        </PDB>
        <Random_random_seed/>
    </simona_input>

SIMONA Configuration
~~~~~~~~~~~~~~~~~~~~

This section contains the coordinates for each atom.
In this example the first 3 atoms of a residue named 'ZZA' are shown::

    <simona_configuration>
        <coord id="0" residue_name="ZZA" residue_id="0" chain_id="0" name="C1" type="C">
            <X>-7.033038</X>
            <Y>-3.171916</Y>
            <Z>16.6546</Z>
        </coord>
        <coord id="1" residue_name="ZZA" residue_id="0" chain_id="0" name="H1" type="H">
            <X>-7.900285</X>
            <Y>-3.041589</Y>
            <Z>15.97632</Z>
        </coord>
        <coord id="2" residue_name="ZZA" residue_id="0" chain_id="0" name="H2" type="H">
            <X>-6.226541</X>
            <Y>-3.668942</Y>
            <Z>16.08142</Z>
        </coord>
        ...
    </simona_configuration>



SIMONA Moves
~~~~~~~~~~~~
This section contains info about the dihedral angles that will be rotated by Monte Carlo algorithm.

Example::

        <simona_moves>
            <simona_dihedrals>
            <DihedralSpec>
                <unique_id>0</unique_id>
                <DihedralInfo>
                  <bond_end>4</bond_end>
                  <bond_start>0</bond_start>
                  <predecessor>2</predecessor>
                  <successor>7</successor>
                </DihedralInfo>
                <AtomSet>
                  <range>
                    <start>5</start>
                    <end>85</end>
                  </range>
                </AtomSet>
                <InvertSet>
                  <range>
                    <start>1</start>
                    <end>4</end>
                  </range>
                </InvertSet>
                <type>1.ZZA.single</type>
                <atomnames>H2.C1.C2.C3</atomnames>
                <atomnames_reversed>C3.C2.C1.H2</atomnames_reversed>
            </DihedralSpec>

            <DihedralSpec>
                <unique_id>1</unique_id>
                <DihedralInfo>
                  <bond_end>7</bond_end>
                  <bond_start>4</bond_start>
                  <predecessor>0</predecessor>
                  <successor>9</successor>
                </DihedralInfo>
                <AtomSet>
                  <range>
                    <start>8</start>
                    <end>85</end>
                  </range>
                </AtomSet>
                <InvertSet>
                  <range>
                    <start>0</start>
                    <end>4</end>
                  </range>
                  <range>
                    <start>5</start>
                    <end>7</end>
                  </range>
                </InvertSet>
                <type>1.ZZA.single</type>
                <atomnames>C1.C2.C3.H7</atomnames>
                <atomnames_reversed>H7.C3.C2.C1</atomnames_reversed>
                 </DihedralSpec>
                ...
            </simona_dihedrals>
      <simona_subunits>
        <simona_subunit>
          <id>0</id>
          <AtomSet>
            <range>
              <start>0</start>
              <end>85</end>
            </range>
          </AtomSet>
        </simona_subunit>
      </simona_subunits>
    </simona_moves>





Energy Models
~~~~~~~~~~~~~
Here everything related to the forcefield can be found. The force field terms included in the 
simulation according to their specific implementation could give energy contributions or
be involved in a specific function for a respective protocol.

In the example the 'NonbondedVacuum' forcefield is used::

    <energy_models>
        <forcefield id="0" name="nano">
          <NonbondedVacuum>
            <epsilon_in>2.6</epsilon_in>
            <param>
              <charge>-0.0959</charge>
              <epsilon>0.1094</epsilon>
              <sigma>3.3997</sigma>
              <unique_id>0</unique_id>
              <AtomSet>
                <range>
                  <start>0</start>
                  <end>11</end>
                </range>
              </AtomSet>
              <AtomSet14>
                <range>
                  <start>8</start>
                  <end>11</end>
                </range>
              </AtomSet14>
            </param>
            <param>
              <charge>0.031</charge>
              <epsilon>0.0157</epsilon>
              <sigma>2.6495</sigma>
              <unique_id>1</unique_id>
              <AtomSet>
                <range>
                  <start>0</start>
                  <end>8</end>
                </range>
              </AtomSet>
              <AtomSet14>
                <range>
                  <start>5</start>
                  <end>8</end>
                </range>
              </AtomSet14>
            </param>
            ...
        </forcefield>
    </energy_models>




Algorithm
~~~~~~~~~

This section contains the workflow of the movements for the Monte Carlo algorithm. 
Here is specified how the movements are generated (Random, Relative, Absolute) and
in which steps the energy evaluation is performed or how the data of the simulation 
is storaged.

The default worflow looks like::

    <Algorithm>
      <RepeatedMove>
        <repeats>10000</repeats>
        <tend>300.0</tend>
        <tscaling>geometric</tscaling>
        <tstart>300.0</tstart>
        <TransformationSequence weight="1.0" repeats="1">
          <TransformationSequence weight="1.0" repeats="1">
            <RecalcEnergies>
              <beginstep>0</beginstep>
              <laststep>0</laststep>
              <stepmod>1000</stepmod>
            </RecalcEnergies>
            <EnergyOutput>
              <beginstep>0</beginstep>
              <laststep>0</laststep>
              <stepmod>1000</stepmod>
            </EnergyOutput>
            <PrintEnergy>
              <beginstep>0</beginstep>
              <laststep>0</laststep>
              <stepmod>1000</stepmod>
            </PrintEnergy>
            <BestConfigurationOutput>
              <beginstep>0</beginstep>
              <filename>best.pdb</filename>
              <laststep>0</laststep>
              <stepmod>1</stepmod>
              <type>pdb</type>
            </BestConfigurationOutput>
            <ConfigurationOutput>
              <beginstep>0</beginstep>
              <filename>trajectory.pdb</filename>
              <laststep>0</laststep>
              <only_new>0</only_new>
              <stepmod>1000</stepmod>
              <type>pdb</type>
            </ConfigurationOutput>
            <MetadataOutput>
              <beginstep>0</beginstep>
              <laststep>0</laststep>
              <metadataname>Summary</metadataname>
              <stepmod>1000</stepmod>
            </MetadataOutput>
          </TransformationSequence>
          <ConditionalTransformation weight="1.0">
            <MetropolisAcceptanceCriterion>
              <energymodel_nr>0</energymodel_nr>
              <kB>0.0019858775</kB>
            </MetropolisAcceptanceCriterion>
            <TransformationChoice weight="1.0" repeats="1">
              <SetDihedralRelativeRandom weight="1.0">
                <dihedral_id>0</dihedral_id>
                <distribution type="gaussian">
                  <sigma>0.5235987755982988</sigma>
                  <mean>0.0</mean>
                </distribution>
              </SetDihedralRelativeRandom>
              <SetDihedralRelativeRandom weight="1.0">
                <dihedral_id>1</dihedral_id>
                <distribution type="gaussian">
                  <sigma>0.5235987755982988</sigma>
                  <mean>0.0</mean>
                </distribution>
              </SetDihedralRelativeRandom>
              ...
            </TransformationChoice>
          </ConditionalTransformation>
        </TransformationSequence>
      </RepeatedMove>
    </Algorithm>



PDB structure
~~~~~~~~~~~~~
This section contains a copy of the PDB file of the simulated molecule(s) (the original PDB file as string without any further formatting).


