Programming: [Arduino reference](https://www.arduino.cc/reference/en/)

When programming an arduino, briefly press the Reset button right when clicking
the “upload” button in the Arduino IDE. The reset button runs the bootloader,
which allows a new image to be flashed via USB. If nothing is uploaded within
one second (this behaviour depends on the bootloader which is used), the bootloader
continues and runs the already installed software.

Problems when uploading: [avrdude upload issues][arduino-upload]

[arduino-upload]: https://stackoverflow.com/questions/19765037/arduino-sketch-upload-issue-avrdude-stk500-recv-programmer-is-not-respondi

## Arduino Pro Mini

Can be powered with up to 12 V.

**IO pins** supply up to 40 mA each.
Input leakage current is [around 1 µA](https://electronics.stackexchange.com/a/67173/135063),
see also [this SO answer](https://stackoverflow.com/a/18177902/271961). This, if the internal pull-up resistor is not used.
The internal pull-up resistors are [around 20 to 150 kΩ](https://www.arduino.cc/en/Tutorial/DigitalPins).


**Logic Levels** – [Logic Levels](https://learn.sparkfun.com/tutorials/logic-levels)

* 3.3 V version: V<sub>IL</sub> = 0.8 V, V<sub>IH</sub> = 2 V
* 5 V version: V<sub>IL</sub> = 1.5 V, V<sub>IH</sub> = 3 V

Pinout is on the [product page](https://store.arduino.cc/arduino-pro-mini).

## Serial communication

![Serial communication](Pictures/serial-communication.png)

Additionally to the built-in serial communications on pins 0 and 1 (also on USB connection), Arduino provides the
[SoftwareSerial](https://www.arduino.cc/en/Reference/softwareSerial) library
for software based serial communication. The RX pins must support interrupts.

UART devices typically use a baud rate of 9600. UART, as well as the serial software library, have a built-in RX and TX
buffer which allows to check if bytes are available for reading – the serial protocol itself does not directly support
this, as only bytes are exchanged on link level.
