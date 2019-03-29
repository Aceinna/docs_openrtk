Tutorial APP
============

A simple static tilt sensor demo is provided here to show how to add your own algorithm and output algorithm results.

OpenIMU provides a user-friendly interface to add your own algorithms. To do that, the user need to get sensor data, run the algorithm and output algorithm results. All interfaces related to these operations are handled in src/dataProcessingAndPresentation.c. And all user codes implementing the algorithms and results packaging are located in src/user/ directory.

Get algorithm input
-------------------
The platform provides APIs to access all available sensor data, as shown in the following table.

+-------------------------------+-------------------------------------------+
| Sensors                       | Get sensor data API                       |
+===============================+===========================================+
| Accelerometer	                | void  GetAccelsData(double \*data)        |
+-------------------------------+-------------------------------------------+
| Gyroscope                     | void  GetRatesData(double \*data)         |
+-------------------------------+-------------------------------------------+
| Magnetometer	                | void  GetMagsData(double \*data)          |
+-------------------------------+-------------------------------------------+
| GPS	                        | void  GetGPSData(gpsDataStruct_t \*data)  |
+-------------------------------+-------------------------------------------+
| Accelerometer temperature     | void  GetAccelsTempData(double \*temps)   |
+-------------------------------+-------------------------------------------+
| Gyroscope temperature	        | void  GetRatesTempData(double \*temps)    |
+-------------------------------+-------------------------------------------+
| Board temperature             | void  GetBoardTempData(double \*temp)     |
+-------------------------------+-------------------------------------------+

Usually, the accelerometer and gyroscope data are already temperature-calibrated.

Run the algorithm
-----------------
A user defined algorithm should provide its main procedure as:

::

  void *RunUserNavAlgorithm(double *accels, double *rates, ……, int dacqRate)


where **accels** and **rates** are pointers to corresponding sensor measurements, and dacqRate is the sensor sampling rate.

This procedure is implemented in src/user/userAlgorithm.c as follows:

::

 void *RunUserNavAlgorithm(double *accels, double *rates, double *mags,
                           gpsDataStruct_t *gps, int dacqRate)
 {

    //---------------------------get accel data---------------------
    float a[3]; // accel of this step
    a[0] = accels[0];
    a[1] = accels[1];
    a[2] = accels[2];

    //-----------------------calculate euler angles------------------
    results[2] = a[0];
    results[3] = a[1];
    results[4] = a[2];
    float accel_norm = sqrt(a[0]*a[0] + a[1]*a[1] + a[2]*a[2]);
    a[0] /= accel_norm;
    a[1] /= accel_norm;
    a[2] /= accel_norm;
    results[0] = asin(a[0]) * R2D;
    results[1] = atan2(-a[1], -a[2]) * R2D;

    //--------------------------return results-----------------------
    return &results;
 }

It just gets the accelerometer measurement, normalizes it, calculates pitch and roll angles, and returns the results. I keep all the input parameters here. Indeed, I need only **accels**. The user could remove unused parameters in your algorithm.

**results** is a global variable declared as
::

 // algorithm results, [pitch roll ax ay az], in units of deg and g
 static float results[5];

and **R2D** is a macro converting radian to degree:
::

 #define R2D 57.2957795130823

User may also need to implement an algorithm initialization procedure. It is not necessary in this demo, but will be shown here.
::

 void InitUserAlgorithm()
 {
    // place additional required initialization here
    // initialize sample rate and period
    results[0] = 0.0;
    results[1] = 0.0;
 }

Now, a simple user-fined algorithm is done. The framework will automatically call **InitUserAlgorithm** at the initialization stage, and periodically call **RunUserNavAlgorithm** to run the user-defined algorithm and get results.

Output results via debug UART
-----------------------------
This section shows how to use the debug UART (default baud rate is 38400) on the EVB to output algorithm results. The user could also output other information the user are interested in.

To use the debug UART, the user needs to include **debug.h**. For example, I want to output algorithm results after the algorithm is called in **dataProcessingAndPresentation.c**.

- include the header file in **dataProcessingAndPresentation.c**.

::

 #include "debug.h"

- output algorithm results. The results are converted to plain text and then transmitted via the debug UART. The user can also choose to encode the results per user requirements.

::

    // Output results via debug UART. Downsampled by osr due to limited UART bandwidth
    static int out_cntr = 0;
    int osr = 8;
    out_cntr++;
    if(out_cntr==osr)
    {
        out_cntr = 0;
        // generate output string from results
        float *tlt = (float*)results;
        char buffer[128];
        sprintf(buffer,
                "pitch:%.3f\troll:%.3f\tax:%.3f\tay:%.3f\taz:%.3f\n",
                tlt[0], tlt[1], tlt[2], tlt[3], tlt[4]);
        // output to debug UART
        DebugPrintString(buffer);
    }

