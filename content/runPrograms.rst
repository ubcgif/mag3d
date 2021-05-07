.. _running:

Running the programs
====================

This section provides describes how to run all executables pertaining to the MAG3D package.

.. note::

    All executable files, input files, output filenames and files specified within input files can be specified in the following manner:

    - as just the filename if contained within the current working directory (Example: *filename.txt*)
    - as a file path relative to the current working directory (Example: *sub_dir\\filename.txt*)
    - as the full path (Example: *C:\\Users\\Name\\Tests\\filename.txt*)

    Executable files should **not** be renamed. However, input file names can be specified by the user if desired.


Executables
-----------

The program library consists of the programs:

    - **magfor3d_60.exe**: A code for forward modeling magnetic data for a magnetic susceptibility model.

    - **magsen3d_60.exe**: calculates the sensitivity matrix for the inversion and outputs sensitivity weights.

    - **maginv3d_60.exe**: performs 3D inversion of magnetic data to recover a susceptibility model.

    - **magpre3d_60.exe**: multiplies the sensitivity file by the model to get the predicted data. This rarely used utility multiplies a model by the sensitivity matrix in to produce the predicted data. This program is included so that users who are not familiar with the wavelet transform and the structure of can utilize the available sensitivity matrix to carry out model studies.

Utility codes relevant to this package include:

   - **blk3cell.exe:** A utility for generating block models on tensor meshes

   - **pfweight_60.exe:** A utility for computing depth or distance weighting for potential field inversion


Contents
--------

To learn the specifics of running each executable, see the following sections:

  .. toctree::
    :maxdepth: 1

    Create Model <programs/createModel>
    Forward Modeling <programs/forward>
    Depth/Distance Weighting <programs/pfweight>
    Sensitivity Matrix <programs/sensitivity>
    Inversion <programs/inversion>
    Predict Data with Pre-Computed Sensitivity Matrix <programs/magpre3d>







