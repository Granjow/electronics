# Controlling Actuators

## Controlling a Beamer with a Raspberry or Arduino

There are two options:

* **Infrared** – send the same codes that a remote control would send. One-way
  communication only, there is no way to validate if the command succeeded.
* **Serial RS232** – communicate over serial wire. More stable and allows to
  query the beamer running state, but requires a cable.


### Infrared beamer control

Some important facts beforehand:

* IR signals for remote controls are usually modulated on a carrier of 38 kHz
  (and some other frequencies). The carrier frequency allows to distinguish
  between other IR sources like e.g. sunlight which would otherwise interfere.
* There are special IR receiver diodes like the [TSOP 4838][tsop] which only
  receive signals on a 38 kHz carrier, other signals are filtered. They output
  the original signal without the carrier.
* The sender is a normal 940 nm IR diode, LIRC will take care of modulation.

The typical steps for a Raspberry are:

1. Set up lirc
2. Capture signals from the original remote control (or find codes online)
3. Send IR data using this configuration


#### lirc configuration

Install `lirc` and configure the device tree overlay; the GPIO pins for sending
and receiving are configured here as well.

`/boot/config.txt` entry for LIRC:

    dtoverlay=lirc-rpi,gpio_in_pin=18,gpio_out_pin=15,debug=true

Configure the LIRC driver and set it to `default` to allow sending IR codes –
see [LIRC won’t transmit][lirc-send].

`/etc/lirc/lirc_options.conf` configuration:

    driver          = default

Now LIRC can be used to manually send and receive data.

```
systemctl stop lircd
mode2 -d /dev/lirc0 -H default          # Receiver
irrecord --list-namespace               # List namespaces (default button names)
irrecord -d /dev/lirc0 -H default OUT   # Record configuration file
irsend LIST DEVICE_NAME ""              # Read commands from /etc/lirc/lirc.conf.d/*.conf
irsend SEND_ONCE DEVICE COMMAND         # Send commands
```


#### IR electronics

Very little harware is required:

* For receiving, connect the output pin of the TSOP4838 (or equivalent) to the
  LIRC input pin.
* For sending, connect the LIRC output pin to an IR diode. Note that this needs
  a proper setup with a transistor like 2N4401 because standard IR diodes
  consume 100 mA, which cannot be supplied by a GPIO pin.


#### Further references

* https://www.einplatinencomputer.com/raspberry-pi-infrarot-empfaenger-und-fernbedienung-einrichten/
* https://indibit.de/raspberry-pi-mit-lirc-infrarot-befehle-senden-irsend/
* http://www.righto.com/2009/08/multi-protocol-infrared-remote-library.html
* http://irdb.tk/


[tsop]: Datasheets/tsop48.pdf
[lirc-send]: https://raspberrypi.stackexchange.com/a/73070/57569


### Serial RS232 beamer control

Hardware requirements:

* Serial cable connecting to the beamer
* Raspberry + manual testing: USB to serial converter, for example a *Digitus
  DA-70156* with the FTDI 232RL chipset
* Arduino: Anything that can output RS232 voltages, e.g. [MAX3232][max]

The steps to get communication running are as follows:

1. Find the data sheet for the beamer’s serial command set, like the [Acer
   RS232 User Manual][acer]
2. Send commands and examine the responses


#### Manual testing with `minicom`

Minicom is a serial terminal which allows to type the commands manually. Install `minicom` and connect the beamer with the USB-to-RS232 adapter; the adapter should show up as `/dev/ttyUSB0` or similar. Then, run minicom:

```bash
# Normal mode
minicom -b 9600 -D /dev/ttyUSB0 -o
# Hex output
minicom -b 9600 -D /dev/ttyUSB0 -o -H
```

Minicom needs some configuration:

```
Ctrl+A O → Serial Port Setup → Disable hardware flow control
Ctrl+A O → Screen and Keyboard → Line Wrap
Ctrl+A E   (Enable echo, i.e. see what you type)
```

The combination Line Wrap and Hex Mode is especially useful as it ensures that
the response of the beamer is always printed on the next line (also if only a
CR carriage return is sent) and the exact character codes are visible (e.g.
`\r` and `\n` can easily be distinguished).


Things to consider:

* Communication is sometimes funny. For example, the Acer H5382BD sends two
  lines in reponse to a status query.
* Line endings matter both for sending and for parsing! Many serial protocols
  use CR, others use CRLF, etc.


#### Beamer Control with a Raspberry Pi

The [serial-half-duplex][shd] NPM package provides the required functionality.


#### Example communication with H5382BD

The beamer response is terminated with CR (`0x0d`). Note that querying for the
lamp state returns two lines, and the answer is case sensitive and also shows
the warmup state. This is not described in any available manual online.

```
(Beamer off, send: * 0 Lamp ?)
*000
LAMP 0

(Beamer off, send: * IR 001)
*001

(Beamer warming up, send: * 0 Lamp ?)
*00
Lamp 0

(Beamer on, send: * 0 Lamp ?)
*000
Lamp 1

(Beamer on, send: * IR 002)
*001
```


[max]: Datasheets/RS232_MAX3222-MAX3241.pdf
[acer]: Datasheets/RS232_User_Manual_Acer_1.0_A_A.xls
[shd]: https://www.npmjs.com/package/serial-half-duplex


