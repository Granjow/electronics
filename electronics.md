# Electronics

## Terms

* Input Clamp Current V<sub>IK</sub>, Output Clamp Current V<sub>OK</sub>
  
  Current which will destroy the IC. Can be caused by too high input voltage.
  See [StackExchange](https://electronics.stackexchange.com/questions/107687/input-and-output-clamping-current-of-the-ic-4082)

## Fuses

* [ESKA: Technische Einführung](http://eska-fuses.de/fileadmin/pdf/content/Technische_Einfuehrung.pdf)

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
[(pdf)](Datasheets/74HCT08.pdf)
3 V to 5 V level shifter. Actually only an AND, but with V<sub>DD</sub> = 5 V and V<sub>IH</sub> = 2 V it can be used
to shift 3.3 V from the Raspi (or from elsewhere) to a 5 V signal.

Compared to optocouplers, the 74xx are much more efficient with I<sub>I</sub> = 1 µA as there is no need to power a LED,
but V<sub>CC</sub> is only 4.5 to 5.5 V.

More about the [74xx Series](https://www.mikrocontroller.net/articles/74xx) which contains many logic chips.
74AHCT125 is popular as well for level shifting.

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

## Random links

* [3V and 5V Tips/Tricks (pdf)](http://www.newark.com/pdfs/techarticles/microchip/3_3vto5vAnalogTipsnTricksBrchr.pdf)