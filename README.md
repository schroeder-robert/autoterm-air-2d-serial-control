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

## Request/Response Message types

These messages can be sent to the heater to get a response.

&nbsp;

### Turn heater on - `01`

&nbsp;

### Get settings - `02`

||Header|Payload|Checksum|
|-|:-|:-|:-|
|▶|`AA 03 00 00 02`||`9D BD`|
|◀|`AA 04 06 00 02`|`01 00 04 10 00 08`|`69 6C`|

&nbsp;

### Turn off - `03`

&nbsp;

### Get version - `06`

&nbsp;

### diag control - `07`

&nbsp;

### Set fan speed - `08`

&nbsp;

### report - `0B`

&nbsp;

### unlock - `0D`

&nbsp;

### Get status - `0F`

||Header|Payload|Checksum|
|-|:-|:-|:-|
|▶|`AA 03 00 00 0F`||`58 7C`|
|◀|`AA 04 13 00 0F`|`00 01 00 13 7F 00 86 01 24 00 00 00 00 00 00 00 00 00 64`|`D9 49`|

&nbsp;

### Set temperature - `11`

&nbsp;

### fuel pump - `13`

&nbsp;

### start - `1C`

&nbsp;

### misc 3 - `1C`

&nbsp;

### Turn only fan on - `23`

&nbsp;

## Diagnostic message types

### connect - `00`

&nbsp;

### heater - `01`