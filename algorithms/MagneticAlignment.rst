Magnetic-Alignment
===================

.. contents:: Contents
    :local:

.. role::  raw-html(raw)
    :format: html

**Overview**

  A so-called “magnetic-alignment” procedure enables estimation of the hard and soft-iron disturbances
  in the system.  As these disturbances are fixed in the body, the corrections must be applied in the
  body-frame.  The procedure works as follows:

      1) The magnetic-field is measured and recorded as the system undergoes a 360+ degree rotation
        about the z-axis.  Ideally this is done when the system is level.

      2) Upon completion, an algorithm determines the ellipse that best fits the distorted circle.

      3) Ellipse parameters (related to the hard and soft-iron disturbances) are saved in the firmware
        and used to correct the magnetic-field measurements.


  In most cases an ellipse describes magnetic-field distortions quite well.  The ellipse parameters
  relate to the magnetic disturbances as follows:

      * The center of the ellipse is equal to the hard-iron values

      * The angle the major-axis of the ellipse makes with a nominal x-axis is equal to the soft-iron
        angle (which forms the matrix :math:`R_{SI}`)

      * The major and minor-axis lengths forms the scaling matrix :math:`S_{SI}`


  The formula for the corrected magnetic measurements works by:

      1) Centering the ellipse by removing the hard-iron bias from the measurements

      2) Rotating the ellipse to align with the nominal x and y-axes

      3) Stretching the ellipse along its major and minor-axes to form a unit-circle

      4) Rotating the unit-circle back into its nominal orientation

  Note: as mentioned earlier, this correction is only done in the XY-plane and cannot correct the raw
  magnetometer signal.  It is only done to determine the system heading.

**Example**

  Magnetic-field information was collected as the system underwent a 360 degree rotation about the z-
  axis (*Figure*).  This was performed twice, once in a disturbance-free environment (no iron added
  to the system) and once with additional iron added to the system. The data in each case was
  processed and a best-fit ellipse FN computed (dashed lines).  In the disturbance-free case, the data
  and the fit were close to circular.  In the case with additional iron, however, the circle was
  clearly distorted and shifted away from the origin.


**Magnetic-Field Measurement in an Environment with and without Iron-Based Disturbances**


  For the measurements taken in the presence of additional iron, the estimation procedure produced the
  following best-fit ellipse parameters:

**Best-Fit Ellipse Parameters**

  +--------------------------+---------------+----------+
  | **Ellipse Parameter**    | **Value**     | **Unit** |
  +==========================+===============+==========+
  |                          |               |          |
  | *Center*                 | -0.128, 0.126 | [G]      |
  |                          |               |          |
  +--------------------------+---------------+----------+
  |                          |               |          |
  | *Major/Minor axes*       | 0.225, 0.198  | [G]      |
  |                          |               |          |
  +--------------------------+---------------+----------+
  |                          |               |          |
  | *Soft-Iron Scale Factor* | 0.882         | [N/A]    |
  |                          |               |          |
  +--------------------------+---------------+----------+
  |                          |               |          |
  | *Angle to Major-Axis*    | -48.497       | [deg]    |
  |                          |               |          |
  +--------------------------+---------------+----------+


  In the correction equation (above), :math:`R_{SI}` is the rotation matrix and corrects for a
  rotation of the magnetic-field due to soft-iron effects:

  .. math::

      R_{SI} = \begin{bmatrix} { { \begin{split} cos{ \begin{pmatrix} { \eta } \end{pmatrix} }
                                  sin{ \begin{pmatrix} { \eta } \end{pmatrix} }
                                  1
                                  \end{split}
                                } \hspace{5mm}
                                { \begin{split} -sin{ \begin{pmatrix} { \eta } \end{pmatrix} }
                                  cos{ \begin{pmatrix} { \eta } \end{pmatrix} }
                                  1
                                  \end{split}
                                } \hspace{5mm}
                                { \begin{split} 0
                                  0
                                  1
                                  \end{split}
                                }
              } \end{bmatrix}


  Where :math:`\eta` is the angle from the nominal x-axis to the semi-major axis.  :math:`S_{SI}` (the
  scale-factor matrix) corrects for the stretching caused by the soft-iron:

  .. math::

      S_{SI} = \begin{bmatrix} { { \begin{split} {1/a}
                                  0
                                  0
                                  \end{split}
                                } \hspace{5mm}
                                { \begin{split} 0
                                  {1/b}
                                  0
                                  \end{split}
                                } \hspace{5mm}
                                { \begin{split} 0
                                  0
                                  1
                                  \end{split}
                                }
              } \end{bmatrix}


  :math:`a` and :math:`b` are the lengths of the semi-major and semi-minor axes.

  For the data-set described above, the values for :math:`R_{SI}` and :math:`S_{SI}`, resulting from
  the best-fit ellipse parameters, are:

  .. math::

      R_{SI} = \begin{bmatrix} { { \begin{split} {0.66266}
                                  {-0.74892}
                                  0
                                  \end{split}
                                } \hspace{5mm}
                                { \begin{split} {0.74892}
                                  {0.66266}
                                  0
                                  \end{split}
                                } \hspace{5mm}
                                { \begin{split} 0
                                  0
                                  1
                                  \end{split}
                                }
              } \end{bmatrix}

  and

  .. math::

      S_{SI} = \begin{bmatrix} { { \begin{split} {4.45226}
                                  0
                                  0
                                  \end{split}
                                } \hspace{5mm}
                                { \begin{split} 0
                                  {5.04689}
                                  0
                                  \end{split}
                                } \hspace{5mm}
                                { \begin{split} 0
                                  0
                                  1
                                  \end{split}
                                }
              } \end{bmatrix}


  Applying these correction factors to the raw magnetic-field measurements results in the unit-circle
  shown in *Figure*.

