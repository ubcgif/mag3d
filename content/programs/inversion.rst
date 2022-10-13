.. _mag3d_inv:

.. important:: In 2022-10, a more exact definition of the regularization was implemented in **maginv3d_60.exe** for sparse-norm inversion. The package containing the improved executable was released as **MAG3D v6.0.1**. Be aware that MAG3D v6.0 and v6.0.1 have all the same features and use the same executable names. Differences in the recovered model using each package were found to be insignificant.

Inversion Program
=================

The inversion is carried out using the executable **maginv3d_60.exe**.

Running the Program
^^^^^^^^^^^^^^^^^^^

The inversion is run by opening a command line window and typing the path to the code **maginv3d_60.exe**, followed by a space, followed by the path to the :ref:`input file <mag3d_inv_input>` (denoted here as **inv.inp**).

.. figure:: images/run_inv.png
    :align: center
    :width: 600


Units:
------

**Input and outputs:**

    - **Magnetic data:** total magnetic intensity data or amplitude data in nT
    - **Magnetic susceptibility model:** magnetic susceptibility in SI

Output Files
------------

The program **maginv3d_60.exe** creates the following output files:

    - **maginv3d.log**: The log file containing the minimum information for each iteration and summary of the inversion.

    - **maginv3d.out**: The "developers" log file containing the details of each iteration including the model objective function values for each component, number of conjugate gradient iterations, etc.

    - **maginv3d_xxx.sus**: Susceptibility (SI) :ref:`model files <modelFile>` output after each "xxx" iteration (i.e., **maginv3d_012.sus**)

    - **maginv3d_xxx.pre**: :ref:`Predicted data files <magFile>` (without uncertainties) output after each "xxx" iteration.






