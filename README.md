# FibaroOpenHAB

Using Fibaro Sensors with OpenHAB2 to display simple information using *BasicUI* sitemap and setting up persistence using *mySQL*.
Hardware used - 
- Fibaro Sensors 
    - Fibaro (Motion) MultiSensor  - *FGMS-001*
    - Fibaro Door/Window Sensor - *FGK-101-107*
- Aeotec Z-Wave Stick Gen5

## Z-Wave Binding
Install ZWave binding using *PaperUI*.

## Aeotec Z-Wave Stick Gen5 Instructions
For inclusion/exclusion of sensors.
[Aeotec User Manual](https://aeotec.freshdesk.com/support/solutions/articles/6000056439-z-stick-gen-5-user-manual-)

## HabMin
HabMin used for adding and managing devices (installed using *PaperUI*). 
1. Add *Serial Controller* automatically discovered once Aeotec Stick plugged in.
    - Enter Port - discovered by typing  **$ ls /dev/tty***
2. Add Motion Sensor discovered by triple clicking B button (waking device up).
    - *Note: Sensor must be already included in the Z-Wave Stick, this is to add the sensor to openHAB.*
3. Device Node and Channels UID can be discovered once device is added. For MultiSensor, following can be used for motion in .items file - 
    > Switch motionSensor         "Motion [%s]" { channel="zwave:device:15b4d86c1b1:node4:alarm_motion" }
Frame label="Movement" {
        Text item=motionSensor
    }
[More info here](https://community.openhab.org/t/solved-fibaro-fgms-001-cannot-see-alarm-off-on-in-gui-paperui/25685/8)

**Note: In current binding sensor_binary is not functioning properly. As far as I can tell it gets triggered only when the device is woken up manually (by tripple clicking and b button) and stays on till next wake up time.**

4. In current setup, Door Sensor is added through the HabMin and therefore does not have a text configuration file.

## MySQL Persistence



    