**Corrected Magnetic Field Readings**

  Note: the nodes located at 45 degree increments around the circle are points where additional data
  was collected to test the heading calculation (described in the next section).


**Results**

  *Table* lists the heading computed from test data using the above equations relating heading to
  corrected magnetic-field.

  **Heading Results from Magnetically Clean and Distorted Readings**

  .. tabularcolumns:: |c|c|c|c|c|


  +-------------------+-------------------+-----------------+-------------------+-----------------+
  |                   | **Disturbance-Free Data**           | **Data with Added Iron Source**     |
  || **True Heading** +-------------------+-----------------+-------------------+-----------------+
  || **[deg]**        | **Heading [deg]** | **Error [deg]** | **Heading [deg]** | **Error [deg]** |
  +===================+===================+=================+===================+=================+
  |                   |                   |                 |                   |                 |
  | 0                 | 359.69            | -0.31           | 0.013             | 0.013           |
  |                   |                   |                 |                   |                 |
  +-------------------+-------------------+-----------------+-------------------+-----------------+
  |                   |                   |                 |                   |                 |
  | 45                | 45.19             | 0.19            | 44.82             | -0.18           |
  |                   |                   |                 |                   |                 |
  +-------------------+-------------------+-----------------+-------------------+-----------------+
  |                   |                   |                 |                   |                 |
  | 90                | 89.96             | -0.04           | 90.15             | 0.15            |
  |                   |                   |                 |                   |                 |
  +-------------------+-------------------+-----------------+-------------------+-----------------+
  |                   |                   |                 |                   |                 |
  | 135               | 135.05            | 0.05            | 135.08            | 0.08            |
  |                   |                   |                 |                   |                 |
  +-------------------+-------------------+-----------------+-------------------+-----------------+
  |                   |                   |                 |                   |                 |
  | 180               | 180.57            | 0.57            | 180.68            | 0.68            |
  |                   |                   |                 |                   |                 |
  +-------------------+-------------------+-----------------+-------------------+-----------------+
  |                   |                   |                 |                   |                 |
  | 225               | 225.64            | 0.64            | 225.62            | 0.62            |
  |                   |                   |                 |                   |                 |
  +-------------------+-------------------+-----------------+-------------------+-----------------+
  |                   |                   |                 |                   |                 |
  | 270               | 270.63            | 0.63            | 270.48            | 0.48            |
  |                   |                   |                 |                   |                 |
  +-------------------+-------------------+-----------------+-------------------+-----------------+
  |                   |                   |                 |                   |                 |
  | 315               | 315.30            | 0.30            | 315.09            | 0.09            |
  |                   |                   |                 |                   |                 |
  +-------------------+-------------------+-----------------+-------------------+-----------------+
  |                   |                   |                 |                   |                 |
  | 360               | 359.79            | -0.21           | 0.10              | 0.10            |
  |                   |                   |                 |                   |                 |
  +-------------------+-------------------+-----------------+-------------------+-----------------+


  Note: the raw results reported a systematic error of approximately 2.0 degrees on all heading
  values.  This was due to a misalignment of the test-fixture relative to true-north.  The values
  presented in *Table* reflect this 2.0 degree correction.  The systematic error is visible in
  Figures with data-clusters that do not fall on the x and y-axes.
