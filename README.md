# wifi-mikrotek-disc5lite
Miktotek DISC Lite5 AC Wifi 5 Mhz bridge longdistance.

Wifi 5 Mhz [Mikrotik Disc Lite5 ac](https://help.mikrotik.com/docs/display/UM/Disc+Lite5+ac)
 * ARM 32-bit
 * Qualcomm ...

[Buy](https://mikrotik.com/buy), [example](https://www.mikrotik-store.eu/en/index?language=en) from Germany.

I know that there has used 60 km point to point bridge over year, average speed 45 Mbps. Weather - no problem.



## Download

* [Firmware](https://mikrotik.com/product/disc_lite5_ac#fndtn-downloads) - look whick packet you need
  * current packages (=read firmware) for Disc Lite5 ac: *routeros-7.13.2-arm.npk* and wireless driver *wireless-7.13.2-arm.npk*
* [Winbox](https://mikrotik.com/download) - nice client to use RouteOS products
* [Netinstall](https://mikrotik.com/download) - bootp software, low level restore
 * [Netinstall doc](https://help.mikrotik.com/docs/display/ROS/Netinstall)

## Example configuration

Gateway 192.168.33.1  
 |  
 |  
 |__ host 192.168.33.20  
 |  
 |__ Mikrotek Disc 5 Lite ac - AP - 192.168.33.211  
&nbsp;&nbsp;&nbsp; |  
&nbsp;&nbsp;&nbsp; |  
&nbsp;&nbsp;&nbsp; | Wifi bridge  
&nbsp;&nbsp;&nbsp; |  
&nbsp;&nbsp;&nbsp; |   
 __| Mikrotek Disc 5 Lite ac - CPE/Station - 192.168.33.212  
 |  
 |  
 |__ host 192.168.33.19  
 |  

## Documentation
 * [RouterOS](https://help.mikrotik.com/docs/display/ROS/RouterOS)
 ** [Wireless](https://help.mikrotik.com/docs/display/ROS/Wireless+Interface)


