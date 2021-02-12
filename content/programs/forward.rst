.. _mag3d_fwd:

Forward Modeling
================

The program **magfor3d_60.exe** performs the 3D forward modelling of magnetic data for a magnetic susceptibility model defined on a tensor mesh.

Running the Program
^^^^^^^^^^^^^^^^^^^

To run the executable, open a command window. In order, enter the path to the *magfor3d_60* executable, the :ref:`tensor mesh file <meshfile>`, the :ref:`survey file <magfile>`, the :ref:`susceptibility model file <modelfile>`, the :ref:`topography file <topofile>` (optional) and a *lower bound value*. This in shown below.

.. figure:: images/run_fwd.png
     :align: center
     :width: 700


Units
^^^^^

    - **Magnetic data:** total magnetic intensity data in nT
    - **Magnetic susceptibility model:** magnetic susceptibility in SI


Output Files
^^^^^^^^^^^^

The program **magfor3d_60.exe** creates the following output files:

    - **magfor3d.mag:** Predicted data file.

    - **magfor3d.log:** Log file which provides details about the parameters used in the forward modelling and diagnostic information about the results.