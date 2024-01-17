# lib_wifi
network-manager(nmcli)를 이용한 command line wifi lib

### reference
* https://www.makeuseof.com/connect-to-wifi-network-on-ubuntu-server/
* https://www.makeuseof.com/connect-to-wifi-with-nmcli/
* https://changun516.tistory.com/120
* How to change wifi interface name to wlan0 from wlxe84e062e27e2
```
# 70-persistent-net.rules in the directory /etc/udev/rules.d
# SUBSYSTEM=="net", ACTION=="add", DRIVERS=="?*", ATTR{address}=="{wlan mac address}" , ATTR{dev_id}=="0x0", ATTR{type}=="1", KERNEL=="wlan*", NAME="wlan0"
SUBSYSTEM=="net", ACTION=="add", DRIVERS=="?*", ATTR{address}=="e8:4e:06:2e:27:e2" , ATTR{dev_id}=="0x0", ATTR{type}=="1", KERNEL=="wlan*", NAME="wlan0"
```
