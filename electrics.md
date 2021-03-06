# Electrics and hardware parts

## AC Power

![3-phase AC power from generator](Pictures/three-phase-ac-generator.png)

3-phase electric power is created by the generator. The phases are shifted by 120° each.

![3-phase AC power, usage](Pictures/three-phase-ac-voltages.png)

In the star/Y configuration, the coils are connected at the neutral point (N)
and the neutral line. Each phase provides 230 V AC relative to N, or 400 V
relative to each other.


## Plugs

[In Switzerland/EU](https://de.wikipedia.org/wiki/Niederspannungsnetz#Farbgebung), old and new colours

```
N       L
o       o
    o
   PE
```

* **N** *(blue, old: yellow)* Neutralleiter
* **L** *(brown, old: black)* Aussenleiter, Phase
* **PE** *(yellow+green, old: yellow+red)* Schutzleiter, Protective Earth


## Wires

Wire shielding: [Control Systems][shielding], page 26.

Rough numbers

    0.75 mm²  15 A
    1    mm²  19 A
    1.5  mm²  24 A

Influence of number of strands on maximum current

     5 → 75 % of the above single-strand value
     7 → 65 %
    10 → 55 %

Copper strand<sup>[1][strombelastbarkeit]</sup>

     Diameter   Amp  AWG    Usage
     ------------------------------------------------
               .3 A  32
     0.05 mm²   1 A  30     Thin flat-band connectors
     0.09 mm²        28     Thin *flat ribbon cables* (available up to 22 AWG)
     0.14 mm²   2 A  26     Often used in dupont wires
                     24     Also popular for dupont wires
     0.25 mm²   4 A  23     Single-strand wire that fits into breadboards
     0.34 mm²   6 A  22
     0.5  mm²   9 A  20     Loudspeaker cable
     0.75 mm²  12 A  18.5   Often used for multi-strand power cable
     1.00 mm²  15 A  17     Often used for single-strand power cable for electrical installations


[strombelastbarkeit]: http://www.linzi.hu/Katalogus/2008-2009/ger/X%20028%20%20Strombelastbarkeit%20(allgemein).pdf
[shielding]: https://support.automationdirect.com/docs/controlsystemdesign.pdf


### Wire Descriptors

Terms like H03VV-F or LiYCY: See
[Reicheltpedia: Leitungen](https://www.reichelt.de/reicheltpedia/index.php5/Leitungen)

DIN VDE 0250 describes wires by starting from the innermost material.
LiYCY is a wire with strands in a PVC mantle followed by a copper shield and again
a PVC mantle.

C = Schirm aus Kupferdrahtgeflecht
Cu = Kupfer
e = eindrähtig
F = feindrähtig, Flachleitung
I = Verlegung im Putz
L / Li = Litze
M = Mantelleitung
N = Normleitung
R = Runddraht, Rundleitung
(St) = statischer Schirm
Y = PVC, Polyvinylchlorid
Z = Zwillingsleitung, Zwillingsader


### Making wires more flexible

Besides choosing a wire which *is* more flexible due to thinner strands or silicone coating, wires can also be
wrapped around a fabric core. Some examples on [StackExchange: Wire for harsh environment](https://electronics.stackexchange.com/a/164551/135063)


## Connectors + Wires

Connector wires can be bought, but one always ends up making custom ones (different length e.g.). I suggest using
pliers for crimping, which yields good results with a bit of practice. The original crimping tools usually cost hundreds
of dollars and are good for professional use.

Connectors should not be soldered. See [Common wire-to-board, wire-to-wire connectors, and crimp tools][crimp-connectors].

References:

* [Connector Basics](https://learn.sparkfun.com/tutorials/connector-basics) at sparkfun

### JST RCY

* [Data Sheet](http://www.jst-mfg.com/product/pdf/eng/eRCY.pdf)
* 28–22 AWG (0.08–0.3 mm²), 3 A
* Outside wire diameter < 2 mm

The RCY is a wire-to-wire connector, i.e. it is designed for connecting two wires. The plug (male) pins are protected
by the housing (no accidential short-circuiting something).

The pins on RCYs are numbered; pin 1 is typically used for GND.

### JST SM

* [Data Sheet](http://www.jst-mfg.com/product/pdf/eng/eSM.pdf)
* 28–22 AWG (0.08–0.3 mm²), 3 A

The male version uses the same terminal as the RCY connector.

### DuPont Mini-PV / Molex SL

* 36–32 AWG and 30–24 AWG (depending on crimp terminals), 3 A

The typical black connectors are originally produced by DuPont and called Mini-PV. Nowadays, they are known as
DuPont connectors and produced by [Molex][molex-catalogue] (SL line) and many others. Originally, they are wire-to-board
connectors, and a plug (male) version does not exist; only a receptor (female) for jumpers was produced.

Pin headers on PCBs are often connected with Dupont wires. I ended up crimping hundreds of them. This video
gives a quite good understanding:
[Custom Cables & Guide to Crimping Dupont PCB Interconnect Cables](https://www.youtube.com/watch?v=GkbOJSvhCgU).
I am using pliers and no special crimping tools, which works just as well with a bit of practice.

For crimping, the components can be found on ebay:

* *(fe)male dupont crimp pin header*; usually come on a band where they have to be cut off with a side cutter,
  convenient for storage.
* *dupont pin housing 1P (2P, 3P, …)*; The pin housing work for both male and female pins, there is no difference.
  1P denotes housings for a single pin, 2P for two pins, etc. I use 2P most often, then 1P, 4P, and 3P.


[molex-catalogue]: http://www.molex.com/catalog/web_catalog/pdfs/C.pdf
[crimp-connectors]: http://tech.mattmillman.com/info/crimpconnectors/


## Wire Ferrules

![Rusted strands](Pictures/power-plug-rust.jpg)

Wire ferrules (*Aderendhülsen* in German) are crimped at the end of stranded wire and prevent oxidation and physical
damage (e.g. by screws) to the strands. A bad example where no wire ferrule was used can be seen in the picture.


## Switches

Single Pole, Double Pole, Single Throw, Double Throw? See [SPST, SPDT, and DPDT Switches Demystified (pdf)][spst]

[spst]: http://musicfromouterspace.com/analogsynth_new/ELECTRONICS/pdf/switches_demystified_assembly.pdf


## LED Strips

The maximum LED strip length is limited by the thickness of the copper wires on it as the thin copper layers have
rather high internal resistance, causing the strip to heat up. To power longer strips, re-supply them e.g. after
every 5 m.


## Tools

![ESD screwdriver](Pictures/esd-vde-screwdriver.jpg)

Tools can be available for different purposes:

* ESD tools are slightly conductive to prevent electrostatic discharge. They are designed for
  working with electrical equipment which could be damaged by ESD. As ESD tools are conductive,
  they *must not* be used on systems which are powered on.
* Insulated tools (VDE in German) *can* be used to work on live systems with a voltage of e.g.
  up to 1000 V.
* Normal tools are usually neither ESD conductive nor do they guarantee any voltage protection.
