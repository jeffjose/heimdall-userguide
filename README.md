### Heimdall User Guide

![header](https://raw.githubusercontent.com/jeffjose/heimdall-userguide/master/header.png)


Heimdall is a cross-platform open-source tool suite used to flash firmware (aka ROMs) onto Samsung mobile devices.

Download - http://glassechidna.com.au/heimdall/

Github - https://github.com/Benjamin-Dobell/Heimdall

### How to use

While `heimdall` comes with a GUI frontend, I prefer to use the commandline. This guide provides instructions on how to use the commandline version of the tool

#### `heimdall`
```
%> heimdall version
v1.4.1
```

### Flash a modem

**Step 1 - Download the modem**


Modems typically come with `.bin` extension. If you get a `rar` or `tar`, unpack it first

**Step 2 - Boot into Download mode**


If you have `adb` connected, do `adb reboot download`. You could also long press power button, select Reboot > Download

**Step 3 - Make `heimdall` detect for good measure**


Once you're in the download mode, run `heimdall detect`

```
%> heimdall detect
Device detected
```

**Step 4 - Find the right partition**


Partition Information is in the PIT files. Download the PIT file using

Note: Entry #1 says **MDM**. That's what we want. Yours might say **MODEM** or **modem**. Make a note of that.

```
%> heimdall print-pit
Heimdall v1.4.1

Copyright (c) 2010-2014 Benjamin Dobell, Glass Echidna
http://www.glassechidna.com.au/

This software is provided free of charge. Copying and redistribution is
encouraged.

If you appreciate this software and you would like to support future
development please consider donating:
http://www.glassechidna.com.au/donate/

Initialising connection...
Detecting device...
Claiming interface...
Setting up interface...

Initialising protocol...
Protocol initialisation successful.

Beginning session...

Some devices may take up to 2 minutes to respond.
Please be patient!

Session begun.

Downloading device's PIT file...
PIT file download successful.

Entry Count: 33
Unknown 1: 1598902083
Unknown 2: 844251476
Unknown 3: 21325
Unknown 4: 14413
Unknown 5: 13881
Unknown 6: 48
Unknown 7: 0
Unknown 8: 0


--- Entry #0 ---
Binary Type: 0 (AP)
Device Type: 2 (MMC)
Identifier: 1
Attributes: 5 (Read/Write)
Update Attributes: 1 (FOTA)
Partition Block Size/Offset: 8192
Partition Block Count: 25544
File Offset (Obsolete): 0
File Size (Obsolete): 0
Partition Name: APNHLOS
Flash Filename: NON-HLOS.bin
FOTA Filename:


--- Entry #1 ---
Binary Type: 0 (AP)
Device Type: 2 (MMC)
Identifier: 2
Attributes: 5 (Read/Write)
Update Attributes: 1 (FOTA)
Partition Block Size/Offset: 33736
Partition Block Count: 105528
File Offset (Obsolete): 0
File Size (Obsolete): 0
Partition Name: MDM
Flash Filename: modem.bin
FOTA Filename:

[snip]

--- Entry #19 ---
Binary Type: 0 (AP)
Device Type: 2 (MMC)
Identifier: 20
Attributes: 5 (Read/Write)
Update Attributes: 1 (FOTA)
Partition Block Size/Offset: 10146376
Partition Block Count: 20480
File Offset (Obsolete): 0
File Size (Obsolete): 0
Partition Name: BOOT
Flash Filename: boot.img
FOTA Filename:


--- Entry #20 ---
Binary Type: 0 (AP)
Device Type: 2 (MMC)
Identifier: 21
Attributes: 5 (Read/Write)
Update Attributes: 1 (FOTA)
Partition Block Size/Offset: 10166856
Partition Block Count: 20480
File Offset (Obsolete): 0
File Size (Obsolete): 0
Partition Name: RECOVERY
Flash Filename: recovery.img
FOTA Filename:

```

**Step 5 - Flash the modem (Try 1)**


While flashing once is good enough for a lot of other things, I've found that I had to flash it twice in a specific manner to make the modem sticky.

`--no-reboot` is required so that you can prevent the device from booting up, and flash the modem again.

```
%> heimdall flash --MDM /path/to/modem.bin --no-reboot
heimdall v1.4.1

Copyright (c) 2010-2014 Benjamin Dobell, Glass Echidna
http://www.glassechidna.com.au/

This software is provided free of charge. Copying and redistribution is
encouraged.

If you appreciate this software and you would like to support future
development please consider donating:
http://www.glassechidna.com.au/donate/

Initialising connection...
Detecting device...
Claiming interface...
Setting up interface...

Initialising protocol...
Protocol initialisation successful.

Beginning session...

Some devices may take up to 2 minutes to respond.
Please be patient!

Session begun.

Downloading device's PIT file...
PIT file download successful.

Uploading MDM
100%
MDM upload successful

Ending session...

Releasing device interface...
```

**Step 6 - Remove Battery > Remove USB Connection > Press Vol Down + Power + Menu to boot into Download mode**


We're getting ready to flash again.


**Step 7 - Flash modem (Try 2)**


Same instructions as above
```
%> heimdall flash --MDM /path/to/modem.bin --no-reboot
```

**Step 8 - repeat Step #6, but boot normally**


Skip the vol down + power + menu bit


**Step 9 - Confirm the new baseband with `adb`**


```
%> adb shell getprop | grep baseband
[gsm.version.baseband]: [I9505ZHUFNB3]
```
