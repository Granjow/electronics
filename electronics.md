# Electronics

## Housings

Connector numbering: 1, 2, 3 etc. start on the bottom left, counter-clock wise.
The left side is marked, e.g. with a dot.
([Thanks, js-boxdrawing](http://marklodato.github.io/js-boxdrawing/))

     8 7 6 5
    ┌┴─┴─┴─┴┐
    │o      │
    └┬─┬─┬─┬┘
     1 2 3 4

**DIP** (dual in-line package): ICs with pins for 2.54 mm breadboards.
DIP-4 has 4 pins, etc. Pitch is 0.1 inch (sometimes denoted as 0.100 BSC)
or 2.54 mm.

The following picture shows a DIP-16 IC, a DIP-4 optocoupler, and another one on a DIP-8 socket.
The sockets have an indent on one side to mark pin 1. This means that the optocopuler
on the picture is mounted totally incorrectly.

![DIP packages](Pictures/dip-packages.jpg)

**SOP**, SOIC, (T)SO… (small outline …): ICs for surface mounting (SMD).
Most packages can have different dimensions (e.g. width/height). Pitch is
the distance from one leg tip to the next one.

* SOIC (Small Outline IC): 1.27 mm pitch
* SOP (Small Outline Package): 0.65, 0.635 or 0.5 mm pitch
* TSSOP (Thin Shrink SOP): 0.65 mm pitch
* MLF (Micro Lead Frame): Square package with 0.5 mm pitch

The package size of SMD components like resistors and capacitors is given by a
code like 1206, which means 0.12×0.06 inch (imperial code), or 5630, which means
5.6×3.0 mm (metric code).

## Terms

* Power supply (rail) pins: SS, DD and so on originate from drain/sink (FET)
  and from base/collector/emitter (BJT).
  More information [on Wikipedia](https://en.wikipedia.org/wiki/IC_power-supply_pin)
  * V<sub>CC</sub> and V<sub>DD</sub> and V<sub>BB</sub>: `+` supply voltage
  * V<sub>SS</sub> and V<sub>EE</sub>: `−` supply voltage
  * GND: Ground
* V<sub>F</sub>: Forward Voltage. E.g. voltage drop over diodes, also in optocouplers.
* V<sub>IK</sub>: Input Clamp Current, V<sub>OK</sub>: Output Clamp Current.
  Current which will destroy the IC if the input voltage is out of range.
  See [StackExchange](https://electronics.stackexchange.com/questions/107687/input-and-output-clamping-current-of-the-ic-4082)


## Fuses

protect against overcurrent.

### Normal fuses

* [ESKA: Technische Einführung](http://eska-fuses.de/fileadmin/pdf/content/Technische_Einfuehrung.pdf)

Fuses usually contain a metal wire which melts when the current is too large, hereby interrupting the electrical circuit.
The fuse then has to be replaced.

Fuses blow at different speed. Usually, fast fuses are used for sensitive electronics, and slow fuses are used
if larger currents occur when powering on a device.

### Resettable fuses

* [PolySwitch Fundamentals](Datasheets/S11_PolySwitch-Fundamentals.pdf)

Resettable fuses conduct only when cold. High currents cause it to heat up, and the resistance rasises.
When the current drops, the fuse cools down and conducts again.

Resettable fuses do not need to be replaced after a trip. They are mainly available up to 60 V / 40 A.

Resettable fuses are guaranteed to trip above I<sub>trip</sub>, but not below I<sub>hold</sub>. The trip time depends
on the current; for the PRFA.025, it is 6 ms at 10 A, and 6 s at 1 A.

**PRFA.025**
[(pdf)](Datasheets/Schurter_typ_PFRA.pdf)
—
Series of resettable fuses up to 60 V and 100 A.


## Breadboard prototyping

Some nice breadboard hacks are listed in [My Top Ten Most Useful Breadboard Tips and Tricks][breadboard-tips].

[breadboard-tips]: http://www.instructables.com/id/My-Top-Ten-Most-Useful-Breadboard-Tips-and-Tricks/


## Driving higher loads

To control higher voltages or currents with e.g. a Raspberry 3.3 V 20 mA output, there are different options.

Same voltage:

* Transistor
* MOSFET

Different voltage:

* Optocoupler
* Mechanical relay
* MOSFET (N-Channel with common ground)
* Solid-state relay ([Photo MOSFET](https://www.renesas.com/en-in/products/optoelectronics/technology/difference.html))

The optocoupler output can again be amplified with transistors as they usually have a current of I<sub>C</sub> = 20 mA.
See [Driving High-Level Loads With Optocouplers][vishay-loads].

[vishay-loads]: http://www.soloelectronica.net/PDF/optoacopladores/vishay_driving_high-level_loads_with_optocouplers_83704.pdf

## Useful parts

### Resistors

http://www.resistorguide.com/

### Transistors

[Transistor as switch](https://www.baldengineer.com/low-side-vs-high-side-transistor-switch.html)

When BJTs and MOSFETs are used as switches, they can be used as *high side*
(P-channel and PNP) or as *low side* (N-channel and NPN) switches.

*Low Side* means the transistor is connected to ground, which is often easier to
control as only the saturation voltage is needed to turn it on fully. However,
if it is used to power on a device which relies on a correct GND level like a
microcontroller, a high side switch should be used due to the voltage drop over
the transistor to ground.

*High Side* means the transistor is connected to the supply voltage.

If the V<sub>gs</sub> voltage of a transistor is too high for the input signal,
a transistor driver is required.

![Transistor driver for higher voltage](Pictures/transistor-driver.png)


### Bipolar transistors (BJTs)

![NPN](Pictures/transistor-npn.png)
![PNP](Pictures/transistor-pnp.png)

Current can flow from collector to emitter if the base voltage V<sub>B</sub> is *more positive* (NPN)
or more negative (PNP) than the emitter voltage V<sub>E</sub>.

There is a voltage drop V<sub>BE</sub> which depends on the transistor. The difference between base and emitter
must in fact be higher than the voltage drop.

A transistor also has a (smaller) voltage drop from collector to emitter, V<sub>CE</sub>, which depends on several variables.
V<sub>CE</sub> also determines losses. A 2N4001 at 500 mA has a V<sub>CE</sub> = 0.75 V, resulting in 375 mW power dissipation.

Transistors contain two np transitions.

![NPN](Pictures/transistor-npn-alt.png)
![PNP](Pictures/transistor-pnp-alt.png)

Transistors work in different modes depending on the voltage levels on each inputs.

* Saturation for V<sub>B</sub> > V<sub>C,E</sub> − acts like a switch. (Again, V<sub>B</sub> > V<sub>E</sub> + V<sub>BE(sat)</sub>)
* Cutoff: No current flows from C to E if V<sub>B</sub> < V<sub>C,E</sub>
* Active: Transistor works as amplifier for V<sub>C</sub> > V<sub>B</sub> > V<sub>E</sub> with I<sub>C</sub> = β I<sub>B</sub>.
  Since I<sub>B</sub> is added to the output current, I<sub>E</sub> is 1/α I<sub>C</sub> for α = β / (β+1).
* Reverse Active: Current flows from E to C if V<sub>C</sub> < V<sub>B</sub> < V<sub>E</sub>. Not how transistors
  should be used; β<sub>R</sub> is lower.

Saturation is when the transistor is fully conductive and increasing the base current I<sub>B</sub> does not change
the output signal any further – or, not significantly.
ON’s data sheet for the 2N4401 contains a nice graph which shows the collector–emitter forward voltage with increasing I<sub>B</sub>.
Note how the curve also depends on I<sub>C</sub>.

![Saturation curves](Pictures/transistor-saturation-2N4401.png)

Switching speed of transistors is limited by their turn-on and turn-off time,
which are calculated as delay time + rise time and storage time + fall time.
Delay time and storage time can be large and can make a transistor unusable
for fast signals and can be reduced with a Baker clamp.

More detailed explanation in [Transistors (PDF)](Datasheets/lect10_BJT-Transistors.pdf).

![BJT Rise and fall time](Pictures/bjt-tON-tOFF.png)

Further links:

* [Transistor drawings](http://hyperphysics.phy-astr.gsu.edu/hbase/Solids/trans2.html)
* [Transistor as Switch](http://www.electronicshub.org/transistor-as-switch/), see also the chapter “Example of NPN Transistor as a Switch”
  about inverting an input signal with a transistor.
* [Bipolar Transistor](http://www.electronics-tutorials.ws/transistor/tran_1.html)

#### Sample Transistors

The following transistors are all in a TO-92 housing with 3 legs. Note that the leg order E–B–C is not standardised,
so it needs to be looked up for each transistor individually in the data sheet.

There are also transistor arrays in DIP housings, like the ULN2802A [(pdf)](http://www.ti.com/lit/ds/symlink/uln2803a.pdf),
which have one common emitter (or collector, for PNP). They are in fact darlington transistor arrays (two transistors in series)
and suit well as switches with a h<sub>fe</sub> of around 1000.
A disadvantage however is their larger V<sub>CE</sub> saturation voltage due to
the second transistor.

**2N4401**
[(pdf)](Datasheets/2N4401-D.PDF)
—
I<sub>C</sub> 600 mA, V<sub>BE(sat)</sub> 0.75–1.2 V, V<sub>CE(sat)</sub> 400–750 mV

**2N3904**
[(pdf)](Datasheets/2N3904-short.PDF)
—
I<sub>C</sub> 200 mA, V<sub>BE(sat)</sub> 0.65 to 0.95 V, V<sub>CE(sat)</sub> 200–300 mV

**BC337**
[(pdf)](Datasheets/BC337-D.PDF)
—
I<sub>C</sub> = 800 mA, V<sub>BE(sat)</sub> = 1.2 V, V<sub>CE(sat)</sub> 300–700 mV

**BC547**
[(pdf)](Datasheets/BC547-short.pdf)
—
I<sub>C</sub> = 100 mA, V<sub>BE(sat)</sub> = 0.7–0.9 V, V<sub>CE(sat)</sub> 90–250 mV. Constant h<sub>fe</sub>.

**BC635**
[(pdf)](Datasheets/bc635.pdf)
—
I<sub>C</sub> 1 A, V<sub>BE(sat)</sub> = 1 V, V<sub>CE(sat)</sub> 10–300 mV.

**BC327** *PNP*
[(pdf)](Datasheets/BC327-D.PDF)
—
I<sub>C</sub> −800 mA, V<sub>CE(sat)</sub> −0.7 to −1 V


**BC557** *PNP*
[(pdf)](Datasheets/BC557-short.pdf)
—
I<sub>C</sub> −100 mA, V<sub>CE(sat)</sub> −90 to −250 mV

**2N2907** *PNP*
[(pdf)](Datasheets/2N2907A-D.PDF)
—
I<sub>C</sub> −600 mA, V<sub>BE(sat)</sub> −0.6 to −2.6 V, V<sub>CE(sat)</sub> −70 to −1600 mV



### MOSFETs

are *voltage controlled*, in contrast to bipolar transistors, which are current controlled. This means that when
a current flows between drain and source, a MOSFET requires (almost) no current through the gate.
It *also* means that the gate has to be discharged to turn the FET off, see
[Calculating the pulldown resistance for a given MOSFET’s gate](https://electronics.stackexchange.com/a/66555/135063)

* In N-channel MOSFETs, current flows if V<sub>G</sub> > V<sub>S</sub>
* In P-Channel MOSFETs, current flows for V<sub>G</sub> < V<sub>S</sub>

Source is connected to ground (N-Channel) or to Vdd (P-Channel), the load goes to the drain.
([Typically.](https://oscarliang.com/how-to-use-mosfet-beginner-tutorial/))

![](Pictures/mosfet-n-p.png)

MOSFETs can drive ridiculously high currents (like 140 A) in their packings with heatsink like TO-220, TO-262, etc.;
these are called *power MOSFETs*. N-channel power mosfets have lower resistance than P-channels.

To efficiently switch MOSFETs at high load, this must be done quickly due to losses during the transition phase.
This is important especially for power MOSFETs operating at higher frequency. Switching quickly requires *high currents*
to charge/discharge, but only for a short amount of time.

The MOSFET gate is sensitive to electrostatical discharge and it is not too hard to break a MOSFET when not handled carefully;
transistors are much more robust in this regard.

When [choosing a MOSFET](https://www.youtube.com/watch?v=GrvvkYTW_0k), it is important
to check that its threshold gate-source voltage V<sub>GS(th)</sub> is lower than
the input signal which is used to turn it on (e.g. 3.3 V for a Raspberry Pi).


#### Thermal Dissipation

When power is dissipated in a MOSFET, its temperature rises. Thermal resistance describes how much of this thermal power
is “kept back” in different transitions. In MOSFETs, there are transitions from junction/die to case, to heat sink, to ambient –
while ambient usually is still air without ventilation.

Data sheets often provide only a subset of those values. θ<sub>JC</sub> is also written as R<sub>θJC</sub>.

![Thermal Dissipation](Pictures/thermal-dissipation-mosfet.png)

Thermal resistance of junction to case, θ<sub>JC</sub>, and case to sink, θ<sub>CS</sub>, are given by the part’s case.
The transition to ambient can be changed by attaching the heat sink to a better/larger heat sink with a thermal resistance
that is below θ<sub>SA</sub>, or by cooling the part with a fan.

Example values for θ<sub>JA</sub> are 313 °C/W for a 2N700 in a TO-92 package, and 62 °C/W for a IRL3803 in a TO-220AB package.

MOSFETs have a maximum junction temperature T<sub>Jmax</sub>, which is affected by the die quality. Power dissipation has
to be low enough so that its junction temperature stays below the maximum value. For an IRL3803 with T<sub>Jmax</sub> = 175 °C
and T<sub>A</sub> = 25 °C, the maximum power dissipation is `150 °C / 62 °C/W = 2.4 W` with ambient cooling, and with an
appropriate heatsink and a θ<sub>JC</sub> of 0.75 °C/W it is `150 °C / 0.75 °C/W = 200 W`.

```math
P_{Dmax} = \frac{T_{Jmax} - T_A}{\Theta_{JA}}
```

Power dissipation mainly happens due to the on-resistance R<sub>DS(on)</sub> in the continuous case,
and when switching the MOSFET.

Maximum current, however, is not only determined by the maximum defined thermal dissipation, but also by
the maximum continuous drain current I<sub>D</sub> which may be much lower than expected from thermal design.
The reason is that when the thin wire bonds from die to packing (legs/case) are surrounded by a medium with
bad thermal conductance, they heat up much faster and burn – see [Continuous MOSFET drain](https://electronics.stackexchange.com/a/137546/135063)

#### References

* [How do I choose my optocoupler to drive a solenoid with a MOSFET?][bjt-vs-mosfet]
* [P-channel MOSFET][p-channel-mosfet] and power MOSFETs
* [MOSFET Thermal Characterization in the Application](Datasheets/71619_mosfet-thermal-characterization.pdf) (PDF)
* [Understanding Thermal Dissipation and Design of a Heatsink](Datasheets/slva462_understanding-thermal-dissipation.pdf)
* [Thermal Design Basics](http://www.analog.com/media/en/training-seminars/tutorials/MT-093.pdf)


#### Sample MOSFETs

Some TO-92 MOSFETs:

**2N7000**
[(pdf)](Datasheets/2N7000-short.pdf)
—
V<sub>DSS</sub> 60 V, R<sub>DS(on)</sub> 1.8 Ω, V<sub>GS(th)</sub> 2.1 V, I<sub>D</sub> 200 mA

**BS170**
[(pdf)](Datasheets/BS170-short.pdf)
—
V<sub>DSS</sub> 60 V, R<sub>DS(on)</sub> 1.2 Ω, V<sub>GS(th)</sub> 2.1 V, I<sub>D</sub> 500 mA

Larger MOSFETs; note that their legs do not fit into 2.54 mm PCB holes, but they can be soldered on top of PCBs.

**SiHU5N50D**
[(pdf)](Datasheets/sihu5n50d.pdf)
—
V<sub>DSS</sub> 500 V, R<sub>DS(on)</sub> 1.2 Ω, V<sub>GS(th)</sub> 3–5 V, I<sub>D</sub> 5.3 A

**IRL3103**
[(pdf)](Datasheets/irl3103.pdf)
—
V<sub>DSS</sub> 30 V, R<sub>DS(on)</sub> 12 mΩ, V<sub>GS(th)</sub> 1 V, I<sub>D</sub> 64 A

**IRL3803**
[(pdf)](Datasheets/irl3803.pdf)
—
V<sub>DSS</sub> 30 V, R<sub>DS(on)</sub> 6 mΩ, V<sub>GS(th)</sub> 1 V, I<sub>D</sub> 140 A

**IRF5305**
[(pdf)](http://www.irf.com/product-info/datasheets/data/irf5305s.pdf)
*P-Channel*
—
V<sub>DSS</sub> −55 V, R<sub>DS(on)</sub> 6 mΩ, V<sub>GS(th)</sub> −2…−4 V, I<sub>D</sub> −31 A

**IRF4905**
[(pdf)](http://www.irf.com/product-info/datasheets/data/irf4905s.pdf)
*P-Channel*
—
V<sub>DSS</sub> −55 V, R<sub>DS(on)</sub> 20 mΩ, V<sub>GS(th)</sub> −2…−4 V, I<sub>D</sub> −74 A

**IRFU9024**
[(pdf)](http://www.vishay.com/docs/91278/sihfr902.pdf)
*P-Channel*
—
V<sub>DSS</sub> −60 V, R<sub>DS(on)</sub> 280 mΩ, V<sub>GS(th)</sub> −2…−4 V, I<sub>D</sub> −8.8 A


[bjt-vs-mosfet]: https://electronics.stackexchange.com/a/43073/135063
[p-channel-mosfet]: http://www.sprut.de/electronic/switch/pkanal/pkanal.html


### Diodes

are conductive in one direction.
Normal diodes have a forward voltage V<sub>f</sub> of around 0.7 V, which increases with higher currents and lower temperatures.
Schottky and Germanium diodes have smaller forward voltages around V<sub>f</sub> = 0.3 V.


**1N4001** up to **1N4007** [(pdf)](Datasheets/1n4001.pdf) —
Rectifier diode, 50 to 1000 V, 1 W.
Forward voltage (voltage drop) is V<sub>F</sub> = 0.75 V @ 0.1 A and 1.1 V @ 1 A.

**1N4148** [(pdf)](Datasheets/1n4148.pdf) —
V<sub>F</sub> 1 V

**1N5817** to **1N5819** [(pdf)](Datasheets/1N5817-D.PDF) —
Schottky barrier rectifier diode. V<sub>F</sub> = 0.32 V @ 0.1 A and 0.45 V @ 1 A.


### Zener Diodes, Z Diodes

are used for overvoltage protection. Unlike normal diodes, they are mainly operated in reverse direction where the
breakdown voltage U<sub>BR</sub> is well-defined. Above this voltage, the diode conducts with a voltage drop of U<sub>BR</sub>.

A voltage of 3.3 V can therefore be maintained by a 3.3 V Zener diode.


### TRIACs

* [Triac Tutorial](https://www.electronics-tutorials.ws/power/triac.html)
* [Dimming 230 V AC with an Arduino](http://alfadex.com/2014/02/dimming-230v-ac-with-arduino-2/)
* [Triac Wattage](Datasheets/Triac-Wattage_gate_r.pdf)
* [Triac Application Note 3003](Datasheets/Triac_AN-3003.pdf)
* [RC Snubber Networks For Thyristor Power Control and Transient Suppression](https://www.onsemi.com/pub/Collateral/AN1048-D.PDF)
* [Thyristors and Triacs — Ten Golden Rules for Success in Your Application](http://www.chtechnology.com/pdf/AN-08-06%20SCR%20Application%20Notes.pdf)

Triacs allow to switch AC, as they are conductive in both directions in *on* state.
A Triac stays on as long as current flows through it. It switches off during
the AC’s zero crossing and can be turned on again at an arbitrary moment,
but it cannot be turned off while current flows.

They can e.g. be used for phase-fired control (German: Phasenanschnittsteuerung)
which allows to dim for example incandescent bulbs on 230 V AC.

Hi-com TRIACs are especially suited for switching purposes like AC dimmers.

**BT136-600** [(pdf)](Datasheets/BT136-600-NXP.pdf) —
4 A, 600 V

**MOC3021** [(pdf)](Datasheets/OEMOC301XM-MOC302XM_E.pdf) —
**MOC3052** [(pdf)](Datasheets/MOC3052M_ENG_TDS-short.pdf) —
Opto-Triac, provides galvanic decoupling and is used to drive larger Triacs.



### Varistors

are *variable resistors* whose resistance depends on the voltage. Above a certain voltage (like 30 V or 250 V),
resistance drops and the varistor conducts.

Compared to Zener diodes, varistors are less precise. They are also used for high voltage to e.g. protect against
lightning strikes where high voltages occur for a very short time.


## Relays

* [Relay Contact Life](Datasheets/RelayContactLife-ENG_CS_13C3236_AppNote_0513.pdf) and materials used in relays

Relays switch a contact by magnetic force, allowing a small voltage to control e.g. 220 V.

Since relays are mechanical switches, their switching speed is limited to e.g. 10 per second, and they also have
an expected life time both mechanically (switch breaks, e.g.) and electrically (switch burns due to sparks on contact).

Operating relays requires a certain amount of power. *Relay modules* operate the relay by amplifying the input signal,
and they often provide galvanic separation between signal and relay power.

**2-channel 5 V Songle Relay Module with Optocoupler**
[(pdf)](Datasheets/SRD-05VDC-SL-C-Datasheet.pdf) —
Common 5 V relay module which is often used with Arduinos, Raspis, etc., easily available.
10 A 250 V, life expectation 10⁵ (el.) / 10⁷ (mech.) operations for 1 op/second.

The relay is not operated directly; it can/should be powered by a second source (JD-VCC) which is galvanically
separated from the input signal. In this relay, a small input signal drives an optocoupler, which itself drives
the relay with a transistor.

Powering it with 3.3 V from a Raspberry *often* works when connecting VCC and JD-VCC.
If the current is not high enough (e.g. 16 mA from the Raspi), the LED IN1 will be on, but nothing happens.
Since relays are mechanical switches, it is often possible to trigger the switch by flicking against the relay,
which is naturally not a long term solution but a fun fact.

More reliable is to use separate 5 V for JD-VCC. The relay switches when IN0 is *low.*

![Relay Module](Datasheets/5v-relay-module-songle.jpg)


### Capacitors

* [Capacitor Guide](http://www.capacitorguide.com/)
* [Decoupling capacitors](https://electronics.stackexchange.com/a/90972/135063)
* [Decoupling Tutorial](http://www.thebox.myzen.co.uk/Tutorial/De-coupling.html)

Capacitors store and release energy quickly. There are two different categories:

Polarised capacitors have a cathode and an anode and break when reversely charged.

* Tantalum electrolyte: Labels like `476` mean 47·10⁶ pF.
* Aluminum electrolyte

Non-polarised:

* Film capacitor

Two frequent decoupling use cases for capacitors:

* Between GND and VCC of a microcontroller to remove high-frequent noise from
  the power supply. Noise may cause the MC to malfunction in various ways.
* In circuits which draw PWM loads, like PWM dimmed power LEDs.
  Without a capacitor, there is PWM noise on the whole supply rail.

Ceramic capacitors are rated by classes which define their temperature stability,
tolerance, and other values. X5V, X7R etc. are Class 2 capacitors whose capacity
is generally larger, but changes in a non-linear way with temperature.
NP0 and others are Class 1 capacitors which have a linear temperature response,
where the capacity of NP0 does not change at all; they are useful for accurate
frequency generators.
See [Ceramic capacitors](https://en.wikipedia.org/wiki/Ceramic_capacitor#Application_classes,_definitions) on Wikipedia.
Ceramic capacitors also react to vibration due to a piezoelectric effect; watch
[Which Capacitor Do I Use? Tech Tips Tuesday](https://www.youtube.com/watch?v=67M7fsbLUIU).

[Tantalum capacitors](http://www.capacitorguide.com/tantalum-capacitor/)
react sensitively to voltage spikes above their rating
and may go into thermal runaway aka. smoke and flames.

[Film capacitors](http://www.capacitorguide.com/film-capacitor/) are bulkier
than other capacitors, but they are stable over time – their capacity
does not change much – and [self-healing](https://en.wikipedia.org/wiki/Film_capacitor#Self-healing_of_metallized_film_capacitors).


## Useful ICs

### I²C

is a common protocol for sending data over two wires (common ground is required too).
It is supported by many devices like ICs, Raspberry, Arduinos, etc.

**PCF8574P** and **PCF8574AP** —
[(pdf)](Datasheets/PCF8574.pdf)
I²C based port expander. Connect SDA (data) and SCL (clock) to the Raspi I²C pins (5 and 7) for 8 additional digital IOs.

Both ICs have 3 address bits/pins, so 8 of them can be used. The P and AP differ only in their
slave address, so when used together, 16 of them can be attached (resulting in 128 IOs).

The IOs are quasi-bidirectional. An IO is either configured LOW, in which case it functions as open drain (“output”) to GND.
When configured HIGH, a 100 µA current source to V<sub>DD</sub> is active and the IO functions as input.

**MCP23008** and **MCP23017**
(PDF: [23008](Datasheets/MCP23008_21919e.pdf),
[23017](Datasheets/MCP23017_20001952C.pdf))
—
I²C based port expander, also available as SPI version. MCP23008 has 8 ports in DIP-18, and MCP23017 has 16 ports in DIP-28.

IOs can be configured individually as input or output. Outputs can source/sink 25 mA each (i.e. both when HIGH or LOW).
They have internal pull-up resistors which can be enabled.
Note that the ¬RST pin needs to be connected to V<sub>DD</sub> unless it needs resetting.

The MCP23017 ports are divided into the two banks A (ports 0–7) and B (ports 8–15) which are addressed separately.
Addressing of registers can be changed by setting IOCON.BANK, which defaults to 0 (GPIO addresses are on 0x12 and 0x13).

The SPI version MCP23S08 uses mode 0, the address bits are by default deactivated,
but can be enabled with the IOCON register. To read a byte, first send the
two normal bytes (opcode and register) and then an additional byte. This byte
is ignored, but the clock is still running and the byte can be clocked out
from the device (i.e. the MCP23S08 can send the result while the master sends
a random byte).

The [MCP23016](Datasheets/MCP23016_20090C.pdf) is deprecated.

See also:

* [Tutorial: Why use MCP23008 / MCP23016 / MCP23017 expanders][mcpxx-tut]
* [How to get pin addresses on a MCP23017][mcp23017-banks] (StackExchange)

[mcpxx-tut]: http://peter224722.blogspot.ch/2014/03/why-use-mcp23008-mcp23016-mcp23017.html
[mcp23017-banks]: https://raspberrypi.stackexchange.com/questions/7205/how-to-get-pin-addresses-on-a-mcp23017

### Optocouplers

drive higher voltages with a lower-voltage input signal. For example, control a 24 V LED button with a 3.3 V signal.

Optocouplers separates two voltages (galvanic isolation) by driving a phototransistor with an infrared LED.
So, in addition to amplifying a signal, the output voltage can also be at a completely different level.

The CTR (Current Transmission Ratio) of an optocoupler is defined as I<sub>C</sub>/I<sub>F</sub>. For a CTR of 100 %,
a forward current of 20 mA through the LED will allow a collector current of 20 mA through the phototransistor.

The LED inside optocopulers decreases over time, and more quickly at high temperatures.

References:

* [How to Use Optocoupler Normalized Curves](Datasheets/Howto-Optocoupler-normalized-curves_83706.pdf)
* [Optocoupler Switching Time Analysis](http://www.cel.com/pdf/appnotes/an3009.pdf)

**PC817**
[(pdf)](Datasheets/PC817C.pdf)
—
DIP-4, CTR 50–140 %.
Max 3 V 50 mA in, 35 V 50 mA 150 mW out. Rise/fall times t<sub>r</sub>, t<sub>f</sub> = 4–18 µs.
Easily available and cheap.

**4N25**
[(pdf)](Datasheets/4n25.pdf)
—
DIP-6. CTR 20–50 %. Rise/fall times t<sub>r</sub>, t<sub>f</sub> = 2 µs.
The phototransistor base is accessible (must be connected) and can be used e.g. to switch off faster;
see [Optocoupler with phototransistor base lead][optocoupler-base-pin] and
[Optocoupler Applications][optocoupler-applications].

**4N35**
[(pdf)](https://www.vishay.com/docs/81181/4n35.pdf)
—
Like the 4N25, but with a CTR > 100 %.

**6N137**
[(pdf)](Datasheets/6N137.pdf)
—
DIP-8, 15 mA, t<sub>r</sub> 50 ns

**ILD2**, **ILQ2**
[(pdf)](http://www.vishay.com/docs/83646/ild1.pdf)
—
Dual/quad channel optocouplers, DIP-8 and DIP-16.

**617** — More optocouplers

[optocoupler-base-pin]: https://electronics.stackexchange.com/questions/48462/optocoupler-with-phototransistor-base-lead
[optocoupler-applications]: [Datasheets/designing-for-optocouplers-with-base-pin.pdf]

### Op-Amps

are transitor based circuits. An op-amp also amplifies the signal, but with additional benefits like a temperature
independent, and generally higher, h<sub>fe</sub>. Ideally, they have infinite input inpedance, i.e. they are
voltage controlled. In reality, a small *input bias current* I<sub>IB</sub> does flow.
The NE5532, for example, has an I<sub>IB</sub> of 1 µA.

Audio OpAmps are specially designed for audio applications, featuring higher
output current in general to drive loudspeakers, and constant output current
regardless of the output voltage to avoid distortion.

References:

* [Op Amp Input Bias Current][opamp-bias-current-pdf]
* [Op Amp Input Bias Current][opamp-bias-current]

**NE5532**
[(pdf)](Datasheets/ne5532-short.pdf)

**LM358** [(pdf)](Datasheets/lm358_lm158-n-short.pdf) –
20 mA OpAmp, 100 dB

**LM386** [(pdf)](Datasheets/lm386-short.pdf) —
125 mW Audio OpAmp

[opamp-bias-current-pdf]: http://www.analog.com/media/en/training-seminars/tutorials/MT-038.pdf
[opamp-bias-current]: http://ecircuitcenter.com/Circuits/op_ibias/op_ibias.htm


### H-Bridge

H bridges provide bidirectional high-current output. This is used e.g. for controlling
stepper and DC motors and relays. DC motors, for example, change their direction
when switching the polarity of the applied voltage.

Outputs can be set to low or high, so they either source or sink current, and
they can be disabled, putting them into high impedance state. A motor is stopped
by putting both outputs to low, and is running freely when the outputs are in
high impedance (disabled).

**L293D** [(pdf)](Datasheets/l293d.pdf) —
600 mA 36 V


### Comparators

**LM393** —
[(pdf)](Datasheets/lm393-n.pdf)
Comparator; compares two voltages and sets the output low or high, depending on which input was higher.

### Counters

**NE555**
[(pdf)](Datasheets/ne555-short.pdf)
—
Decade counter. Can generate frequencies from mHz to 100 kHz, which can e.g. be
used to generate a 38 kHz carrier for IR diodes. Two resistors and one capacitor
are required for this, with R<sub>B</sub> = (2C)⁻¹ (1.44/f − R<sub>A</sub>C).
38 kHz can be achieved with R<sub>A</sub> = 1.2 kΩ, R<sub>B</sub> = 12 kΩ, C = 1.5 nF.

**CD4017B** — Decade counter, 10 inputs

### Logic operators

**74HCT08** —
3 V to 5 V level shifter. Actually only an AND, but with V<sub>DD</sub> = 5 V and V<sub>IH</sub> = 2 V it can be used
to shift 3.3 V from the Raspi (or from elsewhere) to a 5 V signal.

Compared to optocouplers, the 74xx are much more efficient with I<sub>I</sub> = 1 µA as there is no need to power a LED,
but V<sub>CC</sub> is only 4.5 to 5.5 V.

Also, their response time is a lot shorter; around 40 ns compared to 20 µs for a PC817.

[The 74xx Series](https://www.mikrocontroller.net/articles/74xx) contains many logic chips. Some data sheets:
* [SN74HCT00 (pdf)](Datasheets/sn74hct00-short.pdf) is a 2-input NAND (25 ns max.; 9 ns max. for AHCT version)
* [SN74HCT08 (pdf)](Datasheets/sn74hct04-short.pdf) provides 6 NOT inverters (19 ns max.)
* [SN74HCT08 (pdf)](Datasheets/sn74hct08-short.pdf) is a 2-input AND (30 ns max.; 9 ns max. for AHCT version)
* [SN74AHC125 (pdf)](Datasheets/sn74ahct125-short.pdf) is a fast 3-state buffer (10 ns max.)


### Voltage Regulators

**LM3940** [(pdf)](Datasheets/lm3940.pdf) —
Converts 5 V to 3.3 V. Basically by means of converting electrical to thermal energy.
3.3 V, 1 A; U<sub>in</sub> 4.5 to 5.5 V.


### LED control

**AL8861** [(pdf)](Datasheets/AL8861-1094677.pdf) —
Current control for LEDs


### RS485

**MAX485** [(pdf)](Datasheets/RS485_MAX1487-MAX491.pdf)


### A/D converter

**MCP3204** [(pdf)](Datasheets/MCP3204_MCP3208_MIC.pdf) —
4/8 channel, 12 bit, SPI


### IR receiver

**TSOP4838** [(pdf)](Datasheets/tsop48.pdf) —
**TSOP31238** [(pdf)](Datasheets/tsop312.pdf) —
IR receivers for IR signals modulated at 38 kHz (other models of this series are
available for different frequencies). IR controls modulate the HIGH pulses at
high frequencies like 38 or 40 kHz to distinguish between natural IR emissions
like sunlight and IR control pulses. These receivers contain a band-pass filter,
so all frequencies that lie outside of this band are filtered away.

More information about IR data formats, like NEC and RC 5 codes, in Vishay’s
[Data Formats for IR Remote Control][dataform]
[(pdf)](Datasheets/ir-remote-control_dataform.pdf).

[dataform]: http://www.vishay.com/docs/80071/dataform.pdf


## Toys


### Stepper Motors

* [Stepper Motor Troubleshooting](https://www.element14.com/community/thread/35078/l/stepper-motor-troubleshooting)
* [Stepper Motors with Arduino](https://dronebotworkshop.com/stepper-motors-with-arduino/)
* [Step Motors—Troubleshooting basics, part 1](https://www.designworldonline.com/Step-MotorsTroubleshooting-basics-part-1/) and [Troubleshooting step motors: part 2](https://www.designworldonline.com/troubleshooting-step-motors-part-2/)

Running stepper motors from 0 to full speed is a bad idea as a lot of current
is drawn in the beginning. Instead, it needs to be ramped up smoothly.

### Dot-Matrix LCD displays

LCD Displays with pixels, also available with backlight.

**HD44780**
controls dot matrix LCD displays. It requires 6 IOs.

### 7-Segment LCD/LED displays

    ╷╷ ╶┐
    └┤ ┌┘
     ╵ └╴
Typically, each segment of those displays is enabled with a separate input. One element therefore requires 8 pins
(7 segments + GND or V<sub>CC</sub>).

Displays with more (e.g. 4) elements reduce the number of pins by adding *element enable pins.* Only one element
is active simultaneously, but when switching the active element fast enough, it appears to the eye as if all elements
were active. 7-segment displays [can easily be controlled by a Raspberry][7-segment-pi].

**MAX7219**
controls 8 elements and uses 2 inputs, clock and data.

**SBA32-11EGWA**
[(pdf)](Datasheets/Kingbright-SBA32-11EGWA.pdf) is a large segment display with red *and* green (and, together, orange).


### LEDs

**WS2812B** —
[(pdf)](Datasheets/28085-WS2812B-RGB-LED-Datasheet.pdf)
RGB LED with integrated chip (or the other way round), PWM controlled. Usually on LED stripes which can be cut off anywhere.
LEDs take 60 mA when all 3 colours are on, V<sub>DD</sub> = 5 V. Data (D<sub>IN</sub> and D<sub>OUT</sub> PINs) have
logic levels 0.3 V<sub>DD</sub> and 0.7 V<sub>DD</sub> and require 1 µA.
WS2812B is the [improved version of the WS2812](https://acrobotic.com/datasheets/WS2812B_VS_WS2812.pdf).

Since the signals sent on the data pin are around 400 ns, it is not possible to use
standard optocopulers for shifting a 3.3 V  to 5 V. They have a response time of
several 1000 ns. A 74HCT08 can instead be used, or a fast optocoupler like the
6N137 with an output rise time of 50 ns (but requires inverting the signal).

The following image shows the same WS2812B signal directly from the Raspi (blue) and after an optocoupler (red).
Quite obviously, it is not quite the same curve. The PC817 is too slow to reproduce the signal

![PC817](Pictures/ws2812-data-after-pc817.png)

After the faster SN74HCT08, the signal is reproduced almost identically, but at a level of 5 V.

![SN74008](Pictures/ws2812-data-after-sn74hct08n.png)

6N137 example (red is input):

![6N137](Pictures/6n137-inverted-output.png)

To get it working on a Raspi directly, use [rpi-ws281x-native](https://www.npmjs.com/package/rpi-ws281x-native).
Level shifting to 5 V is required, otherwise artifacts (wrong pixels) will occur.
This method is very fast and writes about 24000 LEDs per second on a 60 LED strip (400 fps).

Another solution is to feed colour data from the Raspi to an Arduino. Arduino’s microcontroller then sends data to  WS2812.
Use [node-pixel][pixel] and flash the Arduino (I use an Arduino Pro Mini) [with the Backpack firmware][pixel-backpack];
it then listens on I²C on pins A4 and A5, which may have to be soldered first.
On the Raspi, connect to the Arduino with Johnny-Five [and raspi-io][pixel-raspi-io].
This method is slower with around 740 LEDs per second, which is 12 fps for 60 LEDs.


[7-segment-pi]: http://raspi.tv/2015/how-to-drive-a-7-segment-display-directly-on-raspberry-pi-in-python
[pixel]: https://www.npmjs.com/package/node-pixel
[pixel-backpack]: https://github.com/ajfisher/node-pixel/blob/master/docs/installation.md#i2c-backpack-installation
[pixel-raspi-io]: https://github.com/ajfisher/node-pixel/issues/68#issuecomment-232822499


## EL Wire

* [How to solder or repair EL wire](http://elwirecraft.co.uk/el-how-to-and-tips/how-to-solder-or-repair-el-wire/)
* [The Full "How To" Manual for EL (Electroluminescent) Wire](http://www.instructables.com/id/The-Full-How-Too-Manual-For-EL-Electroluminesce/) (See comment by MTJimL)


## Random links

* [3V and 5V Tips/Tricks (pdf)](http://www.newark.com/pdfs/techarticles/microchip/3_3vto5vAnalogTipsnTricksBrchr.pdf)
