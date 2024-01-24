
Installation Guide
++++++++++++++++++

SIMONA requires a Linux system. If you do not have a working conda or mamba installation, please install mambaforge for your architecture from `github.com/conda-forge/miniforge <https://github.com/conda-forge/miniforge>`_.

After installing, make sure you have the **mamba** command available in your shell and call:

.. code-block:: bash

   # Create a new environment for SIMONA:
   mamba create --name=simona simona_binary -c https://mamba.nanomatch-distribution.de/mamba-repo -c conda-forge
   # Activate the environment to see if it exists
   conda activate simona

