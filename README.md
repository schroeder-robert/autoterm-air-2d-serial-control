# Control Autoterm Air 2D

This documentation is a result of a reverse engineering process and is not complete nor immaculate. Feel free to add your or correct these findings.

&nbsp;

## Serial connection

You can use a simple USB to serial adapter to connect to the heater. Pinout of the connector comming soon.

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
|▶|`AA 03 02 00 01`|`01 00 04 10 00 08`|`B3 EE`|
|◀|`AA 04 06 00 01`|`01 00 04 10 00 08`|`69 5F`|

&nbsp;

__Request/Response payload description__

See "Get/set settings"

&nbsp;

---

&nbsp;

### Get/set settings - `02`

Leave request payload empty to get settings, add bytes from payload description to set settings.

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

---

&nbsp;

### Turn heater/fan off - `03`

||Header|Payload|Checksum|
|-|:-|:-|:-|
|▶|`AA 03 00 00 03`||`5D 7C`|
|◀|`AA 04 00 00 03`||`29 7D`|

&nbsp;

---

&nbsp;

### Get version - `06`

||Header|Payload|Checksum|
|-|:-|:-|:-|
|▶|`AA 03 00 00 06`||`5E BC`|
|◀|`AA 04 05 00 06`|`02 01 03 04 03`|`C1 6E`|

&nbsp;

__Response payload description__

|Byte|Hex|Decimal|Description|
|-:|-:|-:|:-|
|0|`02`|`2`|version: __2__.1.3.4|
|1|`01`|`1`|version: 2.__1__.3.4|
|2|`03`|`3`|version: 2.1.__3__.4|
|3|`04`|`4`|version: 2.1.3.__4__|
|4|`03`|`3`|blackbox version|

&nbsp;

---

&nbsp;

### Diagnostic control - `07`

||Header|Payload|Checksum|
|-|:-|:-|:-|
|▶|`AA 03 01 00 07`|`01`|`1D 9E`|
|◀|`AA 04 00 00 07`|`...`|`...`|

&nbsp;

---

&nbsp;

### Set fan speed - `08`

&nbsp;

---

&nbsp;

### Report - `0B`

&nbsp;

---

&nbsp;

### Unlock - `0D`

&nbsp;

---

&nbsp;

### Get status - `0F`

||Header|Payload|Checksum|
|-|:-|:-|:-|
|▶|`AA 03 00 00 0F`||`58 7C`|
|◀|`AA 04 13 00 0F`|`00 01 00 13 7F 00 86 01 24 00 00 00 00 00 00 00 00 00 64`|`D9 49`|

&nbsp;

__Response payload description__

|Byte|Hex|Decimal|Description|
|-:|-:|-:|:-|
|0|`00`|`0`|status code: __0__.1|
|1|`01`|`1`|status code: 0.__1__|
|2|`00`|`0`|?|
|3|`13`|`19`|internal temp sensor (temp > 127 ? temp - 255 : temp)|
|4|`7F`|`127`|external temp sensor (temp > 127 ? temp - 255 : temp)|
|5|`00`|`0`|?|
|6|`86`|`134`|voltage (volt / 10)|
|7|`01`|`1`|?|
|8|`24`|`36`|heater temp sensor (temp - 15)|
|9|`00`|`0`|?|
|10|`00`|`0`|?|
|11|`00`|`0`|fan rpm set (rpm * 60)|
|12|`00`|`0`|fan rpm actual (rpm * 60)|
|13|`00`|`0`|?|
|14|`00`|`0`|frequency fuel pump (freq / 100)|
|15|`00`|`0`|?|
|16|`00`|`0`|?|
|17|`00`|`0`|?|
|18|`64`|`100`|?|

&nbsp;

---

&nbsp;

### Set temperature - `11`

||Header|Payload|Checksum|
|-|:-|:-|:-|
|▶|`AA 03 01 00 11`|`14`|`B2 51`|
|◀|`AA 04 01 00 11`|`14`|`72 E4`|

&nbsp;

__Request/Response payload description__

|Byte|Hex|Decimal|Description|
|-:|-:|-:|:-|
|0|`14`|`20`|panel temp sensor|

&nbsp;

---

&nbsp;

### Fuel pump - `13`

&nbsp;

---

&nbsp;

### Start - `1C`

&nbsp;

---

&nbsp;

### Unknown - `1C`

&nbsp;

---

&nbsp;

### Turn only fan on - `23`

||Header|Payload|Checksum|
|-|:-|:-|:-|
|▶|`AA 03 04 00 23`|`FF FF 08 FF`|`E1 0B`|
|◀|`AA 04 04 00 23`|`FF FF 08 43`|`B6 4B`|

&nbsp;

__Request/Response payload description__

|Byte|Hex|Decimal|Description|
|-:|-:|-:|:-|
|0|`FF`|`255`|?|
|1|`FF`|`255`|?|
|2|`08`|`8`|level: 0-9|
|3|`FF`|`255`|?|

&nbsp;

&nbsp;

## Diagnostic message types

### connect - `00`

&nbsp;

### heater - `01`