STA8100 firmware loader and upgrade with the Teseo-Suite
========================================================

- The Teseo-Suite should be installed first.
- Get Teseo-Suite Pro v6.2.3 tool from the website: https://www.st.com

Run Teseo-Suit and get a license to active the tool. 
Select menu Help>Activate Teseo-Suit Pro>Rquest full version and send email to ST. 
You can get a license from ST and Activate the Teseo-Suite.

Push the “Boot Mode Switch” side, and connect USB Cable to PC.
Select menu Tools > T5 X-Loader and run.

T5-X-loader Teseo allows to load boot & firmware. Configuration as below.

* **Port Settings:** 
 
  * **Port**: define UART port number. The fourth serial port of your OpenRTK330_EVB.
  * **Loader baud rate**: define programming baud rate 115200. 


* **Option:**

  * **Erase NVM**: erase settings of Teseo.
  * **Restore default product**: erase settings of Teseo.
  * **Variant**: Cut2.


* **Memory:** Choose SQI.

  * **Binary**: Press Binary button to browse your binary file.
    **START** initiates programming. **STOP** cancels programming sequence.
  
  * **Erase only**: erase firmware area.
  * **Program only**: load firmware without perform erase (only available if flash is previously erased).
  * **Destination**: Start address of firmware. Modify memory type to get default one.
  * **Entry point offset**: Start address to run program.
  * **GPIO Reset**: Generate a glitch with a period of “Timing” on DTR or RTS. Helpful, in case of recovery procedure.
  * **Dump memory**

    * **Address**: location of first byte to read.
    * **Size**: **size** of memory to read from 'address'.
  * **Set memory**

    * **Address**: location of interger to write.
    * **Data**: value of integer to write.

When you configurate all the item above, you can click the “Start” to burn the bootloader and firmware. 

The follow figures show the bootloader and firmware burning process.