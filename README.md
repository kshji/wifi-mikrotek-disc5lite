# wifi Mikrotek Disc Lite5 ac
Miktotek DISC Lite5 AC Wifi 5 Ghz bridge longdistance.

Wifi 5 Ghz [Mikrotik Disc Lite5 ac](https://help.mikrotik.com/docs/display/UM/Disc+Lite5+ac)
 * ARM 32-bit
 * PoE
 * Wireless 5 GHz chip model Qualcomm IPQ-4018 
 * Antenna gain 21 dBi for 5 GHz

[Mikrotik Disc Lite5 ac - Specifications](https://mikrotik.com/product/disc_lite5_ac)

[Buy](https://mikrotik.com/buy), [example](https://www.mikrotik-store.eu/en/index?language=en) from Germany.

I know that there has used 60 km point to point bridge over year, average speed 45 Mbps. Weather - no problem.

## Test result
Used bridge confuguration (AP + CPE).  
Used nv2 protocol, better than 802.11. 
Nv2 need pre-shared password. NV2 setup include cell radius setup, example 30 km.
Very sort test, tested on the lake ice at around -1°C in cloudy weather. 
Test done without any special adjustments/setups.
 * 130 meter - TCP/IP speed almost 400 Mbps, UDP over 400 Mbps, signal level 5
 * 300 m - TCP/IP speed 110-150 Mbps, signal level 4-5
 * 600 m - TCP/IP speed 130-150 Mbps
 * 1000 m - TCP/IP speed 60-100 Mbps, signal level 3
 * 1500 m - TCP/IP speed 40-60 Mbps, signal level 2-3

## Download

 * [Firmware/Packages](https://mikrotik.com/product/disc_lite5_ac#fndtn-downloads) - look whick packages you need
   * current packages (=read firmware) for Disc Lite5 ac: *routeros-7.13.2-arm.npk* and wireless driver *wireless-7.13.2-arm.npk*
 * [Winbox](https://mikrotik.com/download) - nice client to use RouteOS products
   * use macaddress or ip-address
 * [Netinstall](https://mikrotik.com/download) - bootp software, low level restore
   * [Netinstall doc](https://help.mikrotik.com/docs/display/ROS/Netinstall)

## Example configuration

Gateway 192.168.33.1  
 |  
 |  
 |-- host 192.168.33.19  
 |  
 |__ Mikrotek Disc Lite5 ac - AP - 192.168.33.211  name: EX1AP
&nbsp;&nbsp;&nbsp; |  
&nbsp;&nbsp;&nbsp; |  
&nbsp;&nbsp;&nbsp; | Wifi bridge  SSID EX1  
&nbsp;&nbsp;&nbsp; |  
&nbsp;&nbsp;&nbsp; |   
 &nbsp;__| Mikrotek Disc Lite5 ac - CPE/Station - 192.168.33.212  name: EX1CPE
 |  
 |  
 |-- host 192.168.33.20  
 |  

 * DHCP is available over the wireless bridge

## Documentation
 * [RouterOS](https://help.mikrotik.com/docs/display/ROS/RouterOS)
   * [Wireless](https://help.mikrotik.com/docs/display/ROS/Wireless+Interface)


## Configuration
I used Winbox, GUI and also Terminal mode.
 * start Winbox
 * boot Disc
 * wait - some minutes
 * you will see disc mac address on the list, select it clicking mac address
 * connect using user **admin** without password
 * after 1st boot ask new password, old is empty

I reset configuration, not used defaults. Use Terminal:
```
/system reset-configuration no-defaults=yes skip-backup=yes

```

## Install packages
System => Packages
 * drop  *routeros-7.13.2-arm.npk* and wireless driver *wireless-7.13.2-arm.npk* to this box
 * reboot

After reboot check System => Packages, you'll see that there are two packages installed

## Setup Identity
Winbox => Terminal or System => Identity
```
# AP - set
/system identity set name=EX1AP
# CPE / Station - set
/system identity set name=EX1CPE
```
## Enable wlan1
Default is: off

Winbox => Wireless => Wireless => Wifi Interfaces => enable line wlan1 
 Or using Terminal
```
/interface set wlan1 disabled=no
```


## Setup Bridge
Winbox => Bridge => 
 * + (add) - bridge1
 * Ports add wlan1 and ether1

```
/interface bridge port 
add bridge=bridge1 interface=wlan1 
add bridge=bridge1 interface=ether1
```


## Setup IP
Winbox => IP => Address list => + (new)
* AP 192.168.33.211, network 255.255.255.0, Interface: **bridge1**
* CPE 192.168.33.212, network 255.255.255.0, Interface: **bridge1**

Default route (gateway)
Winbox => IP => Routing
* add Dst.Addr. 0.0.0.0 Gateway 192.168.33.1

```
/ip address add address=192.168.33.211/24 interface=bridge1 network=192.168.33.0
/ip route add disabled=no dst-address=0.0.0.0/0 gateway=192.168.33.1 routing-table=main suppress-hw-offload=no
```


## Setup DNS
Winbox => IP => DNS 
Servers: ex. 8.8.8.8
```
/ip dns set allow-remote-requests=yes servers=8.8.8.8
```

## Setup Clock - NTP Client
Log is easier to read if setup clock.

Winbox => System => Clock
* timezone
* ntp server

```
/system clock set time-zone-name=Europe/Helsinki
/system ntp client set enabled=yes
/system ntp client servers add address=fi.pool.ntp.org
```

## Setup Wireless

Winbox => Interfaces => wlan1 => tab: Wireless

* change Advanced Mode
* AP = Mode: bridge
* CPE = Mode: station bridge
* Band 
  * AP: 5 GHz A/N/AC
  * CPE: 5Hz-only-AC = work in shorter distance (in my test under 1.5 km)
    * CPE try more fixed band and channel width, AP has more dynamic setup (=listen)
* Channel width: 20/40/80Mhz eeeC worked in my test, longer distance width have to be 10/20 ?
* Frequency: ex. 5580 
* SSID: EX1
* Radioname - only for more readable reading
  AP: EX1AP
  CPE: EX1CPE
* Wireless Protocol: nv2
  * NV2 tab setup security and Pre-shared password 
* Country: select your country

## Testing
* put Discs on
* wait some minutes
* open Winbox

### Winbox using 
Winbox => Wireless => Wireless

* select wlan1 line
* mouse second button select mode which give more information
* 1st col status is S and after it has connected to the other unit, status is RS
* tab Registration you will see your radioname EX1...
* Tools => Ping
  * 192.168.33.211  (=your own)
  * 192.168.33.19 (=your host)
  * 192.168.33.211 (=CPE)
  * 192.168.33.20 (=PC over wifi bridge)

## Config export - import
Use Winbox => Terminal

Save config to the Disc flashdisk
```
/export hide-sensitive file=EX1AP.nv2.config_20230121
```

Restore config from the Disc flashdisk
```
/import file=EX1AP.nv2.config_20230121.rsc
```

Winbox => Files
* you'll see your files
* you can drag&drop those files to your PC


## Backup - Restore
Winbox => Terminal

Some examples, default is crypted.
```
/system backup save name=backup.1.backup password=somemypwd
/system backup save name=backup.2.backup 

/system backup save dont-encrypt=yes name="Backup_$Identity"

/system backup save dont-encrypt=yes name="EX1AP_backup_20230121"

```
Restore
```
/system backup load name="backup.1.backup"
# if include password, restore will ask it
```

## Speedtest
Winbox include speedtest, look Tools
* server
* client

I used OpenSpeedtest Server, it's real TCP/IP test.

[Speed-Test](https://github.com/openspeedtest/Speed-Test)

Server: install using Microsoft Store (OpenSpeedTest-Server)
* start it, it will so the test link

Client, use browser and give the previous link

