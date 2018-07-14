****************
Process Models
****************

.. sectionauthor:: Joseph S Motyka <jmotyka at aceinna.com>


Introduction
=============

As the state-transition model is nonlinear, the state-transition vector cannot be directly used to
propagate the covariance forward in time.  Instead the state-transition vector is linearized based
on the current system states and used for this task.  The resulting linearization (based on partial
derivatives of f wrt the system states) generates a matrix referred to as the Process Jacobian, F.
This matrix is used to propagate the covariance, P, forward in time.

In addition to the Jacobian, the covariance estimate is affected by the process noise, which is
related to sensor noise levels.  The more process noise exists in a system, the larger the
covariance estimate will be at the next time step.  This noise is reflected in the process noise
covariance matrix, Q.

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
