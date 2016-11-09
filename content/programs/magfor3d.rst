.. _magfor3d:

MAGFOR3D
========

This program performs forward modelling. Command line usage:

``magfor3d mesh.msh obs.loc model.sus [topo.dat]``

and will create the forward modelled data file ``magfor3d.mag``.

Input files
-----------

All files are in ASCII text format - they can be read with any text editor. Input files can have any name the user specifies. Details for the format of each file can be found in  :ref:`elements`. The files associated with MAGFOR3D are:

- ``mesh.msh``: The 3D :ref:`mesh <meshfile>`.

- ``obs.loc``: The observation :ref:`locations <magfile>`.

- ``model.den``: The susceptibility :ref:`model <modelfile>` in SI.

- ``topo.dat``: Surface :ref:`topography <topofile>` (optional). If omitted, the surface will be treated as being flat and the top of the 3D mesh.

Output file
-----------

The created file is ``magfor3d.mag``. The file format is that of the :ref:`data file <magfile>` without the associated standard deviations. The forward modelled data are in **nT**.

