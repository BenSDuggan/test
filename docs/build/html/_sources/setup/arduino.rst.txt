Arduion (RFID reader)
=====================

The Arduino is used to get the RFID tag onto the server and saved.  This system should work with any RFID reader but you need to get the reads onto an Arduino supported microcontroller.  With Arduino based readers, like the ETAG, there is no need to purchase another Arduino as it is open-source and Arduino based.  However, with other readers that aren't Arduino based, like the Gen. 2 by Eli Bridge et al., a device must be used to get the reads onto an Arduino.  This makes the setup more complicated and more expensive.  We'll use both of these RFID readers as examples of how this system can be setup.  The software ran on the Arduinos is the same regardless of the reader, just will a few minor changes.


Hardware
--------

Arduino based RFID readers (e.g. ETAG)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To connect the radio to the Arduino (either an Arduino based RFID reader or one that will be used to communicate with a reader) you just need to connect the jumper wires from the radio to the board.  This means identifying which pins are used for SPI, digital, ground and power pins are (making sure you correctly use either 3.3V or 5V, depending on your configuration).
The easiest way to identify where the SPI pins are is by using the 6 pin ISCP header.  You may need to solder pins to it but it has as standard format (shown below).  The VCC is likely at 5V so if you are using the capacitor you \textbf{shouldn't use this VCC} and should use the 3.3V output pin and the board.  The ISCP VCC pin on the ETAG is at 5V.\\

.. figure:: ../../images/icsp_pinout.png
	:align: center

	ISCP 6 pin header on Arudino Uno(ETAG) with pins labeled.


That takes care of GND, VCC, SCK, MOSI, MISO and IRQ (which isn't used).  The CE and CSN pin need to be connected to two digital pins and is defined in software.  By default CE is set to digital pin 3 and CSN is set to digital pin 4 but you can change this if you'd like.  This boils down to the following wiring as shown in the table below and the image showing the final setup.

**Table showing pin connections between radio and Arduino.**

===========  ================  ============  ===============
Power module (from module)     Capacitor (from radio)
-----------------------------  -----------------------------
nRF24 pins   Arduino pins      nRF2 pins     Arduino pins
===========  ================  ============  ===============
1. GND       GND on ICSP       1. GND        GND on ICSP
2. VCC       5V(VCC on ICSP)   2. VCC        3.3V on board
3. CE        digital pin 3     3. CE         digital pin 3
4. CSN       digital pin 4     4. CSN        digital pin 4
5. SCK       SCK on ICSP       5. SCK        SCK on ICSP
6. MOSI      MOSI on ICSP      6. MOSI       MOSI on ICSP
7. MISO      MISO on ICSP      7. MISO       MISO on ICSP
8. IRQ       Not used          8. IRQ        Not used
===========  ================  ============  ===============


.. figure:: ../../images/ETAG_Assembled.jpg
	:align: center

	ETAG with headers soldered on and female jumper wire connecting long range nRF24 with power module to board (note that CE is connected to 2 and CSN is connected to 3).


This is all that needs to be done to setup the hardware on Arduino based RFID readers like the ETAG.


General way of adding system (e.g. Gen. 2)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


For RFID readers that are **not Arduino based**, like the Gen. 2, there is a lot of work that needs to be done.  The reason for this is that we need to get the RFID data off of the board and into the code, which is written for Arduinos.  This means that you have to purchase an Arduino, any extra equipment to create an interface between the reader and Arduino and add the radio to it.  Any Arduino should do but I recommend the Arduino Nano as it is common, cheap, low power, and easy to use.  Once you do this attach the radio using the instructions above.

Next we need a way to get the data off of the RFID reader.  RFID readers typically have a way for people to configure and see what tags the reader is currently reading.  On the Gen. 2 there is a RS-232 serial port where you can attach a cable and connect it to your computer.  When you connect to it you can view the settings and see what reads are coming into the reader.  This provides us with a way to get the data off of the reader into an Arduino.

As Arduinos cannot us USB (without a shield), we'll have to add a device that the Arduino understands, a RS232 to TTL with a null modem.  Attach this to the Gen. 2 and try to secure it with screws.  Make sure that you handle the Gen. 2 with care now as there is a large lever arm attached to the RS232 connector.  Notice that there are 2 rows of 4 pins on the connector.  Attach a wire to ground, VCC and txt pins (you may have to experiment with using the rxt pin).  Check to see if the connector uses 3.3V or 5V (mine used 5V).  Connect that wire to the pin giving that voltage on the reader.  If the voltage of the connector and radio are the same and you only have one pin of that voltage you can plug the VCC cable from the radio into the extra VCC pin on the converter.  Next connect the wire that leads to ground and the last wire to digital pin 5.  This is the setup I went through with my RS232 to TTL converter (link in Appendix).

Lastly the Arduino needs to get powered.  If the RFID reader has pins to output power (VCC) then you can solder wires to it and a ground pin.  Make sure that the output voltage on the RFID reader is not greater than the input voltage the Arduino can take.  If that condition holds then connect the VCC wire to the VIN pin on the Arduino and ground wire to the ground pin on the Arduino.  If this is not an option on your RFID reader then you might be able to add wires to the battery powering the reader or just use a separate battery to power the Arduino.


.. figure:: ../../images/Gen2_Assembled.jpg
	:align: center

	Gen. 2 board with RS-232 to TTL and null modem and long range nRF24 connected to Arduino, powered by the Gen.2.


Software
--------

If you don't already have the Arduino IDE downloaded it from their `website <https://www.arduino.cc/en/Main/Software>`_ and install it.  The Arduino scripts that work with this system are found in the arduino folder of the repository.  If you haven't already downloaded the `GitHub repository <http://github.com/BenSDuggan/RFID-Network>`_ you should do so now.

There are several codes that you can choose to upload.  If you just want to test the code you should upload the send on interval code found in the sendOnInterval folder.  There is a version of the code that works with all boards (sendOnIntervalAll) and one written just for the ETAG (sendOnIntervalETAG) that uses the SD card and RTC on the reader.  If you are using the Gen. 2 or ETAG there is code provided that will read reads off of the antenna with out affecting reader performance.  The Gen. 2 script is called RFIDNetworkGen2 and the ETAG script is called RFIDNetworkeETAG.
Once you have selected the code you wish to use, connect the Arduino to the computer using the a USB, check to see that the correct board and port are selected.  You're can now upload the code by clicking the upload button.





