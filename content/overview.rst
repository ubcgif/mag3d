.. _overview:

MAG3D v6.0 Package Overview
===========================

Highlights of v6.0
------------------

Many advancements have been made since the previous version of this coding package.
Highlights of Mag3D v6.0 include:


    - the ability to forward model and invert surface, borehole, and airborne magnetic data in 3D
    - the ability to forward model and invert both total magnetic intensity and amplitude data
    - sensitivity weighting so that targets recovered through inversion are placed at the correct depth
    - the ability to recover compact and/or blocky models using sparse norms, in additional to smooth models using a standard least-squares approach
    - implementing wavelet compression to reduce the storage cost of the sensitivity matrix and allow the user to solve larger problems


General Code Description
------------------------

is a program library for carrying out forward modelling and inversion of surface, airborne, and/or borehole magnetic data in the presence of a three dimensional Earth. The program library carries out the following functions:

#. Forward modelling of the magnetic field anomaly response of a 3D volume of susceptibility.

#. The model is specified in the mesh of rectangular cells, each with a constant value of susceptibility. Topography is included in the mesh. The magnetic response can be calculated anywhere within the model volume, including above the topography to simulate ground or airborne surveys. There is also a capability to simulate and invert data collected beneath the surface (e.g. borehole surveys) and combinations of ground and borehole surveys.

#. Assumptions:

   -  This code assumes susceptibilities are small enough that the effects of self-demagnetization can be neglected.

   -  Remanent magnetization is not directly accounted for, although anomaly projections can be included with the observations.

#. Inversion of surface, airborne, and/or borehole magnetic data to generate 3D models of susceptibility contrast:

   -  The inversion is solved as an optimization problem with the simultaneous goals of (i) minimizing a model objective function and (ii) generating synthetic data that match observations to within a degree of misfit consistent with the statistics of those data.

   -  To counteract the inherent lack of information about the distance between source and measurement, the formulation incorporates depth or distance weighting.

   -  By minimizing the model objective function, distributions of subsurface susceptibility contrast are found that are both close to a reference model and smooth in three dimensions. The degree to which either of these two goals dominates is controlled by the user by incorporating prior geophysical or geological information
      into the inversion. Explicit prior information may also take the form of upper and lower bounds on the susceptibility contrast in any cell.

   -  The regularization parameter (controlling relative importance of objective function and misfit terms) is determined in one of three ways, depending upon how much is known about errors in the measured data.

   -  Implementation of parallel computing architecture (OpenMP) allows the user to take full advantage of multi-core processors on a CPU. A cluster-based code using Message Passing Interface (MPI) is also available. Notes on computation speed are found at the end of this section.

#. The large size of 3D inversion problems is mitigated by the use of wavelet compression. Parameters controlling the implementation of this compression are available for advanced users.

The initial research underlying this program library was funded principally by the mineral industry consortium "Joint and Cooperative Inversion of Geophysical and Geological Data" (1991 - 1997) which was sponsored by NSERC and the following 11 companies: BHP Minerals, CRA Exploration, Cominco Exploration, Falconbridge, Hudson Bay Exploration and Development, INCO Exploration & Technical Services, Kennecott Exploration Company, Newmont Gold Company, Noranda Exploration, Placer Dome, and WMC.

The current improvements have been funded by the consortium "Potential fields and software for advanced inversion" (2012-2016) sponsored by Newmont, Teck, Glencore, BHP Billiton, Vale, Computational Geoscience Inc, Cameco, Barrick, Rio Tinto, and Anglo American.

Program library content
-----------------------

Executable programs
^^^^^^^^^^^^^^^^^^^

The program library consists of the programs:

    - **magfor3d_60.exe**: A code for forward modeling magnetic data for a magnetic susceptibility model.

    - **magsen3d_60.exe**: calculates the sensitivity matrix for the inversion and outputs sensitivity weights.

    - **maginv3d_60.exe**: performs 3D inversion of magnetic data to recover a susceptibility model.

    - **magpre3d_60.exe**: multiplies the sensitivity file by the model to get the predicted data. This rarely used utility multiplies a model by the sensitivity matrix in to produce the predicted data. This program is included so that users who are not familiar with the wavelet transform and the structure of can utilize the available sensitivity matrix to carry out model studies.

Utility codes relevant to this package include:

   - **blk3cell.exe:** A utility for generating block models on tensor meshes

   - **pfweight_60.exe:** A utility for computing depth or distance weighting for potential field inversion

Graphical user interfaces
^^^^^^^^^^^^^^^^^^^^^^^^^
GUI-based utilities for these codes include respective viewers for the data and models. They are only available on Windows platforms and can be freely downloaded through the UBC-GIF website:

   - `GM_DATA_VIEWER <http://www.eos.ubc.ca/~rshekhtm/utilities/gm-data-viewer.zip>`__: a utility for viewing raw surface or airborne data (not borehole data), error distributions, and for comparing observed to predicted data directly or as difference maps.
   - `MeshTools3D <http://www.eos.ubc.ca/~rshekhtm/utilities/MeshTools3d.zip>`__: a utility for displaying resulting 3D models as volume renderings. Susceptibility volumes can be sliced in any direction, or isosurface renderings can be generated.

Licensing
---------

MAG3D v6.0 is currently only available to the sponsors of the "Potential fields and software for advanced inversion" consortium.

Installing
----------

There is no automatic installer currently available for this package. Please follow the following steps in order to use the software:

#. Extract all files provided from the given zip-based archive and place them all together in a new folder such as

#. Add this directory as new path to your environment variables.

Two additional notes about installation:

-  Do not store anything in the "bin" directory other than executable applications and Graphical User Interface applications (GUIs).

-  A Message Pass Interface (MPI) version is available for Linux upon and the installation instructions will accompany the code.