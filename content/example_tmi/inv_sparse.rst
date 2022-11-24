.. _example_tmi_inv_sparse:

.. note:: The latest example has been generated using MAG3D v6.0.2. The exercise can be completed using previous versions. However some functionality has been added since v5.0, and improvements in performance since v6.0 and v6.0.1 may result in slightly different recovered models.

Sparse Norm Inversion
=====================

Here, we use **magsen3d_60.exe** to compute the sensitivity matrix required for the inversion; which is scaled by distance weighting. We then use **maginv3d_60.exe** to invert the data using sparse norms to recover a model compact susceptibility model. To keep the example simple, we added Gaussian noise with a standard deviation of 0.5 nT to all data points. We then assigned uncertainties of 0.5 nT to all magnetic data. In practice, the noise on the data is not trivial to quantify and choosing appropriate uncertainties is very important for successful inversion.

Files relevant to this part of the example are in the sub-folder *inv_sparse* . Before running this example, you may want to do the following:

    - `Download and open the zip folder containing the entire Mag3d example <https://github.com/ubcgif/mag3d/raw/v6/assets/mag3d_v6_amp_example.zip>`__ (if not done already)
    - Learn how to run :ref:`magsen3d_60.exe <mag3d_sens>` and :ref:`maginv3d_60.exe <mag3d_inv>` from the command line
    - Learn the format of the :ref:`input file for magsen3d_60.exe <mag3d_sens_input>` and of the :ref:`input file for maginv3d_60.exe <mag3d_inv_input>`

Sensitivities
-------------

.. note:: If using MAG3D v6.0.2, the last line in the input file is unused and the sensitivity matrix can be used for the L2 inversion can be reused for sparse-norm inversion.

Here, the code **magsen3d_60.exe** and the input file **sens.inp** (:ref:`see format <mag3d_sens_input>` ) are used to construct the sensitivity matrix and scale it using distance weighting. The distance weighting is applied to the sensitivity matrix to counteract the inversion's natural tendancy to incorrectly place anomalous structures near the observation locations. 

To compute the sensitivities, the following input file was used. Since we are no longer performed an least-squares inversion, a flag of *0* must be entered on the last line of the input file.

.. figure:: ../inputfiles/images/create_sens_sparse_input.png
     :align: center
     :width: 700


Inversion
---------

Here, the code **maginv3d_60.exe** and the input file **inv.inp** (:ref:`see format<mag3d_inv_input>` ) was used to recover a susceptibility model. You cannot perform the inversion until you have created the sensitivity matrix. For this example, we set *P=0* and *Qx=Qy=Qz=2*. That is, we would like to recover a model that is compact but still smooth. To see how these parameters impact the recovered model, see the `GIFtools cookbook <https://giftoolscookbook.readthedocs.io/en/latest/content/fundamentals/Norms.html>`__ .


.. figure:: ../inputfiles/images/create_inv_sparse_input.png
     :align: center
     :width: 700

The true model (left), recovered model using least-squares (middle) and recovered model using sparse norms (right) are shown below. Unlike the least-squares result, the sparse norm result is a compact structure whose maximum amplitude is much closer to that of the true model. And the distance weighting is able to place the center of the recovered model at the correct depth.


.. figure:: images/final_model_sparse.png
     :align: center
     :width: 700



