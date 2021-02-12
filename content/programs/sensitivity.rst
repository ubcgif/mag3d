.. _mag3d_sens:

Sensitivity Matrix
==================

The binary file containing the sensitivity matrix and distance weighting is created with the executable **magsen3d_60.exe**.

Running the Program
^^^^^^^^^^^^^^^^^^^

The sensitivity matrix file is constructed by opening a command line window and typing the path to the code **magsen3d_60.exe**, followed by a space, followed by the path to the :ref:`input file<mag3d_sens_input>` (denoted here as **sens.inp**).

.. figure:: images/run_sensitivity.png
    :align: center
    :width: 600



Output Files
^^^^^^^^^^^^

The program **magsen3d_60.exe** creates the following output files:

    - **magsen3d.log**: A log file summarizing the run of the program.

    - **maginv3d.mtx**: The sensitivity matrix file to be used in the inversion. This file contains the sensitivity matrix, generalized depth weighting function, mesh, and discretized surface topography. It is produced by the program and it's name is not adjustable. It is very large and may be deleted once the work is completed.

    - **sensitivity.txt**: This file is a :ref:`model file <modelFile>` that contains the average sensitivity for each cell. This file can be used for depth of investigation analysis or for use in designing special model objective function weighting.

    - Diagnostic files as described above to examine the wavelet compression properties, if chosen (**Diag=1**).

