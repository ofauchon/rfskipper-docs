


# Flashing with STLinkV2


* Connect Bluepill and STLink

Use 4 Dupont wires to connect the Bluepill to the STLink:

| BluePill      | STLink-V2   |
| ------------- |:-----------:|
| GND           | GND         |
| 3.3V          | 3.3V        |
| SWDIO         | SWDIO       |
| SWCLK         | SWCLK       |


* Flash the RFSkipper firmware 
```
$ st-flash --reset write rfskipper_latest.bin 0x08000000
st-flash 1.6.0
2020-05-15T21:17:25 INFO usb.c: -- exit_dfu_mode
2020-05-15T21:17:25 INFO common.c: Loading device parameters....
2020-05-15T21:17:25 INFO common.c: Device connected is: F1 Medium-density device, id 0x20036410
2020-05-15T21:17:25 INFO common.c: SRAM size: 0x5000 bytes (20 KiB), Flash: 0x10000 bytes (64 KiB) in pages of 1024 bytes
2020-05-15T21:17:25 INFO common.c: Attempting to write 26156 (0x662c) bytes to stm32 address: 134217728 (0x8000000)
Flash page at addr: 0x08006400 erased
2020-05-15T21:17:25 INFO common.c: Finished erasing 26 pages of 1024 (0x400) bytes
2020-05-15T21:17:25 INFO common.c: Starting Flash write for VL/F0/F3/F1_XL core id
2020-05-15T21:17:25 INFO flash_loader.c: Successfully loaded flash loader in sram
 26/26 pages written
2020-05-15T21:17:26 INFO common.c: Starting verification of write complete
2020-05-15T21:17:27 INFO common.c: Flash written and verified! jolly good!
``` 

Great! You are done, the bluepill is flashed with RFSkipper Firmware


* First tests

Unplug the STLink-V2 programmer and connect the bluepill to the computer. 

The usb device should be detected with the twu USB-Serial ports:

```
$ lsusb | grep RFSkipper
Bus 001 Device 028: ID 0000:6018 RFSkipper RFSkipper Gateway, (Firmware fce0621-dirty)

$ dmesg | tail -n 5
[113058.077280] usb 1-1: Product: RFSkipper Gateway, (Firmware fce0621-dirty)
[113058.077281] usb 1-1: Manufacturer: RFSkipper
[113058.077282] usb 1-1: SerialNumber: e3d0b8b8
[113058.080609] cdc_acm 1-1:1.0: ttyACM1: USB ACM device
[113058.081500] cdc_acm 1-1:1.2: ttyACM2: USB ACM device


```

Now you can connect the second ttyACM port (speed : 57600 baud),
and run your first "10;PING; command. 


```
picocom --imap lfcrlf --omap crcrlf -b 57600 -c /dev/ttyACM2

10;PING;
20;02;PONG;
```

Great, your RFSkipper is working !

Here are more commands to play with : 


| Command      | Description   |
| ------------ |:-----------:|
| 10;PING;     | Ping test         |
| 10;SETPARM;RSSI;-50;  | Change RSSI level        |
| 10;RFDEBUG=ON;     | Dump RF Packets  |
| 10;RFDUMP;   | Dump RFM69 Registers |
