
Installation Guide
++++++++++++++++++

Get SIMONA
===================
SIMONA is free for academic use. Please visit http://int.kit.edu/nanosim/simona for more information.

Set up
======
Before using SIMONA you may need some libraries and programs to compile or execute SIMONA. You will need:

* The latest version of GROMACS available.
* The boost libraries (libboost-python-dev)
* PyQt5 (python-pyqt5)
* SIP (python-sip)
* numpy (python-numpy)
* scipy (python-scipy)
* matplotlib (python-matplotlib)
* YAML parser and emitter for Python (python-pyyaml)
* lxml (python-lxml)
* OpenBabel



Pre-compiled Version
====================

SIMONA pre-compiled version will work on Ubuntu 18.04 and higher. Centos 7 is not supported. It requires glibc higher than GLIBC_2.27.

Once you obtain the pre-compiled version untar::

     tar -xvf simona.tar.gz

and then add in your .bashrc::

    export PATH=$PATH:/PATH_TO_SIMONA/simona/python
    export PATH=$PATH:/PATH_TO_SIMONA/simona/bin

.. note::
    The pre-compiled version is limited only to basic and advance use of SIMONA.
    To have developer features, source code is required.


Source code
===========

To install and compile the source code of SIMONA Scons is required. 
Please, follow the following steps::
    
    tar -xvf simona-master.tar.gz
    cd simona
    scons -j 4 

.. note::
    Please, replace "4" with the number of cores you have available to use for compilation according to your machine.


and then add in your .bashrc::

    export PATH=$PATH:/PATH_TO_SIMONA/simona/python
    export PATH=$PATH:/PATH_TO_SIMONA/simona/bin