# Introduction #
CHAdeMO is a standard to deliver high voltage high current DC directly into an electric vehicle battery pack, to facilitate rapid recharging. For the current Nissan Leaf, this is about 30 minutes from near empty to 80%, giving 60-80 miles of range. The observed power is 50 kilowatts to around 50% state of charge, and the power [tapering off](Recharging_Speed.md) at higher levels. This is the development of open source CHAdeMO compliant chargers by reverse engineering the spec in a legal manner.

The standard can be broken down into the two methods used to control the process, analog and digital. Both methods of signaling are used to cross reference each step of the process. This is done to prevent malfunctions in ether system from creating an unsafe condition.

While this may seem as double the workload to reverse engineer, most of the analog side signaling is posted in the public domain on the CHAdeMO [website.](http://chademo.com/05_protocol.html)

## CHAdeMO Charge Flow Chart ##
| **Charger** | **Action** | **Vehicle** |
|:------------|:-----------|:------------|
|Send start-of-charging signal | → Charger pulls Pin 2 up to 12v | Recognize start-of-charging |
| Recieve battery parameters, do a compatibility check | ← Send over [CAN link](CAN_Control.md) | Transmit battery parameters |
|Transmit charger parameters | → Send over CAN Bus | Receive charger parameters, do compatibility check |
|Recognize start Permission Signal | ← Car grounds pin 4 | Send Start Permission Signal |
| Connector lock and perform insulation test, send charging ready signal |→ Charger grounds pin 10 | Recognize charging ready signal, main contactor ON|
|Receives current requirement, adjust to within 2% output | ← Send current requirement every 100ms over CAN Bus | BMS monitors battery SOC, Temp, and input current, calculates current requirements |
| Charging reaches end SOC set in charger | ← OR →     | Stopped by user |
|Receives zero current,outputs zero current	| ← Send current requirement over CAN Bus | Send zero current |
|Recognize charging stop | ← Car disconnects pin 4 from ground | Confirm zero current, contactor off, send charge stop signal |
|Terminate Charging Process | → Charger disconnects Pins 2 & 10, unlocks connector | User Removes CHAdeMO connector from Vehicle |

## Electrical Interface ##
From the CHAdeMO website:

![http://chademo.com/images/01/interface.jpg](http://chademo.com/images/01/interface.jpg)

## Mechanical Interface ##
The plug is most often made by Yazaki. They have some mechanical information in PDF form on their [website.](http://charge.yazaki-group.com/english/download/index.html)