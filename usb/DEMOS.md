# Demos

Snippets for some of the demos.


## Part 1

### Sniffing USB with usbmon

Based on [kernel.org/doc/Documentation/usb/usbmon.txt](https://www.kernel.org/doc/Documentation/usb/usbmon.txt).

``` bash
mount -t debugfs none /sys/kernel/debug/
modprobe usbmon
cat /sys/kernel/debug/usb/usbmon/0u
cat /dev/usbmon0 | xxd
```

### Sniffing USB with wireshark

Follow [How to install Wireshak on Linux and capture USB traffic?](https://stackoverflow.com/questions/31054437/how-to-install-wireshak-on-linux-and-capture-usb-traffic).

### Sending USB control messages to a hub with USB PPPS support

``` bash
git clone https://github.com/mvp/uhubctl
sudo apt-get install libusb-1.0-0-dev
make
```

``` bash
$ cat /etc/udev/rules.d/98-hub.rules 
SUBSYSTEM=="usb", ATTR{idVendor}=="05e3", ATTR{idProduct}=="0608", MODE="0664", GROUP="plugdev"
$ sudo udevadm control --reload-rules
; replug hub
```

### Sending USB control messages to a Logitech web camera

Based on [Turning off the blue status LED on the logitech C920 usb camera?](https://raspberrypi.stackexchange.com/questions/43118/turning-off-the-blue-status-led-on-the-logitech-c920-usb-camera).

``` bash
sudo apt-get install uvcdynctrl cheese
cheese &
sudo uvcdynctrl -i /usr/share/uvcdynctrl/data/046d/logitech.xml
sudo uvcdynctrl -s 'LED1 Mode' 0
```

```
# Wireshark filter to see Control transfers
usb.transfer_type == 2
```


## Part 4

### Rubber Ducky

``` bash
wget https://github.com/hak5darren/USB-Rubber-Ducky/raw/master/duckencoder.jar
java -jar duckencoder.jar -i payload.txt -o inject.bin
# copy inject.bin to SD card
```

```
DELAY 1000
CTRL-ALT t
DELAY 1000
STRING echo DC7499
ENTER
```

### Bash Bunny

https://github.com/hak5/bashbunny-payloads/tree/master/payloads/library/recon/LinuxInfoGrabber

```
...
#RUN UNITY xterm
Q CTRL-ALT t
...
Q STRING export bunnydir=/media/\$USER/BashBunny
Q ENTER
...
Q STRING sync
Q ENTER
Q STRING umount \$bunnydir
Q ENTER
Q STRING exit
Q ENTER
```

https://github.com/hak5/bashbunny-payloads/tree/master/payloads/library/phishing/Captiveportal

### Teensy 3.2

Based on [Getting started with Teensy](https://spuder.wordpress.com/2010/10/21/getting-started-with-teensy-usb-rubber-ducky/).

1. Download and install [Arduino IDE](https://www.arduino.cc/en/Main/Software).
2. Download and install [Teensyduino](https://www.pjrc.com/teensy/td_download.html).
3. Run Arduino IDE.
4. Select Tools -> Board -> Teensy 3.2 / 3.1.
5. Select Tools -> USB Type -> Keyboard.
6. Select File -> Examples -> Teensy ->  USB_Keyboard -> Simple.
7. Upload and run.

Note: LED pin is pin #13 on Teensy 3.2.

### ATtiny85 board

Based on [1$ Rubber Ducky – Hack any PC within seconds MR.Robot style using Attiny85](http://www.khromozome.com/rubber-ducky-hack-pc-within-seconds-mr-robot-style-attiny85/).

1. Download and install [Arduino IDE](https://www.arduino.cc/en/Main/Software).
2. Follow [Connecting and Programming Your Digispark](http://digistump.com/wiki/digispark/tutorials/connecting).
3. Don't forget to add the [udev rule](https://github.com/micronucleus/micronucleus/wiki/Ubuntu-Linux).
4. Select Tools -> Programmer -> USBtinyISP.
5. Select Tools -> Board -> Digispark (default).
6. Select File -> Examples -> DigisparkKeyboard -> Keyboard.
7. Upload (plug in the device after you press the Upload button in Arduino IDE).
8. Run.

### CJMCU BadUSB

Based on [bad_ducky/wiki/Getting-Started](https://github.com/mharjac/bad_ducky/wiki/Getting-Started).

Format SD card as FAT32 and put `payload.txt` on it:

```
DELAY 1000
CTRL ALT t
DELAY 1000
STRING echo DC7499
ENTER
```

1. Download and install [Arduino IDE](https://www.arduino.cc/en/Main/Software).
2. Clone [bad_ducky](https://github.com/mharjac/bad_ducky) and open `bad_ducky.ino` in Arduino IDE.
3. Select Tools -> Board -> Arduino Leonardo.
4. Select Tools -> Port - > ttyACM0 (choose the right one).
5. Select Tools -> Programmer -> AVRISP mkII.
6. Upload.
7. Select Tools -> Serial Monitor.
8. Input mode: `a`.
9. Input language: `en`.
10. Input payload: `PAYLOAD.TXT`.
11. Replug the board.

### Cactus WHID

Based on [github.com/whid-injector/WHID](https://github.com/whid-injector/WHID).

1. Plug in Cactus WHID.
2. Connect to WiFi network `Exploit` with password `DotAgency`.
3. Go to http://192.168.1.1/duckuino.
4. Enter payload, convert to ESPloit format and copy.
5. Go to http://192.168.1.1/livepayload.
6. Paste and run payload.


## Part 5

### Emulating USB keyboard with Facedancer21

To flash: https://github.com/travisgoodspeed/goodfet#firmware

``` bash
git clone https://github.com/ktemkin/Facedancer.git
# Edit Facedancer/USBKeyboard.py and remove "import greatfet"
export BACKEND=goodfet
./facedancer-keyboard.py
```

``` bash
git clone https://github.com/travisgoodspeed/goodfet.git
git clone https://github.com/xairy/facedancer-utils.git
cp ./facedancer-utils/*.py ./goodfet/client/
cd ./goodfet/
```

### USB reconnaissance with Facedancer21

``` bash
git clone https://github.com/nccgroup/umap2.git
cd ./umap2/
pip2 install --user .
```

Device driver scanning:
``` bash
umap2scan -P fd:/dev/ttyUSB0
```

OS fingerprinting:
``` bash
git clone https://github.com/nccgroup/umap.git
cd ./umap/
python3 umap.py -P /dev/ttyUSB0 -O
```


## Part 6

### Raspberry Pi Zero

#### Setup

``` bash
echo "dtoverlay=dwc2" | sudo tee -a /boot/config.txt
echo "dwc2" | sudo tee -a /etc/modules
sync
reboot
```

#### Emulating mass storage drive through legacy gadget module

Based on [Raspberry Pi Zero OTG Mode](https://gist.github.com/gbaman/50b6cca61dd1c3f88f41).

``` bash
dd if=/dev/zero of=image.bin bs=512 count=2880
sudo mkdosfs ./image.bin
modprobe g_mass_storage file=./image.bin stall=0
modprobe -r g_mass_storage
```

#### Emulating keyboard with ConfigFS + FunctionFS

Based on [RaspberryPiZero_HID_MultiTool](https://github.com/darrylburke/RaspberryPiZero_HID_MultiTool/blob/master/gadget/hid/mkdevice.sh) and [Linux USB HID gadget driver](https://www.kernel.org/doc/Documentation/usb/gadget_hid.txt).

``` bash
./start.sh
ls /dev/hidg0
gcc hid_gadget_test.c -o hid
./hid /dev/hidg0 keyboard
./stop.sh
```

`start.sh`:
``` bash
#!/bin/bash

set -eux

modprobe libcomposite

mkdir -p /sys/kernel/config/usb_gadget/my_gadget
cd /sys/kernel/config/usb_gadget/my_gadget

echo 0x1d6b > idVendor # Linux Foundation
echo 0x0104 > idProduct # Multifunction Composite Gadget
echo 0x0100 > bcdDevice # v1.0.0
echo 0x0200 > bcdUSB # USB2
echo 0xEF > bDeviceClass
echo 0x02 > bDeviceSubClass
echo 0x01 > bDeviceProtocol

mkdir -p strings/0x409
echo "fedcba9876543210" > strings/0x409/serialnumber
echo "wtfwasthat" > strings/0x409/manufacturer
echo "Linux USB Device" > strings/0x409/product
mkdir -p configs/c.1/strings/0x409

echo "Config 1: ECM network" > configs/c.1/strings/0x409/configuration
echo 250 > configs/c.1/MaxPower

mkdir -p functions/hid.usb0
echo 1 > functions/hid.usb0/protocol
echo 1 > functions/hid.usb0/subclass
echo 8 > functions/hid.usb0/report_length
echo -ne "\\x05\\x01\\x09\\x06\\xA1\\x01\\x05\\x07\\x19\\xE0\\x29\\xE7\\x15\
\\x00\\x25\\x01\\x75\\x01\\x95\\x08\\x81\\x02\\x95\\x01\\x75\\x08\\x81\\x03\
\\x95\\x05\\x75\\x01\\x05\\x08\\x19\\x01\\x29\\x05\\x91\\x02\\x95\\x01\\x75\
\\x03\\x91\\x03\\x95\\x06\\x75\\x08\\x15\\x00\\x25\\x65\\x05\\x07\\x19\\x00\
\\x29\\x65\\x81\\x00\\xC0" > functions/hid.usb0/report_desc
ln -s functions/hid.usb0 configs/c.1/

ls /sys/class/udc > UDC
```

`stop.sh`:
``` bash
#!/bin/bash

set -eux

cd /sys/kernel/config/usb_gadget/my_gadget

echo "" > UDC

rmdir configs/c.1/strings/0x409/
rmdir strings/0x409/
rm configs/c.1/hid.usb0
rmdir configs/c.1/
rmdir functions/hid.usb0/
cd ..
rmdir my_gadget/

modprobe -r usb_f_hid
modprobe -r libcomposite
```

#### Emulating simple device through GadgetFS

Based on [Create your own USB gadget with GadgetFS](http://blog.soutade.fr/post/2016/07/create-your-own-usb-gadget-with-gadgetfs.html).

``` bash
mkdir /dev/gadget
mount -t gadgetfs gadgetfs /dev/gadget
# get source from the article linked above
gcc -g -o usb usb.c usbstring.c -lpthread
./usb
umount /dev/gadget
modprobe -r gadgetfs
```


## Part 7

### Fuzzing USB with Facedancer21

``` bash
cd Sandbox/
umap2stages -P fd:/dev/ttyUSB0 -C keyboard -s keyboard.stages
umap2kitty -s keyboard.stages &
umap2fuzz -P fd:/dev/ttyUSB0 -C keyboard
```


## Part 8

### USBProxy on BeagleBone Black

Get a shell on the BBB following [this](http://blog.tonywall.com/2013/11/beaglebone-black-serial-debug-connection/).

#### Preimaged

Get the image from [USBProxy releases](https://github.com/dominicgs/USBProxy/releases/) and flash onto an SD card.

Boot BBB, build USBProxy, run `sudo ./usb-mitm -l`.

#### Rebuild image

https://beagleboard.org/latest-images

http://gimx.fr/wiki/index.php?title=Bbb_sniffer

https://github.com/dominicgs/USBProxy/tree/master/doc

https://github.com/dominicgs/USBProxy/issues/50
