Radio
=====

For this system to work every device in the network needs to communicate with each other.  This is done using the nRF24L01+PA+LNA Wireless Transceiver or shortened to the nRF24.  This is a 2.4-2.48 GHz transceiver which means that it can send and receive data at the same frequency your WiFi uses.  Each radio has the option of using 126 different channels (different field sites) different transmission speeds (250 KBPS, 1 MBPS and 2 MBPS) and four different power settings.  It can connect to six other radios (expanded to 127 in software) and has a lessthan 1 mA sleep power consumption.  You can find out more about the radios by viewing the `datasheet <https://www.sparkfun.com/datasheets/Components/SMD/nRF24L01Pluss_Preliminary_Product_Specification_v1_0.pdf>`_.

There are two common versions of the nRF24: short range and long range (see long range below).  From my testing the short range version are inadequate under almost all circumstances so only the long range should be used.  The long range model has an advertised range of 1000 m but I have only tested up to 600 m.  To allow for greater range and more than six radios to be connected a software library is used which creates a mesh network.  This is explained in section 4.3.  These radios and the RFIDNetwork class rely on the SPI protocol, so while it has only been tested on Arduinos and Raspberry Pis it should work on other microcontrollers that support SPI. You can also find everything needed with approximate pricing in the appendix (7.1).

The radio comes with eight 2.54 mm pins called male headers.  The recommended way to connect the radio is by soldering male headers to the microcontroller and then connecting a wire with female headers between the microcontroller and radio.  See the image below.  A modification needs to be made to the radios to ensure reliable performance due to an inadequate supply of power to the IC of the radio.  This can be done by buying a power module (seen in the image below) or soldering a 4.7 :math:`\mu` F-10 :math:`\mu` F capacitor between the ground and VCC pin (1 and 2) on the radio module.


.. figure:: ../../images/ETAG_Assembled.jpg
	:align: center

	ETAG with headers soldered on and female jumper wire connecting long range nRF24 with power module to board.


Setting up the radio
--------------------

The radio will come in a static protector bag.  Take it out of the bag and screw the antenna onto the receptacle.  It should look like the image above.  To ensure reliable performance either a power module or capacitor needs to be added.  The power module makes it easier to switch in the field but is more expensive and uses more power (due to having a power indicator LED)than the capacitor.  The capacitor requires soldering but is cheaper and should result in the same performance.

**Using the power module:**
Notice that there are two rows of 4 male pins on the radio and 2 rows of 4 female pins on the power module.  Insert the radio into the power module so that all pins are seated.  Next connect 7 female wires to the wires on the power module.  Make sure that every pin on the power module is connected except for the pin labeled IRQ (looks like RO).  Make sure that you only plug the **VCC (pin 2) into 5V** otherwise the module might get enough power.  This should look like the picture above.

**Using the capacitors:**
It is recommended to use a 4.7-10 capacitor with the radio.  Solder the capacitor between pins 1 and 2 of the top of the radio (pin 1 has the white square around it and 2 is the the right, in the different row of 4 pins).  If you're using an electrolytic capacitor then make sure the negative lead is soldered to VCC.  Next connect 7 female wires to the wires on the radio.  You don't need to attached the IRQ (pin 8, located farthest away from pin 1 and on the side closer to the antenna).  Make sure that you plug the **VCC (pin 2) into 3.3V** otherwise the module might get damaged.  This should look like the picture below:


	\colorbox{pink}{long range with capacitor and wires}
	%\includegraphics[width=6in]{nRF24_radioCap}\\
	Long range nRF24 radio with capacitor and wires attached.


.. figure:: ../../images/nRF24_pinout.png
	:align: center

	nRF24 pin layout with numbering and purpose.


