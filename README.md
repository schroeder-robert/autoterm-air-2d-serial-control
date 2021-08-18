# Control Autoterm Air 2D

## Serial connection

- Baud rate: __9600__
- Data bits: __8__
- Parity: __none__
- Stop bits: __1__

&nbsp;

## Message design

Every message consists of three parts. A header, a payload and a checksum. The header is always five bytes in length. The checksum consists of two bytes placed at the end of a message. The length of the payload can vary and is described in byte two of the header.

&nbsp;

This is a message example:

|Byte|Hex|Decimal|Description|
|-:|-:|-:|:-|
|0|`AA`|`170`|preamble|
|1|`04`|`4`|2 = diag, 3 = request, 4 = response|
|2|`13`|`19`|payload length|
|3|`00`|`0`|always "0"|
|4|`0F`|`15`|type: see message types|
|5+|`...`|`...`|payload|
|-2 to -1|`D9 49`||checksum|

&nbsp;

## Request/Response message types

These messages can be sent to the heater to get a response.

&nbsp;

### Turn heater on - `01`

||Header|Payload|Checksum|
|-|:-|:-|:-|
|▶|`AA 03 02 00 01`|`00 28`|`27 39`|
|◀|`AA 04 00 00 01`|`...`|`...`|

&nbsp;

### Get/set settings - `02`

Leave request payload empty to get settings, add bytes from paylod description to set settings.

||Header|Payload|Checksum|
|-|:-|:-|:-|
|▶|`AA 03 00 00 02`||`9D BD`|
|◀|`AA 04 06 00 02`|`01 00 04 10 00 08`|`69 6C`|

&nbsp;

__Request/Response payload description__

|Byte|Hex|Decimal|Description|
|-:|-:|-:|:-|
|0|`01`|`1`|use work time: 0 = yes, 1 = no|
|1|`00`|`0`|work time|
|2|`04`|`4`|temperature source: 1 = internal sensor, 2 = panel sensor (set temp message), 3 = external sensor, 4 = no automatic temperature control|
|3|`10`|`0`|temperatur|
|4|`00`|`0`|wait mode: 1 = on, 2 = off|
|5|`08`|`8`|level: 0-9|

&nbsp;

### Turn off - `03`

||Header|Payload|Checksum|
|-|:-|:-|:-|
|▶|`AA 03 00 00 03`||`5D 7C`|
|◀|`AA 04 00 00 03`|`...`|`...`|

&nbsp;

### Get version - `06`

||Header|Payload|Checksum|
|-|:-|:-|:-|
|▶|`AA 03 00 00 06`||`5E BC`|
|◀|`AA 04 00 00 06`|`...`|`...`|

&nbsp;

### Diagnostic control - `07`

||Header|Payload|Checksum|
|-|:-|:-|:-|
|▶|`AA 03 01 00 07`|`01`|`1D 9E`|
|◀|`AA 04 00 00 07`|`...`|`...`|

&nbsp;

### Set fan speed - `08`

&nbsp;

### Report - `0B`

&nbsp;

### Unlock - `0D`

&nbsp;

### Get status - `0F`

||Header|Payload|Checksum|
|-|:-|:-|:-|
|▶|`AA 03 00 00 0F`||`58 7C`|
|◀|`AA 04 13 00 0F`|`00 01 00 13 7F 00 86 01 24 00 00 00 00 00 00 00 00 00 64`|`D9 49`|

&nbsp;

### Set temperature - `11`

||Header|Payload|Checksum|
|-|:-|:-|:-|
|▶|`AA 03 01 00 11`|`14`|`B2 51`|
|◀|`AA 03 00 00 11`|`...`|`...`|

&nbsp;

### Fuel pump - `13`

&nbsp;

### Start - `1C`

&nbsp;

### Unknown - `1C`

&nbsp;

### Turn only fan on - `23`

||Header|Payload|Checksum|
|-|:-|:-|:-|
|▶|`AA 03 04 00 23`|`FF FF 08 FF`|`E1 0B`|
|◀|`AA 04 00 00 23`|`...`|`...`|

&nbsp;

## Diagnostic message types

### connect - `00`

&nbsp;

### heater - `01`