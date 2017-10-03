# Raspberry Pi 3

[Chip](https://www.raspberrypi.org/documentation/hardware/raspberrypi/bcm2837/README.md): BCM3827, Quad-Core ARMv8

## GPIO and Electrical Specifications

* GPIO: [Pinout.xyz](https://pinout.xyz/)
* SX: [Raspi Power Limitations](https://raspberrypi.stackexchange.com/a/51616/57569)
* [GPIO Electrical Specifications](http://www.mosaic-industries.com/embedded-systems/microcontroller-projects/raspberry-pi/gpio-pin-electrical-specifications) (Not necessarily for Raspi 3, but generally interesting)

**Input** is 5±0.25 V, Raspi draws 8–9 W; with periphery (mouse, keyboard, other USB devices) up to 15 W.

**GPIOs** can provide 16 mA each. The 3.3 V pin can provide up to 800 mA. The 5 V pin can provide what remains of the input power (input current is 2.5 A); i.e. minus USB, HDMI (50 mA), GPIOs etc.

Logic levels are V<sub>IL</sub> = 0.8 V (lower voltages will be recognized as LOW), V<sub>IH</sub> = 1.3 V (higher voltages will be recognized as HIGH).

GPIOs have internal pull-up/pull-down resistors with 1.8 kΩ (probably) which can be enabled individually. 
As a result from the logic voltage levels, setting a pin with pull-down to HIGH requires a resistor smaller than `1800 Ω · 0.8 V / 2.5 V = 576 Ω` (voltage divider).

For external pull-up/pull-down resistors, [use 10 kΩ](https://www.raspberrypi.org/forums/viewtopic.php?f=44&t=46525#p366259). 
For setting IOs to LOW/HIGH, use 1 kΩ; this will result in a current of 3.3 mA. 

**USB** can provide 1200 mA.
