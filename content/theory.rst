Background theory
=================

Introduction
------------

This manual presents theoretical background, numerical examples, and explanation for implementing the program library MAG3D. This suite of algorithms, developed at the UBC-Geophysical Inversion Facility, is used to invert magnetic responses over a three-dimensional susceptibility distribution. The manual is designed so that a geophysicist who is familiar with the magnetic experiment, but who is not necessarily versed in the details of inverse theory, can use the codes and invert his or her data.

A magnetic experiment involves measuring the anomalous magnetic field produced by magnetically susceptible materials beneath the surface, which have been magnetized by the earth's main magnetic field. The material with susceptibility :math:`\kappa(x,y,z)` is magnetized when the earth's main field impinges upon the subsurface formation with flux intensity :math:`\mathbf{B}_o`. The magnetized material gives rise to a magnetic field, :math:`\mathbf{B}_a`, which is superimposed on the inducing field to produce a total, or resultant, field. By measuring the resultant field and removing the inducing field from the measurements through numerical processing, one obtains the distribution of the anomalous field due to the susceptible material. In this program library we make the assumption that no remanent magnetization is present and restrict our attention to induced magnetization.

The data from a typical magnetic survey are a set of magnetic field measurements acquired over a 2D grid above the surface or along a number of boreholes within the volume of interest. These data are first processed to yield an estimate of the anomalous field due to the susceptible material in the area. The goal of the magnetic inversion is to obtain, from the extracted anomaly data, quantitative information about the distribution of the magnetic susceptibility in the ground. Thus, it is assumed that the input data to the inversion program is the extracted residual anomaly and the programs in the library are developed accordingly.

Forward modelling
-----------------

General formulation
~~~~~~~~~~~~~~~~~~~

For a given inducing field :math:`\mathbf{B}_o`, the magnetization :math:`\mathbf{J}` depends upon the susceptibility through a differential equation. In most mineral exploration cases, the actual susceptibility is very small. Thus, we use the first order approximation, where the magnetization is proportional to the susceptibility and is given by the product of susceptibility and the inducing magnetic field :math:`\mathbf{H}_o`, 

.. math::
   \mathbf{J}=\kappa\mathbf{H}_o,
   :label: magnetization


where :math:`\mathbf{H}_o=\mathbf{B}_o / \mu_o` and :math:`\mu_o` is the free-space magnetic permeability. This ignores the self-demagnetization effect by which the secondary field reduces the total inducing field within the susceptible region and results in a weaker magnetization than that given by equation :eq:`magnetization`.

The anomalous field produced by the distribution of magnetization :math:`\mathbf{J}` given by the following integral equation with a dyadic Green's function:

.. math::
   \mathbf{B}_a(\mathbf{r})=\frac{\mu_0}{4\pi}\int\limits_V \nabla \nabla \frac{1}{\left |\mathbf{r}-\mathbf{r}_o\right |}\cdot\mathbf{J} dv,
   :label: greensf


where :math:`\mathbf{r}` is the position of the observation point and :math:`V` represents the volume of magnetization at its position, :math:`\mathbf{r}_o`. The above equation is valid for observation locations above the earth's surface. We assume the magnetic permeability is :math:`\mu_o` at borehole locations to allow equation :eq:`greensf` to be valid everywhere.

When the susceptibility is constant within a volume of source region, the above equation can be written in matrix form as:

.. math::
   \mathbf{B}_a=\mu_o\left( \begin{array}{ccc}
   T_{11} & T_{12} & T_{13} \\
   T_{21} & T_{22} & T_{23}\\
   T_{31} & T_{32} & T_{33}
   \end{array} \right)\kappa\mathbf{H}_o\equiv \mu_o\kappa\mathbf{T}\mathbf{H}_o.
   :label: bmatrix


The tensor :math:`\mathbf{T}_{ij}` is given by

.. math::
    \mathbf{T}_{ij}=\frac{1}{4\pi}\int\limits_V\frac{\partial}{\partial x_i}\frac{\partial}{\partial x_j}\frac{1}{\left |\mathbf{r}-\mathbf{r}_o\right |}dv, \mbox{  for }i=1,3 ; j=1,3,
    :label: tij

   
