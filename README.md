# RaspiConfiguration
This repo documents initial setup for raspberry pi needed for kernel development

## Setup LAN between raspi and laptop.

    After flashing OS image onto SD, make following edits to /etc/dhcpcd.conf
  
    interface eth0
    static ip_address=192.168.0.10/24  # Set the static IP for Raspberry Pi
    # static routers=192.168.1.1  # Optional, can be left out for direct connection
    # static domain_name_servers=8.8.8.8  # Optional, can be left out for direct connection.

    Then, reboot raspberry pi.

    On laptop, setup manual ip address specifying static IP address under the same subnet, such as
        192.168.0.123 
    Set subnet mask corresponding to 24,
        255.255.255.0

    Then attempt ssh.

## Network sharing from macbook to raspi.

    Enable *Internet Sharing* in *System Settings* 

    In terminal (macos)
    
    $ ifconfig | grep bridge
    
        bridge0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
        bridge100: flags=8a63<UP,BROADCAST,SMART,RUNNING,ALLMULTI,SIMPLEX,MULTICAST> mtu 1500
	    inet6 fe80::80a9:97ff:feb1:8d64%bridge100 prefixlen 64 scopeid 0x12 

       The second interface is created as bridge.

    $ ifconfig bridge100    
        bridge100: flags=8a63<UP,BROADCAST,SMART,RUNNING,ALLMULTI,SIMPLEX,MULTICAST> mtu 1500
	    options=63<RXCSUM,TXCSUM,TSO4,TSO6>
    	ether 82:a9:97:b1:8d:64
    	inet 192.168.2.1 netmask 0xffffff00 broadcast 192.168.2.255
    	inet6 fe80::80a9:97ff:feb1:8d64%bridge100 prefixlen 64 scopeid 0x12 
    	inet6 fd0b:75a1:70ac:8c26:184e:cf8:4353:67c9 prefixlen 64 autoconf secured 
    	Configuration:
    		id 0:0:0:0:0:0 priority 0 hellotime 0 fwddelay 0
    		maxage 0 holdcnt 0 proto stp maxaddr 100 timeout 1200
    		root id 0:0:0:0:0:0 priority 0 ifcost 0 port 0
    		ipfilter disabled flags 0x0
    	member: en12 flags=3<LEARNING,DISCOVER>
	            ifmaxaddr 0 port 16 priority 0 path cost 0
	    Address cache:
	    	e4:5f:1:70:cd:38 Vlan1 en12 1190 flags=0<>
	    nd6 options=201<PERFORMNUD,DAD>
	    media: autoselect
	    status: active

     The IPv4 address if 192.168.2.1, we need to make raspi under the same subnet.

    On raspi terminal,
    $ vim /etc/dhcpcd.conf

    Edit following: 
    
        interface eth0
        static ip_address=192.168.2.2/24
        # static ip6_address=fd51:42f8:caae:d92e::ff/64
        static routers=192.168.2.1
        static domain_name_servers=8.8.8.8
        # static domain_name_servers=192.168.0.1 8.8.8.8 fd51:42f8:caae:d92e::1

    And then restart dhcp by
    $ sudo systemctl restart dhcpcd

    Now try ssh into raspi.
     

## Setup raspi for loadable kernel module compilation

    $ sudo apt update

    $ sudo apt-get install raspberrypi-^Crnel-headers

    The downloaded headers will be located at /usr/src/linux-headers-`uname -r`


