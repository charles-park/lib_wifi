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

### 테스트 이미지 및 wifi module
* ODROID-M1S : https://dn.odroid.com/RK3566/ODROID-M1S/Ubuntu/ubuntu-20.04-server-odroidm1s-20231030.img.xz
* WIFI 5BK   : https://www.hardkernel.com/ko/shop/wifi-module-5bk/

### 설치 순서
```
// ubuntu system upgrade
root@server:/home/odroid# apt update && apt upgrade -y

// ubuntu package
root@server:~# apt install samba ssh build-essential python3 python3-pip ethtool net-tools usbutils git i2c-tools vim cups cups-bsd overlayroot nmap evtest
root@server:~# apt install network-manager wpasupplicant wireless-tools
```

### WIFI 접속 및 mac address 확인 (rtl8821cu module확인)
```
root@server:~# lsusb -t
/:  Bus 07.Port 1: Dev 1, Class=root_hub, Driver=dummy_hcd/1p, 480M
/:  Bus 06.Port 1: Dev 1, Class=root_hub, Driver=xhci-hcd/1p, 5000M
/:  Bus 05.Port 1: Dev 1, Class=root_hub, Driver=xhci-hcd/1p, 480M
    |__ Port 1: Dev 2, If 0, Class=Wireless, Driver=, 480M
    |__ Port 1: Dev 2, If 1, Class=Wireless, Driver=, 480M
    |__ Port 1: Dev 2, If 2, Class=Vendor Specific Class, Driver=rtl8821cu, 480M
/:  Bus 04.Port 1: Dev 1, Class=root_hub, Driver=ohci-platform/1p, 12M
/:  Bus 03.Port 1: Dev 1, Class=root_hub, Driver=ohci-platform/1p, 12M
    |__ Port 1: Dev 2, If 0, Class=Communications, Driver=cdc_acm, 12M
    |__ Port 1: Dev 2, If 1, Class=CDC Data, Driver=cdc_acm, 12M
/:  Bus 02.Port 1: Dev 1, Class=root_hub, Driver=ehci-platform/1p, 480M
/:  Bus 01.Port 1: Dev 1, Class=root_hub, Driver=ehci-platform/1p, 480M

root@server:~# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.20.36  netmask 255.255.255.0  broadcast 192.168.20.255
        inet6 fe80::21e:6ff:fe53:5d  prefixlen 64  scopeid 0x20<link>
        ether 00:1e:06:53:00:5d  txqueuelen 1000  (Ethernet)
        RX packets 6136  bytes 6999342 (6.9 MB)
        RX errors 0  dropped 2  overruns 0  frame 0
        TX packets 3023  bytes 294005 (294.0 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
        device interrupt 40  

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 30  bytes 3994 (3.9 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 30  bytes 3994 (3.9 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

wlxe0e1a933d4c3: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        ether e0:e1:a9:33:d4:c3  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

```

### ssh root user 설정
```
// root user add
root@server:~# passwd root
root@server:~# vi /etc/ssh/sshd_config
...
# PermitRootLogin prohibit-password
PermitRootLogin yes
StrictModes yes
PubkeyAuthentication yes
...

root@server:~# service ssh restart
```
