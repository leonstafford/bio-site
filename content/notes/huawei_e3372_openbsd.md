+++
date = "2018-12-13"
title = "Huawei E3372 USB 3G/4G modem under OpenBSD 6.4"
tags = ["openbsd", "usb modem"]
categories = []
+++

Notes on getting connected to 3G/4G in Thailand on the dtac prepaid SIM

```
obsd# gammu identify
Device               : /dev/cuaU0
Manufacturer         : Huawei
Model                : E3372 (E3372)
Firmware             : 22.329.63.00.74
IMEI                 : 866785033700732
SIM IMSI             : 520050314388175
```

Example `smsd.conf`

```
loglevel = 7

user = _smsd

pidfile = /var/run/smsd/smsd.pid
infofile = /var/run/smsd/smsd.info

logfile = /var/log/smsd/smsd.log

[GSM1]
device = /dev/cuaU0

init = AT^CURC=0

# For this one I have set incoming=no so it doesn't pull
# the existing messages off the phone.
incoming = no
rtscts = no
baudrate = 115200
eventhandler = /home/leon/smstoolsevents.sh
send_delay = 20
```

And a simple script to log events `/home/leon/smstoolsevents.sh`

```


#!/bin/ksh

# script to react to events, like receiving an SMS

echo '' >> /home/leon/logsmsevents
echo 'received a message' >> /home/leon/logsmsevents


```

`smsd` log with above settings gives the following when `smsd` is restarted:

```
2018-12-13 12:24:38,6, GSM1: Modem is registered to a roaming partner network                                                                                                             [79/2614]
2018-12-13 12:24:38,2, smsd: Smsd mainprocess is awaiting the termination of all modem handlers. PID: 830.                                                                                        
2018-12-13 12:24:38,2, GSM1: Modem handler 0 terminated. PID: 61548, was started 18-12-13 12:15:24, up 9 min.                                                                                     
2018-12-13 12:24:38,2, smsd: Smsd mainprocess terminated. PID: 830, was started 18-12-13 12:15:24, up 9 min.                                                                                      
2018-12-13 12:24:38,2, smsd: Smsd v3.1.21 started.
2018-12-13 12:24:38,2, smsd: Running as _smsd:_smsd (590:590).
2018-12-13 12:24:38,7, smsd: Running startup_check (shell): /var/spool/sms/incoming/smsd_script.FZMA0a /tmp/smsd_data.sP5YHd                                                                      
2018-12-13 12:24:38,7, smsd: Done: startup_check (shell), execution time 0 sec., status: 0 (0)
2018-12-13 12:24:38,4, smsd: File mode creation mask: 022 (0644, rw-r--r--).
2018-12-13 12:24:38,5, GSM1: Modem handler 0 has started. PID: 50667.
2018-12-13 12:24:38,5, GSM1: Using check_memory_method 1: CPMS is used.
2018-12-13 12:24:38,5, smsd: Outgoing file checker has started. PID: 80713.
2018-12-13 12:24:38,7, smsd: All PID's: 80713,50667
2018-12-13 12:24:38,6, GSM1: Checking device for incoming SMS
2018-12-13 12:24:38,6, GSM1: Checking if modem is ready
2018-12-13 12:24:39,7, GSM1: -> AT
2018-12-13 12:24:39,7, GSM1: Command is sent, waiting for the answer. (5)
2018-12-13 12:24:39,7, GSM1: <- OK
2018-12-13 12:24:39,6, GSM1: Pre-initializing modem
2018-12-13 12:24:39,7, GSM1: -> ATE0
2018-12-13 12:24:39,7, GSM1: Command is sent, waiting for the answer. (10)
2018-12-13 12:24:39,7, GSM1: <- OK                                                                                                                                                                
2018-12-13 12:24:39,7, GSM1: -> AT+CMEE=1;+CREG=2
2018-12-13 12:24:40,7, GSM1: Command is sent, waiting for the answer. (10)
2018-12-13 12:24:40,7, GSM1: <- OK
2018-12-13 12:24:40,6, GSM1: Checking if modem needs PIN
2018-12-13 12:24:40,7, GSM1: -> AT+CPIN?
2018-12-13 12:24:40,7, GSM1: Command is sent, waiting for the answer. (10)
2018-12-13 12:24:40,7, GSM1: <- +CPIN: READY OK
2018-12-13 12:24:40,6, GSM1: Initializing modem
2018-12-13 12:24:40,7, GSM1: -> AT^CURC=0
2018-12-13 12:24:41,7, GSM1: Command is sent, waiting for the answer. (10)
2018-12-13 12:24:41,7, GSM1: <- OK
2018-12-13 12:24:41,7, GSM1: -> AT+CSQ
2018-12-13 12:24:41,7, GSM1: Command is sent, waiting for the answer. (10)
2018-12-13 12:24:41,7, GSM1: <- +CSQ: 22,99 OK
2018-12-13 12:24:41,6, GSM1: Signal Strength Indicator: (22,99) -69 dBm (Excellent), Bit Error Rate: not known or not detectable                                                                  
2018-12-13 12:24:41,6, GSM1: Checking if Modem is registered to the network
2018-12-13 12:24:41,7, GSM1: -> AT+CREG?
2018-12-13 12:24:42,7, GSM1: Command is sent, waiting for the answer. (10)
2018-12-13 12:24:42,7, GSM1: <- +CREG: 2,5,"792D","04947221" OK
2018-12-13 12:24:42,6, GSM1: Modem is registered to a roaming partner network
2018-12-13 12:24:42,6, GSM1: Location area code: 792D, Cell ID: 7221
2018-12-13 12:24:42,7, GSM1: -> AT+CSQ
2018-12-13 12:24:42,7, GSM1: Command is sent, waiting for the answer. (10)
2018-12-13 12:24:42,7, GSM1: <- +CSQ: 21,99 OK
2018-12-13 12:24:42,6, GSM1: Signal Strength Indicator: (21,99) -71 dBm (Excellent), Bit Error Rate: not known or not detectable                                                                  
2018-12-13 12:24:42,6, GSM1: Selecting PDU mode
2018-12-13 12:24:42,7, GSM1: -> AT+CMGF=0
2018-12-13 12:24:42,7, GSM1: Command is sent, waiting for the answer. (5)
2018-12-13 12:24:42,7, GSM1: <- OK
2018-12-13 12:24:43,7, GSM1: -> AT+CGSN
2018-12-13 12:24:43,7, GSM1: Command is sent, waiting for the answer. (5)
2018-12-13 12:24:43,7, GSM1: <- 866785033700732 OK                                                                                                                                        [26/2614]
2018-12-13 12:24:43,5, GSM1: IMEI: 866785033700732
2018-12-13 12:24:43,7, GSM1: -> AT+CIMI
2018-12-13 12:24:43,7, GSM1: Command is sent, waiting for the answer. (5)
2018-12-13 12:24:43,7, GSM1: <- 520050314388175 OK
2018-12-13 12:24:43,5, GSM1: IMSI: 520050314388175
2018-12-13 12:24:43,6, GSM1: Checking if reading of messages is supported
2018-12-13 12:24:43,7, GSM1: -> AT+CPMS?
2018-12-13 12:24:44,7, GSM1: Command is sent, waiting for the answer. (5)
2018-12-13 12:24:44,7, GSM1: <- +CPMS: "ME",0,20,"ME",0,20,"ME",0,20 OK
2018-12-13 12:24:44,6, GSM1: Checking memory size
2018-12-13 12:24:44,7, GSM1: -> AT+CPMS?
2018-12-13 12:24:44,7, GSM1: Command is sent, waiting for the answer. (5)
2018-12-13 12:24:44,7, GSM1: <- +CPMS: "ME",0,20,"ME",0,20,"ME",0,20 OK
2018-12-13 12:24:44,6, GSM1: Used memory is 0 of 20
2018-12-13 12:24:44,6, GSM1: No SMS received
2018-12-13 12:24:54,6, GSM1: Checking device for incoming SMS
2018-12-13 12:24:54,6, GSM1: Checking if modem is ready
2018-12-13 12:24:54,7, GSM1: -> AT
2018-12-13 12:24:55,7, GSM1: Command is sent, waiting for the answer. (5)
2018-12-13 12:24:55,7, GSM1: <- OK
2018-12-13 12:24:55,6, GSM1: Pre-initializing modem
2018-12-13 12:24:55,7, GSM1: -> ATE0
2018-12-13 12:24:55,7, GSM1: Command is sent, waiting for the answer. (10)
2018-12-13 12:24:55,7, GSM1: <- OK
2018-12-13 12:24:55,7, GSM1: -> AT+CMEE=1;+CREG=2
2018-12-13 12:24:56,7, GSM1: Command is sent, waiting for the answer. (10)
2018-12-13 12:24:56,7, GSM1: <- OK
2018-12-13 12:24:56,6, GSM1: Initializing modem
2018-12-13 12:24:56,7, GSM1: -> AT^CURC=0
2018-12-13 12:24:56,7, GSM1: Command is sent, waiting for the answer. (10)
2018-12-13 12:24:56,7, GSM1: <- OK
2018-12-13 12:24:56,7, GSM1: -> AT+CSQ
2018-12-13 12:24:57,7, GSM1: Command is sent, waiting for the answer. (10)
2018-12-13 12:24:57,7, GSM1: <- +CSQ: 21,99 OK
2018-12-13 12:24:57,6, GSM1: Signal Strength Indicator: (21,99) -71 dBm (Excellent), Bit Error Rate: not known or not detectable                                                                  
2018-12-13 12:24:57,6, GSM1: Checking if Modem is registered to the network
2018-12-13 12:24:57,7, GSM1: -> AT+CREG?
2018-12-13 12:24:57,7, GSM1: Command is sent, waiting for the answer. (10)
2018-12-13 12:24:57,7, GSM1: <- +CREG: 2,5,"792D","04947221" OK
2018-12-13 12:24:57,6, GSM1: Modem is registered to a roaming partner network
2018-12-13 12:24:57,6, GSM1: Selecting PDU mode
2018-12-13 12:24:57,7, GSM1: -> AT+CMGF=0
2018-12-13 12:24:57,7, GSM1: Command is sent, waiting for the answer. (5)
2018-12-13 12:24:57,7, GSM1: <- OK
2018-12-13 12:24:57,6, GSM1: Checking memory size
2018-12-13 12:24:58,7, GSM1: -> AT+CPMS?
2018-12-13 12:24:58,7, GSM1: Command is sent, waiting for the answer. (5)
2018-12-13 12:24:58,7, GSM1: <- +CPMS: "ME",0,20,"ME",0,20,"ME",0,20 OK
2018-12-13 12:24:58,6, GSM1: Used memory is 0 of 20
2018-12-13 12:24:58,6, GSM1: No SMS received
```

and continues to poll 


Now, we need to set up pppd. Assuming that no received activation SMS is due to not registering on the data network yet, so leaving smsd running and will attempt the ppd setup

[back](/)