and where :math:`x_1`, :math:`x_2`, and :math:`x_3` represent :math:`x-, y-`, and :math:`z-`\ directions, respectively. The expressions of :math:`\mathbf{T}_{ij}` for a cuboidal source volume can be found in :cite:`Bhattacharyya64` and :cite:`Sharma66`. Since :math:`\mathbf{T}` is symmetric and its trace is equal to :math:`-1` when the observation is inside the cell and is :math:`0` when the observation is outside the cell, only five independent elements need to be calculated.

Once :math:`\mathbf{T}` is formed, the magnetic anomaly :math:`\mathbf{B}_a` and its projection onto any direction of measurement are easily obtained by the inner product with the directional vector. The projection of the field :math:`\mathbf{B}_a` onto different directions yields different anomalies commonly obtained in the magnetic survey. For instance, the vertical anomaly is simply :math:`B_{az}`, the vertical component of :math:`\mathbf{B}_a`, whereas the total field anomaly is, to first order, the projection of :math:`\mathbf{B}_a` onto the direction of the inducing field :math:`\mathbf{B}_o`.

Borehole data
~~~~~~~~~~~~~

In a borehole experiment, the three components are measured in the directions of local coordinate axes (:math:`l_1`, :math:`l_2`, :math:`l_3`) defined according to the borehole orientation. Assuming that the borehole dip :math:`\theta` is measured downward from the horizontal surface and azimuth :math:`\varphi` is measured eastward from the North; a commonly used convention has the :math:`l_3`-axis pointing downward along borehole, :math:`l_1`-axis pointing perpendicular to the borehole in the direction of the azimuth. The :math:`l_2`-axis completes the right-handed coordinate system and is :math:`90^\circ` clockwise from the azimuth and perpendicular to the borehole. Based upon the above definition the rotation matrix that transforms three components of a vector in the global coordinate system to the components in the local coordinates is given by

.. math::
   \mathbf{R}=\left( \begin{array}{ccc}
   \cos\varphi \sin\theta & \sin\varphi \sin\theta & -\cos\theta \\
   -\sin\varphi & \cos\varphi & 0\\
   \cos\varphi \cos\theta & \sin\varphi \cos\theta & \sin\theta
   \end{array} \right)
   :label: rotation
  

If a vector is defined in local coordinates as :math:`(l_1,l_2,l_3)^T`, and in global coordinates as :math:`(g_1,g_2,g_3)^T`, then the following two relations hold:

.. math::
   \begin{aligned}
   (l_1,l_2,l_3)^T = \mathbf{R}(g_1,g_2,g_3)^T \nonumber \\
   (g_1,g_2,g_3)^T = \mathbf{R}^T(l_1,l_2,l_3)^T\end{aligned}
   :label: vectors

The rotation matrix :math:`\mathbf{R}` therefore allows measured components in local coordinates to be rotated into global coordinate, or the components of the regional field to be rotated into local coordinates for use in regional removal.

Numerical implementation of forward modelling
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We divide the region of interest into a set of 3D prismatic cells by using a 3D orthogonal mesh and assume a constant susceptibility within each cell. By equation :eq:`magnetization`, we have a uniform magnetization within each cell and its field anomaly can be calculated using equations :eq:`bmatrix` and :eq:`vectors`. The actual anomaly that would be measured at an observation point is the sum of field produced by all cells having a non-zero susceptibility value. The calculation involves the evaluation of equation :eq:`bmatrix` in a 3D rectangular domain define by each cell. The program that performs this calculation is MAGSEN3D. As input parameters, the coordinates of the observation points and the inclination and declination of the anomaly direction must be specified for each datum. For generality, each component in a multi-component data set is specified as a separate datum with its own location and direction of projection.

.. _invMethod:

Inversion methodology
---------------------

Let the set of extracted anomaly data be :math:`\mathbf{d} = (d_1,d_2,...,d_N)^T` and the susceptibility of cells in the model be :math:`\kappa = (\kappa_1,\kappa_2,...,\kappa_M)^T`. The two are related by the sensitivity matrix

.. math::
   \mathbf{d}=\mathbf{G}{\kappa}.
   :label: sens

