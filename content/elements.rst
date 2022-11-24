.. _elements:

.. important:: The features and executable names within the MAG3D the v6.0, v6.0.1 and v6.0.2 packages remain the same. Differences in version number correspond to improvements in performance.


Elements of the MAG3D v6 Package
================================

This section provides a brief description of each program in the MAG3D v6 library. In addition, we describe the file formats for all input and supporting files used by the coding library.

Introduction
------------

The program library consists of the programs:

    - **magfor3d_60.exe**: A code for forward modeling total magnetic data for a magnetic susceptibility model.

    - **magsen3d_60.exe**: calculates the sensitivity matrix for the inversion and outputs sensitivity weights.

    - **maginv3d_60.exe**: performs 3D inversion of magnetic data to recover a susceptibility model.

    - **magpre3d_60.exe**: multiplies the sensitivity file by the model to get the predicted data. This rarely used utility multiplies a model by the sensitivity matrix in to produce the predicted data. This program is included so that users who are not familiar with the wavelet transform and the structure of can utilize the available sensitivity matrix to carry out model studies.

Utility codes relevant to this package include:

   - **blk3cell.exe:** A utility for generating block models on tensor 

   - **pfweight_60.exe:** A utility for computing depth or distance weighting for potential field inversion


Main Input Files
----------------

Here, we describe the main input files for executables contained with the MAG3D v6 coding package.

.. toctree::
    :maxdepth: 1

    Create model <inputfiles/createModel>
    Forward modeling <inputfiles/forward>
    Depth/Distance weights <inputfiles/pfweights>
    Sensitivity matrix <inputfiles/sensitivity>
    Inversion <inputfiles/inversion>


Supplementary Files
-------------------

Here, we describe the formats of supplementary files used to run MAG3D v6.

.. toctree::
    :maxdepth: 1

    Mesh <files/meshfile>
    Topography <files/topo>
    Observation/Location <files/magfile>
    Susceptibility and active models <files/model>
    Weighting <files/weight>

