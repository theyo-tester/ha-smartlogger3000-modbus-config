# Home Assistant Modbus Configuration fo the SmartLogger 3000

My selection of modbus registers compatible for the Huawei Smartlogger 3000

Make sure that you enable the ModbusTCP option in our SmartLogger and that you set the IP of the smartLogger to a fixed address.

Also change the addresing in the ModbusTCP configuration to logic addressing.

My setup contains three inverters at specific slave (logic) addresses and a smart meter. Also only the 10kW inverter has a battery connected to it.

So first find out how many inverters you have, and what address each of them, including the meter, have.

Further the Referece Doc says regarding the smartlogger itself: 
"In the Modbus-TCP communications protocol, the logic device ID is fixed to 0."


Useful notes: 
- I also introduced a sensor teplate to calculate the actual Load Power because this is not directly provided by the SmartLogger
- At the end of the file I have grouped the entities in different groups, you can get rid of them if you dont need them
- You can include this file into your Homeassistant Config by adding this line to your configuration.yaml

````
homeassistant:
    packages:
        pack_modbus: !include modbus.yaml
````
