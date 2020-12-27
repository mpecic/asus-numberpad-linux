Asus ZenBook Numberpad toggle for Linux
=======================================

Enable/disable touchpad numeric backlight on selected Asus ZenBook models.

Touchpad I2C HID device on ZenBook model UM425IA is `ASUE140A`:

```
% ls -l /sys/devices/platform/AMDI0010:03/i2c-0/i2c-ASUE140A:00/
total 0
drwxr-xr-x 5 root root    0 Dec 27 07:38 0018:04F3:3134.0002
lrwxrwxrwx 1 root root    0 Dec 27 07:38 driver -> ../../../../../bus/i2c/drivers/i2c_hid
lrwxrwxrwx 1 root root    0 Dec 27 08:10 firmware_node -> ../../../../LNXSYSTM:00/LNXSYBUS:00/AMDI0010:03/ASUE140A:00
-r--r--r-- 1 root root 4096 Dec 27 08:10 modalias
-r--r--r-- 1 root root 4096 Dec 27 07:38 name
drwxr-xr-x 2 root root    0 Dec 27 08:10 power
lrwxrwxrwx 1 root root    0 Dec 27 07:38 subsystem -> ../../../../../bus/i2c
lrwxrwxrwx 1 root root    0 Dec 27 08:10 supplier:regulator.0 -> ../../../../virtual/devlink/regulator.0--i2c-ASUE140A:00
-rw-r--r-- 1 root root 4096 Dec 27 07:38 uevent
```
On Windows, I inserted [I2C probe](https://github.com/bentiss/SimplePeripheralBusProbe) before Asus driver and got raw I2C commands. This is enable command:
```
05 00 3d 03 06 00 07 00 0d 14 03 01 ad
```
And disable command:
```
05 00 3d 03 06 00 07 00 0d 14 03 00 ad
```
On Linux, it's possible to send such commands using hidraw. For testing I just modified the [hid-example.c](https://github.com/torvalds/linux/blob/master/samples/hidraw/hid-example.c) from linux kernel source, compiled and ran as root:
```
gcc hid-asus.c -o numberpad
sudo ./numberpad 1 /dev/hidraw1
```
First argument is 0 (disable) or 1 (enable), second is hidraw device.

To enable I2C HID debug log, I added the following to kernel commandline:
```
i2c-hid.debug=1 hid.debug=1
```
