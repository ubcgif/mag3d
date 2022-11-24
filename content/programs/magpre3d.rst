.. _magpre3d:

MAGPRE3D
========

This utility multiplies a model by the stored sensitivity matrix in to produce predicted data. This program is included so that users who are not familiar with the wavelet transform and the structure of could utilize the available sensitivity matrix to carry out modelling exercises. The command line usage is:

``magpre3d_60 maginv3d.mtx obs.loc model.sus``

Input files
-----------

#. ``maginv3d.mtx``: The sensitivity matrix file create from :ref:`mag3d_sens`.

#. ``obs.loc``: The magnetic :ref:`location file <magfile>`.

#. ``model.sus``: The susceptibility :ref:`model file <modelFile>` in SI.


Output file
-----------

The output file is a :ref:`predicted data file <magfile>` (omitting uncertainty column) and is named ``magpre3d.mag``. This program can be used to reproduce output predicted files from :ref:`maginv3d_60 <mag3d_inv>`.

