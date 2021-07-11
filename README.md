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
Note: Here the line " **ctrl_interface=/var/run/hostapd**" will create a client socket between hostapd and hostapd_cli program. By default, **hostapd_cli** will connect to the socket created in "/var/run/hostapd". If this line is not present in hostapd, the hostapd_cli may generate the error as follows.
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
<p align="left">
  <img src="https://github.com/maneshthankappan/Analysis-of-Genuine-Channel-Switch-Announcements-in-802.11-Networks/blob/main/hostapd_cli_output">
</p>

## Step 4- Execute channel switch command 
The format of channel siwtch command is   **chan_switch <cs_count> <freq>**  , where cs_count represents no.of beacons after which the channel switch command
is sent or channel switch occur. freq represents the frequency of the new channel.For this activity, please note that the current channel of the AP is 6 (see Step 2). Before executing the channel switch command following figure shows the connected channel in a Windows 10 machine.

<p align="left">
  <img src="https://github.com/maneshthankappan/Analysis-of-Genuine-Channel-Switch-Announcements-in-802.11-Networks/blob/main/Channel_info_before_channel_switch.jpg">
</p>
 
 Then the command can be executed as...
 <p align="left">
  <img src="https://github.com/maneshthankappan/Analysis-of-Genuine-Channel-Switch-Announcements-in-802.11-Networks/blob/main/chan_switch_command">
</p>
 
 Here, note that the chan_switch command is successfull (as displayed as OK) and the AP is moved to channel 11 by sending channel switch announcements(CSA) to all associated clients. [Channel frequencies can be found here](https://github.com/maneshthankappan/Analysis-of-Genuine-Channel-Switch-Announcements-in-802.11-Networks/blob/main/channel_frequencies)
 Following figure shows the figure shows the switched channel in a Windows 10 machine.
 <p align="left">
  <img src="https://github.com/maneshthankappan/Analysis-of-Genuine-Channel-Switch-Announcements-in-802.11-Networks/blob/main/Channel_info_after_channel_switch.jpg">
</p>

## **Step 5-Analysis of PCAP file and channel switch announcements**.
To capture genuine channel switch announcement send by the hostapd,another wireless interface is put on monitor mode and must be in channel 11 to capture CSA beacons on the current channel.
```
 ifconfig wlan0 down
 iwconfig wlan0 set monitor
 ifconfig wlan0 up
 iw wlan0 set channel 11
```
The **network trace or pcap file** containing channel switch announcements can be [viewed online on cloudshark] (https://www.cloudshark.org/captures/d25ec1280611) 
Use the following **filter** to see beacons of the AP with SSID "testnetwork"
 ```
wlan.csa.channel_switch_mode
 ```
