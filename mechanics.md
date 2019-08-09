# Mechanics

## Limit Switches

Design rules for limit switches:
[Womack Data Sheet 8: Design of Cams for Actuating Limit Switches][womack-cams]

Eaton: [The Basics of Limit Switches](Datasheets/Limit-Switch-Design-Eaton_pct_1549250.pdf)

[womack-cams]: https://www.womackmachine.com/engineering-toolbox/data-sheets/design-of-cams-for-actuating-limit-switches/


## Encoders

Encoder types:

* *Encoders* produce a digital signal from the position
  * Incremental: Each step generates a pulse
  * Absolute: Positions provide encoded information
  * Resolver: Position and speed is provided by sin/cos wave
* *Tachometers* produce a voltage proportional to speed

Absolute encoders often use Gray code where every position change only flips
a single bit to avoid glitches while reading the position.
See [Encoder Whitepaper][encoder-whitepaper] (PDF) by Automation Direct.

[How to Measure Conveyor Belt Speed with Encoders](https://www.dynapar.com/Knowledge/Applications/Measure_Conveyor_Speed_with_Encoders/)

[Angle Encoders: How to Measure Angle Using Encoders](https://www.dynapar.com/Knowledge/Applications/Angle_Encoders/)

[encoder-whitepaper]: https://cdn.automationdirect.com/static/press/encoder-white-paper.pdf


## Tolerances

[Tolerances PDF](https://de.misumi-ec.com/pdf/fa/2014/P1_2287-2288_F80_DE.pdf)
