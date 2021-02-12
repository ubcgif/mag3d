.. _example_tmi_fwd:

Forward Modeling
================

Here, the code **magfor3d_60.exe** is used to predict the data for an airborne survey. For simplicity, the magnetic data are predicted on regular grid of data points with a horizontal spacing of 40 m in both the Easting and Northing directions. The flight height is 30 m for all data points. The Earth's field had an inclination of 65 degrees, a declination of 25 degrees and an intensity of 50,000 nT. Files relevant to this part of the example are in the sub-folder *fwd*. For this example, we use the model that were created in the example ":ref:`create model<example_tmi_model>`".

Before running this example, you may want to do the following:

	- `Download and open the zip folder containing the entire Mag3d example <https://github.com/ubcgif/mag3d/raw/v6/assets/mag3d_v6_tmi_example.zip>`__ (if not done already)
	- :ref:`Learn how to run code from command line <mag3d_fwd>`
	- forward modeling does not required and input file


The total magnetic intensity data output by the simulation is shown below.


.. figure:: images/dpred.png
     :align: center
     :width: 700



