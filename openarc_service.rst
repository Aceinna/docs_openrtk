OpenARC GNSS Correction Service
================================

.. contents:: Contents
    :local:



Introduction
~~~~~~~~~~~~~~~

OpenARC is Aceinna’s precise positioning platform that offers easy system integration of GNSS corrections with high performance GNSS RTK/INS hardware. OpenARC provides secure GNSS corrections powered by a dense RTK network nation-wide over the United States and a cloud-based architecture.OpenARC offers performance (<10 cm accuracy with no latency), security and integrity (fault tolerance and encryption) and flexibility, while being cost effective. 

OpenARC service is inherently supported by the OpenRTK330LI navigation module and its cloud service interface is embedded in the module firmware, and provides a vertically integrated and seamless positioning platform for industrial and autonomous vehicle applications.


Usage with the OpenRTK330LI Module
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. **Register an OpenARC user account**

  a. Go to https://openarc.aceinna.com, click “Sign Up” to register an account.

    .. image:: media/openarc_signup.png
            :align: center
            :scale: 40%

  b. On the Sign Up page, enter the user name, email, password and confirm password to register, or directly use your GitHub account to register

    .. image:: media/openarc_signup_info.png
            :align: center
            :scale: 40%

2. **Create GNSS correction service account**

  a. Login your OpenARC user account, click on your username that is located on the right-upper corner of the web page, then click on "RTK credentials"

    .. image:: media/openarc_rtkcre.png
            :align: center
            :scale: 40%

  b. On the "RTK Credentials" web page, click "Add" button

    .. image:: media/openarc_rtkcre_add.png
            :align: center
            :scale: 40%

  c. On the "Create RTK Credentials" webpage, create a username and password for your GNSS correction data service, which will be used as the "username" and "password" for a typical NTRIP setting, e.g.

      * *IP Address: openarc.aceinna.com*
      * *PORT: 8011*
      * *Mount Point: RTK*
      * *User Name: username*
      * *Password: password*

3. **Subscribe correction service**

  a. On your OpenARC account webpage, click "Subscriptions" on the left side menu, and then click the "Add" button to create a new data service subscription,

     .. image:: media/openarc_subscribe.png
            :align: center
            :scale: 40%

  b. On the "Create Subscription" page, select the subscription type and modify the number of devices that will be associated with this subscription, and then click "Submit" button to get to the payment page,

    .. image:: media/openarc_choose_subscribe.png
            :align: center
            :scale: 40%

  c. Fill in your payment method information, and complete the OpenARC GNSS correction service account creation and subsription.

    .. image:: media/openarc_payment.png
          :align: center
          :scale: 40%

4. **Bundle your OpenRTK330LI device**

  a. On your OpenARC account webpage, click "Devices" on the left side menu, and then click the "Add" button to start adding a device,

    .. image:: media/openarc_add_device.png
          :align: center
          :scale: 100%

  b. On the pop up window, enter your OpenRTK330LI device's serial number manually. This step is optional as OpenARC will associate your device with your subscription automatically when the device is connected with OpenARC for the first time. Each OpenRTK330LI device has a service trial time after you registered with OpenARC by default, which means during this time you can perform RTK positioning with OpenRTK330LI device.

    .. image:: media/openarc_add_sn.png
          :align: center
          :scale: 90%

  c. Once your OpenRTK330LI device is associated your OpenARC account, for each device on the device list you can click the "bind" button to bundle with your purchased RTK correction service subscription.

    .. image:: media/openarc_bind.png
        :align: center
        :scale: 80%

    .. image:: media/openarc_bind_sub.png
        :align: center
        :scale: 80%