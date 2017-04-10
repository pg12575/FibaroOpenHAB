# FibaroOpenHAB

Using Fibaro Sensors with OpenHAB2 to display simple information using *BasicUI* sitemap, setting up persistence using *mySQL* and remote access.
Hardware used (so far) - 
- Fibaro Devices 
    - Fibaro (Motion) MultiSensor  - *FGMS-001*
    - Fibaro Door/Window Sensor - *FGK-101-107*
    - Fibaro Wall Plug - *FGWPE-101*
- Aeotec Z-Wave Stick Gen5

*In the current setup, PaperUI is used to discover and add new devices while HabMin provides a better UI for configuration with a mix of text configuration files*
OpenHAB UI's can be accessed at http://ip-network-of-device:8080

### Location of openHAB Files on Linux
- openHAB application -           */usr/share/openhab2*
    - Includes *addons, bin, runtime, start.sh, start_debug.sh*
- Site configuration -	        */etc/openhab2*
    - Includes *html, icons, items, persistence, rules, scripts, services, sitemaps, sounds, things, transform*
- Log files -     	            */var/log/openhab2*
    - Includes *events.log, openhab.log*
- Userdata like rrd4j databases - */var/lib/openhab2*
    - Includes *cache, config, etc, hueemulation, jsondb, kar, log, persistence, tmp, **zwave***
        - **Important: zwave folder includes .xml files for each node/device**
- Service configuration - 	    */etc/default/openhab2*

[More info..](http://docs.openhab.org/installation/linux.html#file-locations)

*For [openHABian](https://github.com/openhab/openhabian/releases)* - 
- *username:password = openhabian*
- *all folders located at - openhabian@openhabianpi:/$*

### Important Linux Commands

```sh
sudo systemctl status openhab2.service 	// check status of openHAB
sudo systemctl start openhab2.service 	// start openHAB
sudo systemctl stop openhab2.service	// stop openHAB
sudo systemctl restart openhab2.service	// restart openHAB
```
*Restart is sometimes required when .sitemap file is changed to re-sync Basic UI so it updates in realtime.*

## Z-Wave Binding
Install ZWave binding using *PaperUI*.
New devices can be added using *PaperUI*'s inbox discovery feature. If device not correctly recognised/initialized, wake up again by triple clicking the b button. 
Ignore Z-Wave Binding instructions available online as they only seem appropriate for OH1, follow this for OH2.

*NOTE: HabMin currently buggy for adding Z-Wave devices, so only used for configuring them at the moment*

*Reminder: Don't forget to wake up battery powered devices when any changes to .items file is made*


## Aeotec Z-Wave Stick Gen5 Instructions
For inclusion/exclusion of sensors manually (not usually required!).
[Aeotec User Manual](https://aeotec.freshdesk.com/support/solutions/articles/6000056439-z-stick-gen-5-user-manual-)

## HabMin, .items File and Sitemap (Adding Fibaro Devices)
HabMin used for adding and managing devices (installed using *PaperUI*). 

### Serial Controller

- Add *Serial Controller* automatically discovered once Aeotec Stick plugged in.
    - Enter Port - discovered by typing  **$ ls /dev/tty***
- Add Fibaro devices discovered by triple clicking B button (waking device up).
- Device Node and Channels UID can be discovered once device is added. For Fibaro devices, following can be used in the .items file 

### Fibaro Motion/Multi Sensor

      Switch	 motionSensor         "Motion [%s]" 	<motion> 	(mSensor)	{ channel="zwave:device:15b4d86c1b1:node4:alarm_motion" }
    Number mSensorBattery1   	"Battery [%s]" 	<battery> 			{ channel="zwave:device:15b5860e0b8:node4:battery-level" }
   
  **OR**
  
```    
    Number	motionSensor1	 	"Motion Node 4 [%s]" <motion> 	(mSensor) 	{channel="zwave:device:15b5860e0b8:node4:alarm_motion" }
    
    Number 	mSensorBattery1   	"Battery [%s]"       <battery> 		{ channel="zwave:device:15b5860e0b8:node4:battery-level" }
```
    
[More info here](https://community.openhab.org/t/solved-fibaro-fgms-001-cannot-see-alarm-off-on-in-gui-paperui/25685/8)

*alarm_motion* can be set as Switch or Number to display ON/OFF or 1/0.

*Note: In current binding sensor_binary is not functioning properly. As far as I can tell it gets triggered only when the device is woken up manually (by tripple clicking and b button) and stays on till next wake up time.*

### Fibaro Door/Window Sensor


    
    Number doorSensor1    "Door Node 5 [%s]" <window>  { channel="zwave:device:15b5860e0b8:node5:sensor_door"}
    
    
### Fibaro Wall Plug
```
Switch heater    "Plug Node 7 [%s]"  { channel="zwave:device:15b5860e0b8:node7:switch_binary"}

Number heaterWatts    "Watts [%s]"  <battery>{ channel="zwave:device:15b5860e0b8:node7:sensor_power"}

Number heaterKWH    "KWH [%s]"  <battery>{ channel="zwave:device:15b5860e0b8:node7:meter_kwh"}
```
*Note: meter_watts is not working in current binding. Instead use sensor_power.*

### Corresponding sitemap - 

```
 sitemap smarthome label="FibaroOpenHAB SmartHome" {

    Frame label="Movement" {


	Text item=motionSensor1 
	Text item=mSensorBattery1 

	Text item=motionSensor2 
	Text item=mSensorBattery2

	Text item=doorSensor1
	Text item=dSensorBattery1 
    }

    Frame label="Heating" {
	
	Switch item=heater

	Text item=heaterWatts
	Text item=heaterKWH
	
	} 
}
 
```
    
Here is a snapshot of the BasicUI setup through this sitemap (without wall plug):
![BasicUI](https://cloud.githubusercontent.com/assets/10930753/24872118/26adaffc-1e14-11e7-8240-5e51d805fa8e.png)

*Don't forget to change Basic UI sitemap name through Paper UI.*


[Rest of the document to be updated..]

## MySQL Persistence
[Instructions](https://community.openhab.org/t/openhab2-mysql-persistence-setup/15829)

## Remote Access
[Instructions](https://github.com/openhab/openhab-cloud/blob/master/README.md)



    
