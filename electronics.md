# Electronics

## Fuses

* [ESKA: Technische Einführung](http://eska-fuses.de/fileadmin/pdf/content/Technische_Einfuehrung.pdf)

## Useful ICs

**PCF8574P** and **PCF8574AP** — I²C based port expander. Connect SDA (data) and SCL (clock) 
to the Raspi I2C pins (5 and 7) for 8 additional digital IOs. 

Both ICs have 3 address bits, so 8 of them can be used. The P and AP differ only in their 
slave address, so when used together, 16 of them can be attached (resulting in 128 IOs).

**MCP23008** and **MCP23016** — I²C based port expander. More features, but did not work for me.

**PCF817** — Optocoupler. Separate two voltages. 3 V 50 mA in, 35 V 50 mA 150 mW out.

**4N25** — Optocoupler.

**ILD2**, **ILQ2**, **617** — More optocouplers

**NE555** — Decade counter.
