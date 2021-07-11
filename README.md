# Generation of Genuine Channel Switch-Announcements in 802.11 Networks
This activity describes how to trigger Wi-Fi routers or hostapd to send genuine channel switch announcements. 
## Introduction
Hostapd (host access point daemon) is a user space daemon software enabling a network or wireless interface card to act as an access point and authentication server. 
hostapd_cli is a frontend program to interact with hostapd. It is used to query the status of hostapd and set parameters through command line interface.In this activity, hostapd_cli is used to send channel switch command over the control interface on which hostapd is running. Following are the detailed steps..

## Step 1-Create an AP(Access Point) with hostapd.
Please visit https://github.com/lucascouto/krackattacks-scripts) to see how to create an AP using hostapd so that Wi-Fi clients can connect to it.  

## Step 2-Update the hostapd.conf file with the control interface as follows
```
interface=wlan1
driver=nl80211
ssid=testnetwork
hw_mode=g
channel=11
macaddr_acl=0
ignore_broadcast_ssid=0
auth_algs=1
wpa=1
wpa_passphrase=123456781
wpa_key_mgmt=WPA-PSK
wpa_pairwise=TKIP
rsn_pairwise=CCMP
ctrl_interface=/var/run/hostapd

```
Note: Here the line " ctrl_interface=/var/run/hostapd" will create a client socket between hostapd and hostapd_cli program. By default, Hostapd_CLI will connect to the socket created in "/var/run/hostapd". If this line is not present in hostapd, the hostapd_cli may generate the error as follows.
```
hostapd_cli v2.9
Copyright (c) 2004-2019, Jouni Malinen <j@w1.fi> and contributors
 
This software may be distributed under the terms of the BSD license.
See README for more details.
 
 
Could not connect to hostapd - re-trying
```
## Step 3-Run the hostapd_cli 
Once the hostapd is up and running, and all clients are able to connect to it, hostapd_cli can be initiated by following command
```
hostapd_cli 
```
Above command will provide an interactive environment over command line interface as follows. 
<p align="center">
  <img src="https://raw.githubusercontent.com/lucascouto/mitm-images/master/mitm.png">
</p>
