# Raspberry Pi 3

[Chip](https://www.raspberrypi.org/documentation/hardware/raspberrypi/bcm2837/README.md): BCM3827, Quad-Core ARMv8

## GPIOs

* GPIO: [Pinout.xyz](https://pinout.xyz/) provides a nice map of pins
* SX: [Raspi Power Limitations](https://raspberrypi.stackexchange.com/a/51616/57569)
* [GPIO Electrical Specifications][gpio-specs] (Not necessarily for Raspi 3, but generally interesting)
* [rpio freezes](https://github.com/raspberrypi/linux/issues/2550): Add `dtoverlay=gpio-no-irq` to `/boot/config`

The Raspi has 40 GPIO pins. **24 pins** can actually be used for GPIOing.

```
5533GGGGGGGGiiee00OOOOOOOOOOOOOOOOOOOOOO
│ │ └GND (8)│ │ └IOs (24)
│ └──3.3V   │ └──EEPROM reserved  
└────5V     └────I²C with Pull-Up
```

Of the other 16 pins, there are

* 2× 5 V pins
* 2× 3.3 V pins
* 8× GND pins
* 2× I²C pins with pullup (Physical pins 3, 5)
* 2× Reserved I²C EEPROM pins (Physical pins 27, 28)

Pinout of available pins, `~~` are available:

    02 04 06 08 10 12 14 16 18 20 22 24 26 28 30 32 34 36 38 40
    5v 5v 0v ~~ ~~ ~~ 0v ~~ ~~ 0v ~~ ~~ ~~ xx 0v ~~ 0v ~~ ~~ ~~
    3v xx xx ~~ 0v ~~ ~~ ~~ 3v ~~ ~~ ~~ 0v xx ~~ ~~ ~~ ~~ ~~ 0v
    01 03 05 07 09 11 13 15 17 19 21 23 25 27 29 31 33 35 37 39

### Electrical Specifications

**Input** is 5±0.25 V, Raspi draws 8–9 W; with periphery (mouse, keyboard, other USB devices) up to 15 W.

**GPIOs** can provide 16 mA each. The 3.3 V pin can provide up to 800 mA. The 5 V pin can provide what remains of the input power (input current is 2.5 A); i.e. minus USB, HDMI (50 mA), GPIOs etc.

Logic levels are V<sub>IL</sub> = 0.8 V (lower voltages will be recognized as LOW), V<sub>IH</sub> = 1.3 V (higher voltages will be recognized as HIGH).

GPIOs have internal pull-up/pull-down resistors with 1.8 kΩ (probably) which can be enabled individually.
As a result from the logic voltage levels, setting a pin with pull-down to HIGH requires a resistor smaller than `1800 Ω · 0.8 V / 2.5 V = 576 Ω` (voltage divider).

For external pull-up/pull-down resistors, [use 10 kΩ](https://www.raspberrypi.org/forums/viewtopic.php?f=44&t=46525#p366259).
For setting IOs to LOW/HIGH, use 1 kΩ; this will result in a current of 3.3 mA.

**USB** can provide 1200 mA.

### GPIO debugging

Read/write GPIOs. `gpio` works with the `wPi` WiringPi GPIO numbers!

```
~> gpio readall
 +-----+-----+---------+------+---+---Pi 3---+---+------+---------+-----+-----+
 | BCM | wPi |   Name  | Mode | V | Physical | V | Mode | Name    | wPi | BCM |
 +-----+-----+---------+------+---+----++----+---+------+---------+-----+-----+
 |     |     |    3.3v |      |   |  1 || 2  |   |      | 5v      |     |     |
 |   2 |   8 |   SDA.1 | ALT0 | 1 |  3 || 4  |   |      | 5V      |     |     |
 |   3 |   9 |   SCL.1 | ALT0 | 1 |  5 || 6  |   |      | 0v      |     |     |
 |   4 |   7 | GPIO. 7 |   IN | 0 |  7 || 8  | 0 | IN   | TxD     | 15  | 14  |
 |     |     |      0v |      |   |  9 || 10 | 1 | IN   | RxD     | 16  | 15  |
 |  17 |   0 | GPIO. 0 |   IN | 0 | 11 || 12 | 0 | ALT5 | GPIO. 1 | 1   | 18  |
 |  27 |   2 | GPIO. 2 |   IN | 0 | 13 || 14 |   |      | 0v      |     |     |
 |  22 |   3 | GPIO. 3 |   IN | 0 | 15 || 16 | 0 | IN   | GPIO. 4 | 4   | 23  |
 |     |     |    3.3v |      |   | 17 || 18 | 0 | IN   | GPIO. 5 | 5   | 24  |
 |  10 |  12 |    MOSI |   IN | 0 | 19 || 20 |   |      | 0v      |     |     |
 |   9 |  13 |    MISO |   IN | 0 | 21 || 22 | 0 | IN   | GPIO. 6 | 6   | 25  |
 |  11 |  14 |    SCLK |   IN | 0 | 23 || 24 | 1 | IN   | CE0     | 10  | 8   |
 |     |     |      0v |      |   | 25 || 26 | 1 | IN   | CE1     | 11  | 7   |
 |   0 |  30 |   SDA.0 |   IN | 1 | 27 || 28 | 1 | IN   | SCL.0   | 31  | 1   |
 |   5 |  21 | GPIO.21 |   IN | 1 | 29 || 30 |   |      | 0v      |     |     |
 |   6 |  22 | GPIO.22 |   IN | 1 | 31 || 32 | 0 | IN   | GPIO.26 | 26  | 12  |
 |  13 |  23 | GPIO.23 |   IN | 0 | 33 || 34 |   |      | 0v      |     |     |
 |  19 |  24 | GPIO.24 |   IN | 0 | 35 || 36 | 0 | IN   | GPIO.27 | 27  | 16  |
 |  26 |  25 | GPIO.25 |   IN | 0 | 37 || 38 | 0 | IN   | GPIO.28 | 28  | 20  |
 |     |     |      0v |      |   | 39 || 40 | 0 | IN   | GPIO.29 | 29  | 21  |
 +-----+-----+---------+------+---+----++----+---+------+---------+-----+-----+
 | BCM | wPi |   Name  | Mode | V | Physical | V | Mode | Name    | wPi | BCM |
 +-----+-----+---------+------+---+---Pi 3---+---+------+---------+-----+-----+
~> gpio mode 21 out
~> gpio write 21 1
~> gpio readall |grep GPIO.21
 |   5 |  21 | GPIO.21 |   IN | 1 | 29 || 30 |   |      | 0v      |     |     |
```

### I²C debugging

I²C needs to be enabled first:

```bash
echo "dtparam=i2c_arm=on" >> /boot/config.txt
echo "i2c-dev" >> /etc/modules
```

See also:

* [Raspberry Pi: I2C-Konfiguration und -Programmierung](http://www.netzmafia.de/skripten/hardware/RasPi/RasPi_I2C.html)
* [Mehr I/O-Ports bereitstellen mit dem PCF8574](http://www.raspberry-pi-geek.de/Magazin/2016/03/Mehr-I-O-Ports-bereitstellen-mit-dem-PCF8574)

`i2cdetect` (as root) can list devices on the bus:

```
~> i2cdetect -y 1
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          -- -- -- -- -- -- -- -- -- -- -- -- --
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
40: -- -- 42 -- -- -- -- -- -- -- -- -- -- -- -- --
[...]

~> i2cdetect -F 1
Functionalities implemented by /dev/i2c-1:
I2C                              yes
SMBus Quick Command              yes
SMBus Send Byte                  yes
SMBus Receive Byte               yes
[...]
```

To read/write values: `i2cget` and `i2cset`. In case of PCF8574, only `i2cget` is used because the IOs are read and written
with the same command (note that `0x20` is the address of the IC as shown in `i2cdetect`):

```
~> i2cget -y 1 0x20
0x02
~> i2cget -y 1 0x20 0x01
0x02
```

For MCP23008:
```
~> i2cget -y 1 0x20 0x09
0x12
~> i2cset -y 1 0x20 0x00 0xfe   # Configure pin 0 (bit 1) as output and all others (0xfe) as input
~> i2cset -y 1 0x20 0x09 0x01   # Set pin 0 (bit 1) to high
```

For MCP23017 in the standard configuration with `IOCON.BANK = 0`:

```
~> i2cset -y 1 0x21 0x00 0xf0   # Configure pins 0 to 3 as output (0) by writing 0b11110000
~> i2cset -y 1 0x21 0x12 0x0d   # Enable pins 0,2,3 by writing 0b00001101
```

[gpio-specs]: (http://www.mosaic-industries.com/embedded-systems/microcontroller-projects/raspberry-pi/gpio-pin-electrical-specifications)

## Audio

For higher quality audio, use the [HiFiBerry](https://www.hifiberry.com/).

Play sound at non-max with `omxplayer` which is generally quite efficient also for playing videos:

    omxplayer --vol -5000 file.mp3

Speech synthesis output; use omxplayer for better quality. See [RPi Text to Speech](https://elinux.org/RPi_Text_to_Speech_(Speech_Synthesis))

    echo "Hey there" | espeak -ven+f3 -k5 -s150 --stdin -w /tmp/test.wav; omxplayer /tmp/test.wav

Play sound with `mplayer` and change the volume with `amixer`:

    mplayer file.mp3
    amixer contents           # Get available commands
    amixer cset numid=1 50%   # Set the volume to 50 %

## Display

* [Get display settings right](https://www.opentechguides.com/how-to/article/raspberry-pi/28/raspi-display-setting.html)

Dump HDMI settings:

    tvservice -d tvdump
    edidparser tvdump

Enable at boot time: [hdmi_force_hotplug=1](http://raspberrypi.stackexchange.com/questions/2169/how-do-i-force-the-raspberry-pi-to-turn-on-hdmi)

    hdmi_force_hotplug=1 # Use HDMI mode even if no monitor is connected on startup
    hdmi_drive=2         # Use normal HDMI mode with sound

    # Example shell command to enable HDMI
    echo "Enabling HDMI always"
    perl -pi -e 's/#hdmi_force_hotplug=1/hdmi_force_hotplug=1/' /boot/config.txt

Enable HDMI from terminal: [Enable and disable the HDMI port on the Raspberry Pi](https://gist.github.com/AGWA/9874925).
This is useful e.g. if the resolution on a screen does not fit when plugging it after startup.
Also works via SSH.

    tvservice -s # Status
    tvservice -o # Off
    tvservice -p # On with preferred settings
    sudo fgconsole # Get current virtual terminal number
    sudo chvt 1 # Switch to tty1 first, otherwise screen often stays black.
    sudo chvt 7 # Set virtual terminal to 7 (like Shift+Ctrl+F7)

Just turn display on and off

    vcgencmd display_power 0
    vcgencmd display_power 1

To get available modes:

    tvservice -m DMT

Also interesting

    fbset -i

## UI and Kiosk mode

For Kiosk mode, see [Raspberry Pi Kiosk Screen Tutorial][kioskmode]

Start Chromium in full-screen mode without dialogs and toolbars.

    chromium-browser --start-maximized --kiosk --noerrdialogs --incognito --app=URL

Disable cursor except when moving it

    sudo apt-get install unclutter

Desktop wallpaper and no trash icon: Edit the file `.config/pcmanfm/LXDE-pi/desktop-items-0.conf`

    wallpaper=/home/pi/x.jpg
    show_trash=0

Restart X

    sudo /etc/init.d/lightdm stop
    sudo /etc/init.d/lightdm start
    # OR
    sudo systemctl [stop|start|restart] lightdm

[kioskmode]: https://www.danpurdy.co.uk/wp-content/cache/page_enhanced/www.danpurdy.co.uk/web-development/raspberry-pi-kiosk-screen-tutorial/_index.html


## Serial

When the serial console is enabled, some GPIO pins function as hardware serial port.
This serial terminal is available already at boot time.
Using the serial console, no network connection or screen is necessary.

See [Adafruit’s guide][ada-serial] on the serial terminal.

[ada-serial]: https://cdn-learn.adafruit.com/downloads/pdf/adafruits-raspberry-pi-lesson-5-using-a-console-cable.pdf


## Performance

Get current temperature ([more on vcgencmd](https://www.elinux.org/RPI_vcgencmd_usage))

    vcgencmd measure_temp

Run CPU benchmark

    sysbench --test-cpu --num-threads 4 --cpu-max-prime 30000 run

### MicroSD performance

The MicroSD card speed can be improved especially on some slow cards by [overclocking the SD reader][overclock].
Unfortunately, this seems to break wlan.

Current measurements (`dd`: bs=4096. `OC`: Overclocked. `X`: Desktop loaded. `www`: Auto-started Chromium is ready.)

```
dd raspbian   1st run   init 6   i6 after setup      OC        Card

24.5 MB/s¹    68 s      37 s     25 s X, 34 s www    -         SanDisk Ultra 32 GB MicroSDHC I
28.6 MB/s¹    51 s      21 s     20 s X, 25 s www    -         Samsung Evo 32 GB MicroSDHC I
37.5 MB/s¹    57 s      28 s     27 s X, 35 s www    -         Lexar 1000x 32 GB MicroSDHC II
32.1 MB/s¹    40 s      18 s     18 s X, 27 s www    -         Samsung Evo+ 32 GB MicroSDHC I
48.5 MB/s¹    38 s      19 s     22 s X, 26 s www    -         Samsung Pro 16 GB  MicroSDHC I
39.1 MB/s²    93 s      33 s     56 s X, 71 s www    37s 45s   Kingston UHS-I 16 GB MicroSDHC U3
14.6 MB/s²    38 s      21 s     23 s X, 33 s www    21s 27s   SanDisk Ultra 16 GB MicroSDHC A1
19.6 MB/s²    44 s      24 s     24 s X, 35 s www    25s 31s   Samsung Evo+ 32 GB MicroSDHC I
52.0 MB/s³    48 s      22 s                                   SanDisk Extreme 32 GB MicroSDHC A1

¹ Raspbian 2016-x
  Written in a Lexar card reader, which I suspect to break MicroSD cards, but writes much faster.
² Raspbian 2017-09-07 (4.6 GB)
  Written through normal SD adapter
³ Custom Raspbian (3.1 GB)
```

More performance measurements:
* [Model 3 B+](https://www.jeffgeerling.com/blog/2018/raspberry-pi-3-b-review-and-performance-comparison)
* [Micro SD cards](http://www.pidramble.com/wiki/benchmarks/microsd-cards)

[overclock]: http://www.jeffgeerling.com/blog/2016/how-overclock-microsd-card-reader-on-raspberry-pi-3

SanDisk Extreme A1 on a Raspi 3B

```
	Include fsync in write timing
	O_DIRECT feature enabled
	Auto Mode
	File size set to 102400 kB
	Record Size 4 kB
	Command line used: ./iozone -e -I -a -s 100M -r 4k -i 0 -i 1 -i 2 -f /tmp/iozone-testfile
	Output is in kBytes/sec
	Time Resolution = 0.000001 seconds.
	Processor cache size set to 1024 kBytes.
	Processor cache line size set to 32 bytes.
	File stride size set to 17 * record size.
                                                              random    random     bkwd    record    stride                                    
              kB  reclen    write  rewrite    read    reread    read     write     read   rewrite      read   fwrite frewrite    fread  freread
          102400       4     3044     3144     8570     8573     8067     4437                                                                

iozone test complete.
```

Same on 3B+

```
	Include fsync in write timing
	O_DIRECT feature enabled
	Auto Mode
	File size set to 102400 kB
	Record Size 4 kB
	Command line used: ./iozone -e -I -a -s 100M -r 4k -i 0 -i 1 -i 2 -f /tmp/iozone-testfile
	Output is in kBytes/sec
	Time Resolution = 0.000001 seconds.
	Processor cache size set to 1024 kBytes.
	Processor cache line size set to 32 bytes.
	File stride size set to 17 * record size.
                                                              random    random     bkwd    record    stride                                    
              kB  reclen    write  rewrite    read    reread    read     write     read   rewrite      read   fwrite frewrite    fread  freread
          102400       4     3123     3247     9124     9041     8468     4629                                                                

iozone test complete.
```


## Network

List blocked devices (WLAN/Bluetooth)

    rfkill list

Prefer WLAN over Ethernet: Add metric to `/etc/dhcpcd.conf`. 300 is default, higher number = less important.
See `man 5 interfaces`.

    interface eth0
    [...]
    metric 304

Connect to WLAN (same effect as via task bar) in `/etc/wpa_supplicant/wpa_supplicant.conf`: Add

    network={
        ssid="NETWORK_SSID"
        psk="NETWORK_PASSWORD"
        key_mgmt=WPA-PSK
    }

or the output of `wpa_password "YOUR_SSID" "YOUR_PSK"` if the password should be encrypted.
Then reconfigure wlan0 and check the settings.
More information in the [Raspi docs](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md).

    wpa_cli -i wlan0 reconfigure
    ifconfig wlan0
    ping -I wlan0 google.com # Use wlan0 for ping. See ifmetric settings above.

Enable SSH

    systemctl enable ssh
    systemctl start ssh

## Boot Overlays

Boot overlays e.g. allow to enable I²C, disable audio, etc.
Documentation, including default values, can be found in `/boot/overlays/README`.


## Temperature

The `vcgencmd` tool allows to measure temperature, CPU voltage, etc.

```bash
vcgencmd measure_temp
vcgencmd commands
vcgencmd display_power 0
```

See also: [vcgencmd usage](https://www.elinux.org/RPI_vcgencmd_usage)


## Build your own Raspbian

Use [pi-gen](https://github.com/RPi-Distro/pi-gen) and customise the stages. Drop stages 4 and 5 if Python
and Mathematica are not required; results in a < 500 MB large ISO (takes 40 minutes on a laptop with 10 Mb/s download).

The builder runs on a docker image, so all required tools are installed in a separate environment.