The matrix has elements :math:`g_{ij}` which quantify the contribution to the :math:`i^{th}` datum due to a unit susceptibility in the :math:`j^{th}` cell. The program performs the calculation of the sensitivity matrix, which is to be used by the subsequent inversion. The sensitivity matrix provides the forward mapping from the model to the data during the entire inverse process. We will discuss its efficient representation via the wavelet transform in a separate section.

The first question that arises in the inversion of magnetic data concerns definition of the "model". We choose magnetic susceptibility :math:`\kappa` as the model for since the anomalous field is directly proportional to the susceptibility. The inverse problem is formulated as an optimization problem where a global objective function, :math:`\phi`, is minimized subject to the constraints in equation :eq:`sens`. The global objective functions consists of two components: a model objective function, :math:`\phi_m`, and a data misfit function, :math:`\phi_d`, such that

.. math::
   \begin{aligned}
   \min \phi = \phi_d+\beta\phi_m \\
   \mbox{s. t. } \kappa^l\leq \kappa \leq \kappa^u, \nonumber\end{aligned}
   :label: globphi


where :math:`\beta` is a trade off parameter that controls the relative importance of the model smoothness through the model objective function and data misfit function. When the standard deviations of data errors are known, the acceptable misfit is given by the expected value :math:`\phi_d` and we will search for the value of :math:`\beta` via an L-curve criterion :cite:`Hansen00` that produces the expected misfit. Otherwise, a user-defined :math:`\beta` value is used. Bound are imposed through the projected gradient method so that the recovered model lies between imposed lower (:math:`\kappa^l`) and upper (:math:`\kappa^u`) bounds. 

We next discuss the construction of a model objective function which, when minimized, produces a model that is geophysically interpretable. This function gives the flexibility to incorporate as little or as much information as possible. At the minimum, it drives the solution towards a reference model :math:`\kappa_o` and requires that the model be relatively smooth in the three spatial directions. Here we adopt a right handed Cartesian coordinate system with positive north and positive down. Let the model objective function be

.. _mof:
.. math::
   \begin{aligned}
   \phi_m(\kappa) &=& \alpha_s\int\limits_V w_s\left\{w(\mathbf{r})[\kappa(\mathbf{r})-{\kappa}_o] \right\}^2dv + \alpha_x\int\limits_V w_x \left\{\frac{\partial w(\mathbf{r})[\kappa(\mathbf{r})-{\kappa}_o]}{\partial x}\right\}^2dv \\ \nonumber
   &+& \alpha_y\int\limits_V w_y\left\{\frac{\partial w(\mathbf{r})[\kappa(\mathbf{r})-{\kappa}_o]}{\partial y}\right\}^2dv +\alpha_z\int\limits_V\ w_z\left\{\frac{\partial w(\mathbf{r})[\kappa(\mathbf{r})-{\kappa}_o]}{\partial z}\right\}^2dv,\end{aligned}
   :label: mof

where the functions :math:`w_s`, :math:`w_x`, :math:`w_y` and :math:`w_z` are spatially dependent, while :math:`\alpha_s`, :math:`\alpha_x`, :math:`\alpha_y` and :math:`\alpha_z` are coefficients, which affect the relative importance of different components in the objective function. The reference model is given as :math:`\kappa_o` and :math:`w(\mathbf{r})` is a generalized depth weighting function. The purpose of this function is to counteract the geometrical decay of the sensitivity with the distance from the observation location so that the recovered susceptibility is not concentrated near the observation locations. It should be noted that although traditionally the depth weighting is applied through the model objective function, practically applies it to the sensitivity matrix prior to compression, increasing the effectiveness of the wavelet transform. The details of the depth weighting function will be discussed in the next section.

The objective function in equation :eq:`mof` has the flexibility to incorporate many types of prior knowledge into the inversion. The reference model may be a general background model that is estimated from previous investigations or it will be a zero model. The reference model would generally be included in the first component of the objective function but it can be removed, if desired, from the remaining terms; often we are more confident in specifying the value of the model at a particular point than in supplying an estimate of the gradient. The choice of whether or not to include :math:`\kappa_o` in the derivative terms can have significant effect on the recovered model as shown through the synthetic example (section [RefModSection]). The relative closeness of the final model to the reference model at any location is controlled by the function :math:`w_s`. For example, if the interpreter has high confidence in the reference model at a particular region, he can specify :math:`w_s` to have increased amplitude there compared to other regions of the model, thus favouring a model near the reference model in those locations. The weighting functions :math:`w_x`, :math:`w_y`, and :math:`w_z` can be designed to enhance or attenuate gradients in various regions in the model domain. If geology suggests a rapid transition zone in the model, then a decreased weighting on particular derivatives of the model will allow for higher gradients there and thus provide a more geologic model that fits the data. 