Compile the project, upload the firmware, and the user can get result via debug UART.

Implementing user-defined packets via UART
------------------------------------------
The debug UART is mainly intended for debug usage. The user may want to output algorithm results via the interface UART (default baud rate is 57600) on the EVB. OpenIMU provides an easy-to-use framework for the user to define your own packets. User-defined packets are declared and implemented in **UserMessaging.h** and **UserMessaging.c**.


- Add your packet code in **UserMessaging.h**.

I added a **USR_OUT_TLT** packet as an example.

::

 // User input packet codes, change at will
 typedef enum {
    USR_OUT_NONE  = 0,  // 0
    USR_OUT_TEST,       // 1
    USR_OUT_DATA1 ,     // 2
    USR_OUT_TLT,        // 3
 // place output packet definitions here
    USR_OUT_MAX
 }UserOutPacketType;

- Add encoding procedure in **UserMessaging.c**.

User defined packets are encoded by this procedure:

::

 BOOL HandleUserOutputPacket(uint8_t *payload, uint8_t *payloadLen)

After I added my encoding codes, this procedure is as follows.

::

 BOOL HandleUserOutputPacket(uint8_t *payload, uint8_t *payloadLen)
 {
    static uint32_t _testVal = 0;
    BOOL ret = TRUE;

	switch (_outputPacketType) {
        case USR_OUT_TEST:
            {  uint32_t *testParam = (uint32_t*)(payload);
             *payloadLen = USR_OUT_TEST_PAYLOAD_LEN;
             *testParam  = _testVal++;
            }
            break;
        case USR_OUT_DATA1:
            {   int n = 0;
                double accels[3];
                double mags[3];
                double rates[3];
                float *sensorData = (float*)(payload);
                *payloadLen = USR_OUT_DATA1_PAYLOAD_LEN;
                GetAccelsData(accels);
                for (int i = 0; i < 3; i++, n++){
                    sensorData[n] = (float)accels[i];
                }
                GetRatesData(rates);
                for (int i = 0; i < 3; i++, n++){
                    sensorData[n] = (float)rates[i];
                }
                GetMagsData(mags);
                for (int i = 0; i < 3; i++, n++){
                    sensorData[n] = (float)mags[i];
                }
            }
            break;
        // place additional user packet preparing calls here
        // case USR_OUT_XXXX:
        //      *payloadLen = YYYY; // total user payload length, including user packet type
        //      payload[0]  = ZZZZ; // user packet type
        //      prepare dada here
        //      break;
        case USR_OUT_TLT:
            {
                if ( tlt == NULL )
                {
                    *payloadLen = 0;
                    ret = FALSE;
                }
                else
                {
                    // get resutls
                    *payloadLen = sprintf((char*)payload,
                            "pitch:%.3f\troll:%.3f\tax:%.3f\tay:%.3f\taz:%.3f\n",
                            tlt[0], tlt[1], tlt[2], tlt[3], tlt[4]);
                }
            }
            break;

        default:
             *payloadLen = 0;
             ret         = FALSE;
             break;      /// unknown user packet, will send error in response
        }

        return ret;
 }

This procedure will be called at the defined rate by the framework.

The framework default outputs calibrated IMU sensor data. To output your own packets, the user should tell the framework the packet code of your packet, and then feed the algorithm results to the encoding procedure we just implemented above.

- Register the user-defined packet in the framework.

This can be done by calling **setOutputPacketCode** when initializing user-defined algorithm in **dataProcessingAndPresentation.c**. To use **setOutputPacketCode**, the user need

::

 #include "SystemConfiguration.h"

and then call it in

::

 void initUserDataProcessingEngine()
 {
    InitUserDataStructures();    // default implementation located in file UserData.c
    InitUserFilters();           // default implementation located in file UserFilters.c
    InitUserAlgorithm();         // default implementation located in file user_algorithm.c
    setOutputPacketCode(0x7A32);    // set output packet to user defined packets
 }

In this way, the default packet will be replaced by the user-defined packet.

- Feed algorithm results to the encoding procedure.

In **dataProcessingAndPresentation.c**, after calling the user-defined algorithm, the framework will call

::

 WriteResultsIntoOutputStream(results) ;   // default implementation located in file file UserMessaging.c

to feed **results** to **UserMessaging.c**. **WriteResultsIntoOutputStream** is implemented like this:

::

 void WriteResultsIntoOutputStream(void *results)
 {
    //  implement specific data processing/saving here
    tlt = (float*)results;
 }

where **tlt** is a global variable declared as

::

 static float *tlt;  // pointer to algorithm results

Now, compile the project, upload the firmware, and the user can get results via the interface UART.
