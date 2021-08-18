# Control Autoterm Air 2D

## Serial connection

- Baud rate: __9600__
- Data bits: __8__
- Parity: __none__
- Stop bits: __1__

&nbsp;

## Message design

Every message consists of three parts. A header, a payload and a checksum. The header is always five bytes in length. The checksum are always two bytes at the end of a message. The length of the payload can vary and is described in byte two of the header.

&nbsp;

This is a message example:

|Byte|Hex|Decimal|Description|
|-|-|-|-|
|0|`AA`|`170`|preamble|
|1|`04`|`4`|3 = request, 4 = response|
|2|`13`|`19`|payload length|
|3|`00`|`0`|always "0"|
|4|`0F`|`15`|type: |
|5+|`...`|`...`|payload|
|-2 to -1|`D9 49`||checksum|

&nbsp;

## Control communication

These messages can be sent to the heater to get a response.

&nbsp;

### Get status

||Base|Payload|Checksum|Note|
|-|-|-|-|-|
|▶|`AA 03 00 00 0F`||`58 7C`||
|◀|`AA 04 13 00 0F`|`00 01 00 13 7F 00 86 01 24 00 00 00 00 00 00 00 00 00 64`|`D9 49`||

&nbsp;

### Get settings

||Base|Payload|Checksum|Note|
|-|-|-|-|-|
|▶|`AA 03 00 00 02`||`9D BD`||
|◀|`AA 04 06 00 02`|`01 00 04 10 00 08`|`69 6C`||