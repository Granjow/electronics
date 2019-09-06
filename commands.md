
MCP23008
For address 0x20

Bits:
IO7 IO6 IO5 IO4 …

IODIR 0x00
1 = input

Set IO4 to IO7 as outputs: (First byte is for IOs, second byte is ignored)
i2cset -y 1 0x20 0x90


GPIO  0x09
1 = logic high



ATtiny85

RST to 3v3
