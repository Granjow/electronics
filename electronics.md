# Electronics

## Good to know

Connector numbering: 1, 2, 3 etc. start on the bottom left. The left side is marked, e.g. with a dot.
([Thanks, js-boxdrawing](http://marklodato.github.io/js-boxdrawing/))

     8 7 6 5
    ┌┴─┴─┴─┴┐
    │o      │
    └┬─┬─┬─┬┘
     1 2 3 4

## Terms

* Forward Voltage V<sub>F</sub>

  E.g. voltage drop over diodes, also in optocouplers. 

* Input Clamp Current V<sub>IK</sub>, Output Clamp Current V<sub>OK</sub>
  
  Current which will destroy the IC if the input voltage is out of range.
  See [StackExchange](https://electronics.stackexchange.com/questions/107687/input-and-output-clamping-current-of-the-ic-4082)

## Fuses

* [ESKA: Technische Einführung](http://eska-fuses.de/fileadmin/pdf/content/Technische_Einfuehrung.pdf)

## Wires

Pin headers on PCBs are often connected with Dupont cables. I ended up crimping hundreds of them. This video
gives a quite good understanding:
[Custom Cables & Guide to Crimping Dupont PCB Interconnect Cables](https://www.youtube.com/watch?v=GkbOJSvhCgU).
I am using pliers and no special crimping tools, which works just as well with a bit of practice.

For crimping, the components can be found on ebay:

* *(fe)male dupont crimp pin header*; usually come on a band where they have to be cut off with a side cutter,
  convenient for storage.
* *dupont pin housing 1P (2P, 3P, …)*; The pin housing work for both male and female pins, there is no difference.
  1P denotes housings for a single pin, 2P for two pins, etc. I use 2P most often, then 1P, 4P, and 3P.

## Useful parts

**1N4001** up to **1N4007** — 
[(pdf)](Datasheets/1n4001.pdf)
Rectifier diode, 50 to 1000 V, 1 W.
Forward voltage (voltage drop) is V<sub>F</sub> = 1.1 V.

## Useful ICs

**PCF8574P** and **PCF8574AP** —
[(pdf)](Datasheets/PCF8574.pdf)
I²C based port expander. Connect SDA (data) and SCL (clock) 
to the Raspi I2C pins (5 and 7) for 8 additional digital IOs. 

Both ICs have 3 address bits, so 8 of them can be used. The P and AP differ only in their 
slave address, so when used together, 16 of them can be attached (resulting in 128 IOs).

**MCP23008** and **MCP23016** —
[(pdf 016)](Datasheets/MCP23016_20090C.pdf)
[(pdf 017)](Datasheets/MCP23017_20001952C.pdf)
I²C based port expander. More features, but did not work for me, communication randomly stopped.
(Very small chance that I used the wrong data sheet.)

**MCP23017** —
[Why use MCP23008 / MCP23016 / MCP23017 expanders](http://peter224722.blogspot.ch/2014/03/why-use-mcp23008-mcp23016-mcp23017.html)

**PC817** —
[(pdf)](Datasheets/PC817C.pdf)
Optocoupler, 4 legs. Separates two voltages (galvanic isolation) by driving a phototransistor with an infrared LED. 
Max 3 V 50 mA in, 35 V 50 mA 150 mW out.

Easily available and cheap.

**4N25** —
[(pdf)](Datasheets/4n25.pdf)
Optocoupler, 6 legs. Easily available.

**ILD2**, **ILQ2**, **617** — More optocouplers

**LM393** —
[(pdf)](Datasheets/lm393-n.pdf)
Comparator; compares two voltages and sets the output low or high, depending on which input was higher.

**NE555** — Decade counter.

**CD4017B** — Decade counter, 10 inputs

**74HCT08** —
3 V to 5 V level shifter. Actually only an AND, but with V<sub>DD</sub> = 5 V and V<sub>IH</sub> = 2 V it can be used
to shift 3.3 V from the Raspi (or from elsewhere) to a 5 V signal.

Compared to optocouplers, the 74xx are much more efficient with I<sub>I</sub> = 1 µA as there is no need to power a LED,
but V<sub>CC</sub> is only 4.5 to 5.5 V.

Also, their response time is a lot shorter; around 40 ns compared to 20 µs for a PC817.

[The 74xx Series](https://www.mikrocontroller.net/articles/74xx) contains many logic chips. Some data sheets:
* [SN74HCT00 (pdf)](Datasheets/sn74hct00-short.pdf) is a 2-input NAND (25 ns max.; 9 ns max. for AHCT version)
* [SN74HCT08 (pdf)](Datasheets/sn74hct08-short.pdf) is a 2-input AND (30 ns max.; 9 ns max. for AHCT version)
* [SN74AHC125 (pdf)](Datasheets/sn74ahct125-short.pdf) is a fast 3-state buffer (10 ns max.)

## Voltage Regulators

**LM3940** —
[(pdf)](Datasheets/lm3940.pdf) Converts 5 V to 3.3 V. Basically by means of converting the rest to heat.
3.3 V, 1 A; U<sub>in</sub> 4.5 to 5.5 V.

## Toys

**WS2812B** —
[(pdf)](Datasheets/28085-WS2812B-RGB-LED-Datasheet.pdf)
RGB LED with integrated chip (or the other way round), PWM controlled. Usually on LED stripes which can be cut off anywhere.
LEDs take 60 mA when all 3 colours are on, V<sub>DD</sub> = 5 V. Data (D<sub>IN</sub> and D<sub>OUT</sub> PINs) have 
logic levels 0.3 V<sub>DD</sub> and 0.7 V<sub>DD</sub> and require 1 µA.
WS2812B is the [improved version of the WS2812](https://acrobotic.com/datasheets/WS2812B_VS_WS2812.pdf).

Since the signals sent on the data pin are around 400 ns, it is not possible to use optocopulers for shifting a 3.3 V
signal to 5 V. They have a response time of several 1000 ns. A 74HCT08 can instead be used.

The following image shows the same WS2812B signal directly from the Raspi (blue) and after an optocoupler (red).
Quite obviously, it is not quite the same curve. The PC817 is too slow to reproduce the signal

![PC817](Pictures/ws2812-data-after-pc817.png)

After the faster SN74HCT08, the signal is reproduced almost identically, but at a level of 5 V.

![SN74008](Pictures/ws2812-data-after-sn74hct08n.png)

To get it working on a Raspi directly, use [rpi-ws281x-native](https://www.npmjs.com/package/rpi-ws281x-native).
Level shifting to 5 V is required, otherwise artifacts (wrong pixels) will occur. 
This method is very fast and writes about 24000 LEDs per second on a 60 LED strip (400 fps).

Another solution is to feed colour data from the Raspi to an Arduino. Arduino’s microcontroller then sends data to  WS2812. 
Use [node-pixel][pixel] and flash the Arduino (I use an Arduino Pro Mini) [with the Backpack firmware][pixel-backpack];
it then listens on I²C on pins A4 and A5, which may have to be soldered first. 
On the Raspi, connect to the Arduino with Johnny-Five [and raspi-io][pixel-raspi-io].
This method is slower with around 740 LEDs per second, which is 12 fps for 60 LEDs.


[pixel]: https://www.npmjs.com/package/node-pixel
[pixel-backpack]: https://github.com/ajfisher/node-pixel/blob/master/docs/installation.md#i2c-backpack-installation
[pixel-raspi-io]: https://github.com/ajfisher/node-pixel/issues/68#issuecomment-232822499


## EL Wire

* [How to solder or repair EL wire](http://elwirecraft.co.uk/el-how-to-and-tips/how-to-solder-or-repair-el-wire/)
* [The Full "How To" Manual for EL (Electroluminescent) Wire](http://www.instructables.com/id/The-Full-How-Too-Manual-For-EL-Electroluminesce/) (See comment by MTJimL)


## Random links

* [3V and 5V Tips/Tricks (pdf)](http://www.newark.com/pdfs/techarticles/microchip/3_3vto5vAnalogTipsnTricksBrchr.pdf)
