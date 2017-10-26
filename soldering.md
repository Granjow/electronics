# Soldering


## Soldering howtos

* [Professionelles Löten von Lochrasterplatinen](http://docplayer.org/5770969-Professionelles-loeten-von-lochrasterplatinen.html)

### Soldering Temperature

The soldering temperature depends on the type of solder (lead free or not, for example) and also
on the size of the object to solder. When the temperature is right, a clean solder takes around 2–3 seconds for through-hole PCBs
and 1–2 s for SMDs.

Use the lowest temperature which allows to solder cleanly. If it takes several seconds to heat legs until the solder melts,
the temperature is too low.

I personally use lead-free solder, which is just as easy to solder with as the old solder with lead. Also, if you
accidentially breathe in the soldering fumes, it better be lead free as lead is quite poisonous. 

I solder at 325 °C usually, for pin connectors or for heating solder bridges 335 °C, for soldering even larger Chinch plugs 350 °C
or more (they should also accept the solder within 3 seconds at most), and for removing insulation of coated copper wires 400 °C.

Those numbers *depend on the soldering station* and are only thought as a rough guidance. Soldering stations have
different temperature control, the heat transfer (or, heat loss) to the soldering tip is different, the solders differ, etc.


## Soldering coated copper wire

Coated copper wire (or magnet wire) is copper wire with a very thin insulation layer. Due to the insulation coating,
wires can cross each other without conducting, which is very useful for slightly more complex PCB wiring.

To solder coated copper wire, the insulation has to be removed. This can be done physically with specialised tools,
but usually the coating burns at a (specified) temperature around 400 °C. Put some solder on the soldering tip, heat it 
to 400 °C, and dip the end of the coated copper wire into the hot solder; the coating evaporates.

At lower temperatures, the solder does not stick to the coating.

After removing the coating, solder at normal temperatures (around 330 °C for Pb free solder).


## Soldering DIP ICs

From around DIP-6 and above, consider using DIP sockets which are soldered first.
The IC can then be plugged onto the socket. Benefits: it is not heated while soldering,
and it can easily be replaced if broken or removed/salvaged if not required anymore.

![DIP sockets](Pictures/dip-sockets.jpg)

DIP sockets can be soldered next to each other, and ICs can span multiple sockets without a problem, so if you do not
have the correct size, combine some sockets and you are fine.

# MOSFETs

Larger MOSFETs have a hole in their heatsink which allows them to be mounted onto an even larger heatsink.
ON Semi provides a in-depth [manual on soldering MOSFETs][onsemi-mosfet], 
and Vishay has [guidelines on handling failed MOSFETs][vishay-mosfet].

[onsemi-mosfet]: https://www.onsemi.com/pub/Collateral/SOLDERRM-D.PDF
[vishay-mosfet]: http://www.vishay.com/docs/71436/an839.pdf