Numerically, the model objective function in equation eq:`mof` is discretized onto the mesh defining the susceptibility model using a finite difference approximation. This yields: 

.. math::
    \begin{aligned}
    \phi_m({\kappa}) = ({\kappa}-{\kappa}_o)^T(\alpha_s \mathbf{W}_s^T\mathbf{W}_s+\alpha_x \mathbf{W}_x^T\mathbf{W}_x+\alpha_y \mathbf{W}_y^T\mathbf{W}_y+\alpha_z \mathbf{W}_z^T\mathbf{W}_z)({\kappa}-{\kappa}_o), \nonumber\\
    \equiv({\kappa}-{\kappa}_o)^T\mathbf{W}_m^T\mathbf{W}_m({\kappa}-{\kappa}_o), \nonumber\\
    =\left \| \mathbf{W}_m({\kappa}-{\kappa}_o) \right \|^2,\end{aligned}
    :label: modobjdiscr

where :math:`\mathbf{m}` and :math:`\mathbf{m}_o` are :math:`M`-length vectors representing the recovered and reference models, respectively. Similarly, there is an option to remove to the reference model from the spatial derivatives in equation :eq:`modobjdiscr` such that

.. math::
    \begin{aligned}
    \phi_m({\kappa}) = ({\kappa}-{\kappa}_o)^T(\alpha_s \mathbf{W}_s^T\mathbf{W}_s)({\kappa}-{\kappa}_o) + {\kappa}^T(\alpha_x \mathbf{W}_x^T\mathbf{W}_x+\alpha_y \mathbf{W}_y^T\mathbf{W}_y+\alpha_z \mathbf{W}_z^T\mathbf{W}_z){\kappa}, \nonumber \\
    \equiv ({\kappa}-{\kappa}_o)^T\mathbf{W}_s^T\mathbf{W}_s({\kappa}-{\kappa}_o) + {\kappa}^T\mathbf{W}_m^T\mathbf{W}_m{\kappa}, \nonumber\\
    =\left \| \mathbf{W}_s({\kappa}-{\kappa}_o) + \mathbf{W}_m{\kappa}\right \|^2.\end{aligned}
    :label: modobjdiscrOut


In the previous two equations, the individual matrices :math:`\mathbf{W}_s`, :math:`\mathbf{W}_x`, :math:`\mathbf{W}_y`, and :math:`\mathbf{W}_z` are straight forward to calculated once the model mesh and the weighting functions :math:`w(\mathbf{r})` and :math:`w_s` , :math:`w_x`, :math:`w_y`, :math:`w_z` are defined. The cumulative matrix :math:`\mathbf{W}_m^T\mathbf{W}_m` is then formed for the chosen configuration.

The next step in setting up the inversion is to define a measure of how well the observed data are reproduced. Here we use the :math:`l_2`-norm measure

.. math::
    \begin{aligned}
    \phi_d = \left\| \mathbf{W}_d(\mathbf{G}\kappa-\mathbf{d})\right\|^2.\end{aligned}
    :label: phid

For the work here, we assume that the contaminating noise on the data is independent and Gaussian with zero mean. Specifying :math:`\mathbf{W}_d` to be a diagonal matrix whose :math:`i^{th}` element is :math:`1/\sigma_i`, where :math:`\sigma_i` is the standard deviation of the :math:`i^{th}` datum makes :math:`\phi_d` a chi-squared distribution with :math:`N` degrees of freedom. The optimal data misfit for data contaminated with independent, Gaussian noise has an expected value of :math:`E[\chi^2]=N`, providing a target misfit for the inversion. We now have the components to solve the inversion as defined in equation :eq:`globphi`.

