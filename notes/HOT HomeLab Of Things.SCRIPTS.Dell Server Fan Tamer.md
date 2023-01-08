---
id: qjohlc5mtovwgwiqfzdgwea
title: Dell Server Fan Tamer
desc: ''
updated: 1673141563618
created: 1673137361927
---

Copy the code below and modify to your content. Comments inside the script explain how to use this script and what tools are required.
I have used this script on __Dell R710__ (an oldie I know) with a great success. 
With some tweaking it will work with other models too.
I have adapted it to __Dell R210__ also but those servers have minimum fan speed around 5250 RPM which is still audible. Below that speed, fans just stop which is not really desirable ;) 

>` #!/bin/bash`
>
	#This script makes Dell servers like R710 and other super quiet for home lab use.  
	#I have chosen readings from hottest cores on each cpu in order to regulate the fan speed.  
	#To control fan speed we need ipmitool.  
	#Install with: <sudo apt install ipmitool>  
	#To identify hot cores or to check cpu temperatures in general, use sensors.  
	#To install: <sudo apt install lm-sensors>  
	#I running it as a cron job every 1 minute.  
	#Server on every reboot/power loss will revert to default fan speed control.  
>
	hotcorecpu1=$(sensors coretemp-isa-0000 | grep "Core 9:" | awk -F '[+.0]' {'print $2'})  
	hotcorecpu2=$(sensors coretemp-isa-0001 | grep "Core 0:" | awk -F '[+.0]' {'print $2'})  
>	
	#Switch off automatic fan speed control
	#You can leave this command here or add it as a cron job on each reboot and then comment it out here. 
	#Without it automatic fan control will be restored within few seconds.
>	
	ipmitool -U root -P calvin raw 0x30 0x30 0x01 0x00
>
	#HOW I RUN IT:
	#Add this record to your crontab -e (root cronjobs):
	#	*/1 * * * *     /bin/fancontrol.sh #to run script every 1 minute in root cronjobs
>
	#Testing readings:
	#echo $hotcorecpu1
	#echo $hotcorecpu2
>
>
	if [[($hotcorecpu1 -ge 1) || ($hotcorecpu2 -ge 1)]] && [[($hotcorecpu1 -lt 41) || ($hotcorecpu2 -lt 41)]]
		then
		ipmitool -U root -P calvin raw 0x30 0x30 0x02 0xff 0x04 #4% of fan speed
	elif [[($hotcorecpu1 -ge 41) || ($hotcorecpu2 -ge 41)]] && [[($hotcorecpu1 -lt 45) || ($hotcorecpu2 -lt 45)]]
		then
		ipmitool -U root -P calvin raw 0x30 0x30 0x02 0xff 0x0a #10% of fan speed
	elif [[($hotcorecpu1 -ge 45) || ($hotcorecpu2 -ge 45)]] && [[($hotcorecpu1 -lt 51) || ($hotcorecpu2 -lt 51)]]
		then
		ipmitool -U root -P calvin raw 0x30 0x30 0x02 0xff 0x14 #20% of fan speed
	elif [[($hotcorecpu1 -ge 51) || ($hotcorecpu2 -ge 51)]] && [[($hotcorecpu1 -lt 55) || ($hotcorecpu2 -lt 55)]]
		then
		ipmitool -U root -P calvin raw 0x30 0x30 0x02 0x23 0x1e #35% of fan speed
	elif [[($hotcorecpu1 -ge 55) || ($hotcorecpu2 -ge 55)]] && [[($hotcorecpu1 -lt 64) || ($hotcorecpu2 -lt 64)]]
		then
        ipmitool -U root -P calvin raw 0x30 0x30 0x02 0xff 0x32 #50% of fan speed
	else #panic mode
		ipmitool -U root -P calvin raw 0x30 0x30 0x02 0xff 0x50 #80% of fan speed
	fi
>
	#To check fans speed at any time run (this command should work remotely also as it is connects to your iDrac):
	# ipmitool -I lanplus -H <YOUR IP> -U root -P calvin sdr elist all | grep -i fan
>	
>	#Enjoy silent server!

Download:
[fancontrol.sh](https://github.com/Greguus/Notes/fancontrol.sh)