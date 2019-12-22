# USB

Materials related to the "Introduction to USB hacking" talk. For the most part covers only USB 2 (no USB 3+ or USB Type C) on Linux. Presented at PHDays 2018/2019 in Moscow and Chaos Constructions 2018 in Saint Petersburg.

Slides:
[PHDays 2018](https://docs.google.com/presentation/d/1S1yNeehSXOvtC_QmtqETSF3frlMLgka6v3XQVupJXTQ/edit?usp=sharing),
[Chaos Constructions 2018](https://docs.google.com/presentation/d/1C0Mfh9kcHQvqaLd6hbx6ilM_blYwg05YVW5rRS6Z8yQ/edit?usp=sharing),
[PHDays 2019](https://docs.google.com/presentation/d/1BP1wNSa0MvMdlZuRURw9VsWC198IgrtDFujgI2-2Fto/edit?usp=sharing).

Demos: [here](https://github.com/xairy/hardware-village/blob/master/usb/DEMOS.md).

Links: [here](https://github.com/xairy/hardware-village/blob/master/usb/LINKS.md).


## Hardware

[Rubber Ducky](https://hakshop.com/products/usb-rubber-ducky-deluxe) (45$)

[Bash Bunny](https://hakshop.com/products/bash-bunny) (100$)

[LAN Turtle](https://hakshop.com/products/lan-turtle) (55$)

[USB Armory](https://inversepath.com/usbarmory) (150$)

[USB Kill](https://usbkill.com/) (90$)

[Facedancer21](http://goodfet.sourceforge.net/hardware/facedancer21/) (85$)

[NXP LPC4330-Xplorer Board](https://www.nxp.com/support/developer-resources/nxp-designs/lpc4330-xplorer-board:OM13027) (60$)

[Teensy 3.2](https://www.pjrc.com/store/teensy32.html) (20$)

[ATtiny85 board](https://www.aliexpress.com/item/Free-shipping-1pcs-Digispark-kickstarter-development-board-ATTINY85-module-for-Arduino-usb/32697283942.html) (1.3$)

[CJMCU BadUSB](https://www.aliexpress.com/item/CJMCU-virtual-Keyboard-Badusb-USB-TTF-memory-Keyboard-ATMEGA32U4-module/32817551271.html) (10$)

[Cactus WHID](https://www.aliexpress.com/item/Cactus-Micro-compatible-board-plus-WIFI-chip-esp8266-for-atmega32u4/32318391529.html) (16$)

[Cactus Micro Rev2](https://www.aliexpress.com/item/Cactus-Micro-Rev2-Pro-Micro-atmega32u4-WIFI-ESP8266-module-ESP-11-ESP-03/32804236925.html) (35$)

[Raspberry Pi Zero](https://www.raspberrypi.org/products/raspberry-pi-zero/) (5$)

[Raspberry Pi Zero W](https://www.raspberrypi.org/products/raspberry-pi-zero-w/) (10$)

[BeagleBone Black](https://beagleboard.org/black) (70$)

[Nexus 7 2013 (Wi-Fi) tablet](https://en.wikipedia.org/wiki/Nexus_7_(2013)) (150$)

[EC3380-AB](http://www.bplus.com.tw/Adapter/EC3380-AB.html) (180$)

[AirDrive Keylogger Max](http://www.keelog.com/hardware-keylogger/) (100$)

[OpenVizsla](http://openvizsla.org/) (140$)


## Agenda

This is the ultimate plan, variations are possible.

### Part 1: USB 101

Slides:
* Follow [USB 101](http://www.cypress.com/file/134171/download)

#### Demos

1. Looking at syslog (`dmesg`) when a new USB device is connected.
2. Checking connected devices and their descriptors with `lsusb`.
3. Sniffing USB (LS/FS) packets with a logic analyzer.
4. Sniffing USB with usbmon (manually and with wireshark).
5. Sending USB control messages to a hub with USB PPPS support.
6. Sending USB control messages to a Logitech web camera.

### Part 2: USB attack surface

Slides:
* Attack surface (physical, firmware, BadUSB, remote)
* Original BadUSB
* USBKill

### Part 3: Consumer ready BadUSB

Slides:
* BadUSB: consumer ready vs self designed
* BadUSB: microcontroller based vs Linux based

#### Demos

1. Rubber Ducky.
2. Bash Bunny.
3. Lan Turtle.

### Part 4: Microcontroller based BadUSB

#### Demos

1. Teensy 3.2.
2. ATtiny55 board.
3. CJMCU BadUSB.
4. Cactus WHID.

### Part 5 Facedancer

Slides:
* Facedacer software overview
* Facedancer21/GreatFET ONE/NXP LPC4330 Xplorer overview.

#### Demos

1. Emulating USB keyboard with Facedancer.
2. USB reconnaissance with Facedancer.
3. Fuzzing USB with Facedancer.

### Part 6 Linux gadget subsystem

Slides:
* Linux USB stack
* udev rules
* USB sysfs
* usbfs
* Linux USB gadget subsystem
* Legacy gadget interface
* ConfigFS + FunctionFS
* GadgetFS
* Kernel gadget interface

#### Demos

1. Emulating mass storage drive through legacy gadget interface on Raspberry Pi Zero.
2. Emulating keyboard with ConfigFS + FunctionFS on Raspberry Pi Zero.
3. Emulating keyboard through GadgetFS on Raspberry Pi Zero.
4. Emulating keyboard through the kernel gadget interface on Raspberry Pi Zero.
5. Emulating keyboard with an Android device.

### Part 7: USB fuzzing

Slides:
* Fuzzing, hardware vs virtual
* vUSBf, QEMU and usbredir
* syzkaller, Linux gadget subsystem and dummy_hcd

#### Demos

0. Fuzzing with Facedancer already demoed in part 6.
1. Fuzzing USB with vUSBf.
2. Fuzzing USB with syzkaller.
3. Crashing a Linux machine via a bug in a USB driver.
4. Crashing a Windows machine via a bug in a USB driver.

### Part 8: USB sniffing

Slides:
* Hardware vs software sniffers
* "Low-level" vs "high-level" sniffers
* Beagle analyzers
* OpenVizsla
* Facedancer and Linux gadget subsystem based sniffers

#### Demos

0. Sniffing with usbmon already demoed in part 1.
1. Sniffing with a logic analyzer already demoed in part 1.
2. Sniffing USB with USBProxy on BeagleBone Black.
3. Sniffing USB with USBProxy 'Nouveau' with Facedancer.
4. USB MitM with two Facedacer boards.
5. Sniffing USB with OpenVizsla.