To solve the optimization problem when constraints are imposed we use the projected gradients method :cite:`CalamaiMore87,Vogel02`. This technique forces the gradient in the Krylov sub-space minimization (in other words a step during the conjugate gradient process) to zero if the proposed step would make a model parameter exceed the bound constraints. The result is a model that reaches the bounds, but does not exceed them. This method is computationally faster than the log-barrier method because (1) model parameters on the bounds are neglected for the next iteration and (2) the log-barrier method requires the calculation of a barrier term. Previous versions of MAG3D used the logarithmic barrier method :cite:`Wright97,NocedalWright99`.

The weighting function is generated by the program that is in turn given as input to the sensitivity generation program MAGSEN3D. This gives the user full flexibility in using customized weighting functions. This program allows user to specify whether to use a generalized depth weighting or a distance-based weighting that is useful in regions of largely varying topography. Distance weighting must be used when borehole data are present.

Depth Weighting and Distance Weighting
--------------------------------------

It is a well-known fact that static magnetic data have no inherent depth resolution. A numerical consequence of this is that when an inversion is performed, which minimizes :math:`\int m(\mathbf{r})^2 dv`, subject to fitting the data, the constructed susceptibility is concentrated close to the observation locations. This is a direct manifestation of the kernel's decay with the distance between the cell and observation locations. Because of the rapidly diminishing amplitude, the kernels of magnetic data are not sufficient to generate a function that possess significant structure at locations that are far away from observations. In order to overcome this, the inversion requires a weighting to counteract this natural decay. Intuitively, such a weighting will be the inverse of the approximate geometrical decay. This gives cells at all locations equal probability to enter into the solution with a non-zero susceptibility.


.. _depthWeight:

Depth weighting for surface or airborne data
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The sensitivity decays predominantly as a function of depth for surface data. Numerical experiments indicate that a function of the form :math:`(z+z_o)^{-3}` closely approximates the kernel's decay directly under the observation point provided that a reasonable value is chosen for :math:`z_o`. The value of 3 in the exponent is consistent with the fact that, to first order, a cuboidal cell acts like a dipole source whose field decays as inverse distance cubed. The value of :math:`z_o` can be obtained by matching the function 1/\ :math:`(z+z_o)^3` with the field produced at an observation point by a column of cells. Thus we use a depth weighting function of the form

.. math:: w(\mathbf{r}_j)=\left[\frac{1}{\Delta z_{j}}\int\limits_{\Delta z_{ij}}\frac{dz}{(z+z_o)^\alpha}\right]^{1/2}, ~~ j=1,...,M.
     :label: depthw

For the inversion of surface data, where :math:`\alpha=3`, :math:`\mathbf{r}_j` is used to identify the :math:`j^{th}` cell, and :math:`\Delta z_j` is its thickness. This weighting function is normalized so that the maximum value is unity. Numerical tests indicate that when this weighting is used, the susceptibility model constructed by minimizing the model objective function in equation :eq:`mof`, subject to fitting the data, places the recovered anomaly at approximately the correct depth.

If the data set involves highly variable observation heights the normal depth weighting function might not be most suitable. Distance weighting used for borehole data may be more appropriate as explained in the next section.

.. _distWeight:

Distance weighting for borehole data
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

For data sets that contain borehole measurements, the sensitivities do not have a predominant decay direction, therefore a weighting function that varies in three dimensions is needed. We generalize the depth weighting used in surface data inversion to form such a 3D weighting function called distance weighting: 

.. math::
      w(\mathbf{r}_j)=\frac{1}{\sqrt{\Delta V_{j}}} \left\{\sum_{i=1}^{N}\left[\int\limits_{\Delta V_{j}}\frac{dv}{(R_{ij}+R_o)^\alpha}\right]^{2}\right\}^{1/4}, ~~j=1,...,M,
      :label: distw

where :math:`\alpha=3`, :math:`V_j` is the volume of :math:`j^{th}` cell, :math:`R_{ij}` is the distance between a point within the source volume and the :math:`i^{th}` observation, and :math:`R_o` is a small constant used to ensure that the integral is well-defined (chosen to be a quarter of the smallest cell dimension). This weighting function is also normalized to have a maximum value of unity. For inversion of borehole data, it is necessary to use this more general weighting. This weighting function is also advantageous if surface data with highly variable observation heights are inverted.

.. _waveletSection:

Wavelet Compression of Sensitivity Matrix
-----------------------------------------

