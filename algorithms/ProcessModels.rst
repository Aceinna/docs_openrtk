****************
Process Models
****************

.. sectionauthor:: Joseph S Motyka <jmotyka at aceinna.com>


Introduction
=============

As the state-transition model is nonlinear, the state-transition vector cannot be directly used to
propagate the covariance forward in time.  Instead the state-transition vector, :math:`\vec{f}`, is
linearized based on the current system states and used for this task.  The resulting linearization
(computed from the partial derivatives of :math:`\vec{f}` with respect to the system states,
:math:`\vec{x}`) generates a matrix referred to as the Process Jacobian, :math:`F`.  This matrix is
used to propagate the covariance, :math:`P`, forward in time.

The covariance estimate is also affected by the process noise, which is related to sensor-noise
levels.  The more process noise that exists in a system, the larger the covariance estimate will be
at the next time step.  This noise is reflected in the process-noise covariance matrix, :math:`Q`.

Formulation of these matrices are described in the following sections.


.. contents:: Contents
    :local:

.. role::  raw-html(raw)
    :format: html


Individual Process Models
===========================

.. toctree::
    :maxdepth: 3

    Process_Jacobian
    Process_Covariance


.. links-placeholder
