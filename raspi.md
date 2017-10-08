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

### GPIO debugging

Read/write GPIOs. `gpio` works with the WiringPi GPIO numbers!

```
~> gpio readall
 +-----+-----+---------+------+---+---Pi 3---+---+------+---------+-----+-----+
 | BCM | wPi |   Name  | Mode | V | Physical | V | Mode | Name    | wPi | BCM |
 +-----+-----+---------+------+---+----++----+---+------+---------+-----+-----+
 |     |     |    3.3v |      |   |  1 || 2  |   |      | 5v      |     |     |
 |   2 |   8 |   SDA.1 | ALT0 | 1 |  3 || 4  |   |      | 5V      |     |     |
 |   3 |   9 |   SCL.1 | ALT0 | 1 |  5 || 6  |   |      | 0v      |     |     |
 |   4 |   7 | GPIO. 7 |   IN | 0 |  7 || 8  | 0 | IN   | TxD     | 15  | 14  |
 |     |     |      0v |      |   |  9 || 10 | 1 | IN   | RxD     | 16  | 15  |
 |  17 |   0 | GPIO. 0 |   IN | 0 | 11 || 12 | 0 | ALT5 | GPIO. 1 | 1   | 18  |
 |  27 |   2 | GPIO. 2 |   IN | 0 | 13 || 14 |   |      | 0v      |     |     |
 |  22 |   3 | GPIO. 3 |   IN | 0 | 15 || 16 | 0 | IN   | GPIO. 4 | 4   | 23  |
 |     |     |    3.3v |      |   | 17 || 18 | 0 | IN   | GPIO. 5 | 5   | 24  |
 |  10 |  12 |    MOSI |   IN | 0 | 19 || 20 |   |      | 0v      |     |     |
 |   9 |  13 |    MISO |   IN | 0 | 21 || 22 | 0 | IN   | GPIO. 6 | 6   | 25  |
 |  11 |  14 |    SCLK |   IN | 0 | 23 || 24 | 1 | IN   | CE0     | 10  | 8   |
 |     |     |      0v |      |   | 25 || 26 | 1 | IN   | CE1     | 11  | 7   |
 |   0 |  30 |   SDA.0 |   IN | 1 | 27 || 28 | 1 | IN   | SCL.0   | 31  | 1   |
 |   5 |  21 | GPIO.21 |   IN | 1 | 29 || 30 |   |      | 0v      |     |     |
 |   6 |  22 | GPIO.22 |   IN | 1 | 31 || 32 | 0 | IN   | GPIO.26 | 26  | 12  |
 |  13 |  23 | GPIO.23 |   IN | 0 | 33 || 34 |   |      | 0v      |     |     |
 |  19 |  24 | GPIO.24 |   IN | 0 | 35 || 36 | 0 | IN   | GPIO.27 | 27  | 16  |
 |  26 |  25 | GPIO.25 |   IN | 0 | 37 || 38 | 0 | IN   | GPIO.28 | 28  | 20  |
 |     |     |      0v |      |   | 39 || 40 | 0 | IN   | GPIO.29 | 29  | 21  |
 +-----+-----+---------+------+---+----++----+---+------+---------+-----+-----+
 | BCM | wPi |   Name  | Mode | V | Physical | V | Mode | Name    | wPi | BCM |
 +-----+-----+---------+------+---+---Pi 3---+---+------+---------+-----+-----+
~> gpio mode 21 out
~> gpio write 21 1
~> gpio readall |grep GPIO.21
 |   5 |  21 | GPIO.21 |   IN | 1 | 29 || 30 |   |      | 0v      |     |     |
```

### I²C debugging

`i2cdetect` can list devices on the bus:

```
~> i2cdetect -y 1
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          -- -- -- -- -- -- -- -- -- -- -- -- -- 
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
40: -- -- 42 -- -- -- -- -- -- -- -- -- -- -- -- --
[...]

~> i2cdetect -F 1
Functionalities implemented by /dev/i2c-1:
I2C                              yes
SMBus Quick Command              yes
SMBus Send Byte                  yes
SMBus Receive Byte               yes
[...]
```

To read/write values: `i2cget` and `i2cset`.