The two major obstacles to the solution of a large-scale magnetic inversion problem are the large amount of memory required for storing
the sensitivity matrix and the CPU time required for the application of the sensitivity matrix to model vectors. This program library overcomes these difficulties by forming a sparse representation of the sensitivity matrix using a wavelet transform based on compactly supported, orthonormal wavelets. For more details, the users are referred to :cite:`LiOldenburg03,LiOldenburg10`. Here, we give a brief description of the method necessary for the use of the MAG3D library.

Each row of the sensitivity matrix in a 3D magnetic inversion can be treated as a 3D image and a 3D wavelet transform can be applied to it. By the properties of the wavelet transform, most transform coefficients are nearly or identically zero. When coefficients of small magnitudes are discarded (the process of thresholding), the remaining coefficients still contain much of the necessary information to reconstruct the sensitivity accurately. These retained coefficients form a sparse representation of the sensitivity in the wavelet domain. The need to store only these large coefficients means that the memory requirement is reduced. Further, the multiplication of the sensitivity with a vector can be carried out by a sparse multiplication in the wavelet domain. This greatly reduces the CPU time. Since the matrix-vector multiplication constitutes the core computation of the inversion, the CPU time for the inverse solution is reduced accordingly. The use of this approach increases the size of solvable problems by nearly two orders of magnitude.

Let :math:`\mathbf{G}` be the sensitivity matrix and :math:`\mathcal{W}` be the symbolic matrix-representation of the 3D wavelet transform. Then applying the transform to each row of :math:`\mathbf{G}` and forming a new matrix consisting of rows of transformed sensitivity is equivalent to the following operation:

.. math::
   \widetilde{\mathbf{G}}=\mathbf{G}\mathcal{W}^T,
   :label: senswvt

where :math:`\widetilde{\mathbf{G}}` is the transformed matrix. The thresholding is applied to individual rows of :math:`\mathbf{G}` by the following rule to form the sparse representation :math:`\widetilde{\mathbf{G}}^S`,

.. math::
   \widetilde{g}_{ij}^{s}=\begin{cases}
   \widetilde{g}_{ij} & \mbox{if } \left|\widetilde{g}_{ij}\right| \geq \delta _i \\
   0 & \mbox{if } \left|\widetilde{g}_{ij}\right| < \delta _i
   \end{cases}, ~~ i=1,\ldots,N,
   :label: elemg

where :math:`\delta _i` is the threshold level, and :math:`\widetilde{g}_{ij}` and :math:`\widetilde{g}_{ij}^{s}` are the elements of :math:`\widetilde{\mathbf{G}}` and :math:`\widetilde{\mathbf{G}}^S`, respectively. The threshold level :math:`\delta _i` are determined according to the allowable error of the reconstructed sensitivity, which is measured by the ratio of norm of the error in each row to the norm of that row, :math:`r_i(\delta_i)`. It can be evaluated directly in the wavelet domain by the following expression:

.. math::
    r_i(\delta_i)=\sqrt{\frac{\underset{\left | {\widetilde{g}_{ij}} \right| <\delta_i}\sum{\widetilde{g}_{ij}}^2}{\underset{j}\sum{\widetilde{g}_{ij}^2}}}, ~~i=1,\ldots,N,
    :label: rhoi

Here the numerator is the norm of the discarded coefficients and the denominator is the norm of all coefficients. The threshold level :math:`\delta_{i_o}` is calculated on a representative row, :math:`i_o`. This threshold is then used to define a relative threshold :math:`\epsilon =\delta_{i_{o}}/ \underset{j}{\max}\left | {\widetilde{g}_{ij}} \right |`. The absolute threshold level for each row is obtained by

.. math::
   \delta_i = \epsilon \underset{j}{\max}\left | {\widetilde{g}_{ij}} \right|, ~~i=1,\ldots,N.
   :label: deltai

The program that implements this compression procedure is MAGSEN3D. The user is asked to specify the relative error :math:`r^*` and the program will determine the relative threshold level :math:`\delta_i`. Usually a value of a few percent is appropriate for :math:`r^*`. When both surface and borehole data are present, two different relative threshold levels are calculated by choosing a representative row for surface data and another for borehole data. For experienced users, the program also allows the direct input of the relative threshold level.
