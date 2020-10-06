.. _example:


.. note:: A new example has been developed for demonstrating functionality specific to v6.0. The example can be completed using v5.0, however some functionality may not exist in v5.0 and the format of certain input files may differ slightly.

Examples
========

**Mag3d v6.0**

Here, the program libraries for Mag3d v6.0 will be used to:

    - define a magnetic susceptibility model on a tensor mesh
    - predict total magnetic intensity data for a synthetic susceptibility model
    - generate sensitivity weights for the inverse problem
    - compute and store the sensitivity matrix
    - invert total magnetic intensity data to recover a susceptibility model


Zip folders containing all necessary files can be downloaded here:

    - `Files for mag3d v6 example <https://github.com/ubcgif/mag3d/raw/master/assets/mag3d_example.zip>`__


The full example is parsed into 5 sections:

.. toctree::
    :maxdepth: 1

    Create tensor model <example/create_model>
    Forward modeling <example/fwd>
    Weights <example/weights>
    Generate sensitivity matrix <example/sensitivity>
    Inversion <example/inv>


**Mag3d v5.0**

A legacy example using the Mag3d v5.0 package can be found here:

.. toctree::
	Legacy examples <example/old_example>

