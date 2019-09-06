# Communication Protocols

## SPI

Bidirectional.

MCP23S08 example for reading bytes: Send one additional byte.

```
  Opcode       Register 0   Random byte
→ 0b01000001 - 0b00000000 - 0b00000000

  (   IC is listening    )  Result
← 0b00000000 - 0b00000000 - 0b11000101
```

![MCP23S08 with SPI](Pictures/pulseview-spi-mcp23s08.png)


## DMX

[DMX Shield](http://www.mathertel.de/Arduino/DMXShield.aspx) with MAX485
can be used with an Arduino.

Arduino test with a DMX controlled LED light:
Using `delayMicroseconds()` provides stable results, with `delay()` however it flickers.
Also using slow functions like `sin(millis())` causes flickering.

MAX485

* Pin 6 → XLR pin 3
* Pin 7 → XLR-Pin 2


* [DMX512-A](https://opendmx.net/index.php/DMX512-A)
* [HiFiBerry DAC+ Pro XLR konfigurieren](https://www.hifiberry.com/build/documentation/configuring-linux-3-18-x/)
