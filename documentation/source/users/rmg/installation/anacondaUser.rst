.. _anacondaUser:

****************************************************************************
Binary Installation Using Anaconda for Unix-Based Systems: Linux and Mac OSX
****************************************************************************


#. Download and install the `Anaconda Python Platform <https://www.anaconda.com/download/>`_ for Python 3.7.

   The download will be a .sh file with a name like ``Anaconda3-2019.07-Linux-x86_64.sh``. Open a terminal in the same
   directory as this file, and type the following to install Anaconda (replace the name of your .sh file below). ::

    bash Anaconda3-2019.07-Linux-x86_64.sh

   **When prompted to append Anaconda to your PATH, select or type Yes**.  Install the Anaconda folder inside your home directory (typically ``/home/YourUsername/`` in Linux and ``/Users/YourUsername`` in Mac). When prompted, you do not need to install Microsoft VSCode (but feel free to if you are looking for a lightweight IDE).

#. Install both RMG and the RMG-database binaries through the terminal.   Dependencies will be installed automatically. It is safest to make a new conda environment for RMG and its dependencies. Type the following command into the terminal to create the new environment named 'rmg_env' containing the latest stable version of the RMG program and its database. ::

    conda create -c defaults -c rmg -c rdkit -c cantera -c pytorch -c conda-forge --name rmg_env rmg rmgdatabase

   Whenever you wish to use it you must first activate the environment::

    source activate rmg_env

#. Optional: If you wish to use the :ref:`QMTP interface <qm>` with `MOPAC <http://openmopac.net/>`_ to run quantum mechanical calculations for improved thermochemistry estimates of cyclic species, please obtain a legal license through the `MOPAC License Request Form <http://openmopac.net/form.php>`_.  Once you have it, type the following into your terminal ::

    mopac password_string_here

#. You may now run an RMG test job. Save the `Minimal Example Input File <https://raw.githubusercontent.com/ReactionMechanismGenerator/RMG-Py/master/examples/rmg/minimal/input.py>`_
   to a local directory.  Use the terminal to run your RMG job inside that folder using the following command ::

    rmg.py input.py

   If you encounter the ImportError related to ``libmkl_rt.so.2``, refer to the :ref:`Fixing the ImportError related to libmkl_rt.so.2 <fixImportError>`
   section below to fix the error and re-run the RMG test job.

You may now use RMG-Py, Arkane, as well as any of the :ref:`Standalone Modules <modules>` included in the RMG-Py package.

.. _fixImportError:

Fixing the ImportError related to ``libmkl_rt.so.2``
============================================================

You may encounter the following ImportError when you try to run a RMG test job ::

    Traceback (most recent call last):
      File "/PATH-TO-YOUR-ANACONDA/envs/rmg_env/bin/rmg.py", line 48, in <module>
        from rmgpy.rmg.main import RMG, initialize_log, process_profile_stats, make_profile_graph
      File "/PATH-TO-YOUR-ANACONDA/envs/rmg_env/lib/python3.7/site-packages/rmgpy/rmg/main.py", line 51, in <module>
        from cantera import ck2cti
      File "/PATH-TO-YOUR-ANACONDA/envs/rmg_env/lib/python3.7/site-packages/cantera/__init__.py", line 4, in <module>
        from ._cantera import *
    ImportError: libmkl_rt.so.2: cannot open shared object file: No such file or directory

where ``PATH-TO-YOUR-ANACONDA`` is the path to the ``Anaconda3`` directory installed on your machine.
The default install location of Anaconda on Linux is ``/home/<your_username>/Anaconda3``.

To fix this issue, simply copy ``libmkl_rt.so.1`` as ``libmkl_rt.so.2`` under the rmg environment library by typing the following
line on your terminal::

    cp /PATH-TO-YOUR-ANACONDA/envs/rmg_env/lib/libmkl_rt.so.1 /PATH-TO-YOUR-ANACONDA/envs/rmg_env/lib/libmkl_rt.so.2

Note that ``PATH-TO-YOUR-ANACONDA`` needs to be the path to your ``Anaconda3`` directory, which mostly likely looks
like ``/home/<your_username>/Anaconda3``. If you are unable to locate the ``libmkl_rt.so.1`` file on your computer, you can find its location with the following command::

    locate libmkl_rt.so.1

After copying the file as ``libmkl_rt.so.2``, try running the RMG test job again::

    rmg.py input.py

You will see a line saying ``MODEL GENERATION COMPLETED`` on your terminal if your RMG test job ran successfully.


Updating your binary installation of RMG in Linux or Mac OSX
============================================================

If you had previously installed a binary version of the RMG package, you may
check and update your installation to the latest stable version available on Anaconda by typing the following command on the terminal ::

    source activate rmg_env
    conda update rmg rmgdatabase -c rmg
