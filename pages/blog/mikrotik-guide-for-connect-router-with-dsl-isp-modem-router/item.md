---
title: 'Mikrotik - Guide for connect router with ISP modem/router'
date: '10:09 18-07-2020'
taxonomy:
    category:
        - mikrotik
        - guide
    tag:
        - Mikrotik
        - Router
        - 'Router Setup Guide'
        - SFR
hide_git_sync_repo_link: false
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
hide_from_post_list: false
feed:
    limit: 10
---

This tutorial is for setting up a MikroTik router hap-ac2, to setup to the modem/router provided by my ISP, and use the latter as just a modem.

### Text diagram of network
ISP Modem (public IP) - > MikroTik on ether1 (client dhcp set to grab public IP from ISP) - > ether2-ether5 will be bridge switch offering DHCP to my lan.

===

Plug the ISP modem into ether1 on the MikroTik. This will become your WAN port. The other ports will become switch ports called bridge-LAN.

Below is a sample config for you. Plug a laptop/PC into ether2. Then negotiate a connection to the MikroTik with Winbox using a MAC address. Go to System / Reset Configuration. Check No Default and Do Not Backup. Reboot. Then slowly copy paste each section of this script into a terminal window. If you get disconnected, just connect back again.

### Configuration code

>     # Configure ports
>     /interface ethernet
>     set [ find default-name=ether1 ] master-port=none
>     set [ find default-name=ether2 ] master-port=none
>     set [ find default-name=ether3 ] master-port=ether2
>     set [ find default-name=ether4 ] master-port=ether2
>     set [ find default-name=ether5 ] master-port=ether2
>     
>     
>     # Create and setup bridge
>     /interface bridge
>     add name=bridge-LAN protocol-mode=none
>     /interface bridge port
>     add bridge=bridge-LAN interface=ether2
>     add bridge=bridge-LAN interface=wlan1
>     /interface bridge settings
>     set use-ip-firewall=yes
>     
>     
>     # Give bridge an IP
>     /ip address
>     add interface=bridge-LAN address=192.168.88.1/24
>     
>     
>     # Get an IP from ISP
>     /ip dhcp-client
>     add default-route-distance=0 dhcp-options=hostname,clientid disabled=no interface=ethe1
>     
>     
>     # Setup Wireless
>     /interface wireless
>     set [ find default-name=wlan1 ] band=2ghz-b/g/n disabled=no distance=indoors wps-mode=disabled frequency=auto mode=ap-bridge ssid="MYSSID"
>     /interface wireless security-profiles
>     set [ find default=yes ] authentication-types=wpa2-psk group-key-update=60m mode=dynamic-keys supplicant-identity=MikroTik wpa2-pre-shared-key="PASSWORD"
>     
>     
>     # Setup DHCP for LAN
>     /ip pool
>     add name=dhcp_pool1 ranges=192.168.88.10-192.168.88.254
>     /ip dhcp-server
>     add add-arp=yes address-pool=dhcp_pool1 always-broadcast=yes authoritative=yes disabled=no interface=bridge-LAN lease-time=1d name=dhcp1
>     /ip dhcp-server network
>     add address=192.168.88.0/24 dns-server=192.168.88.1 domain=home.local gateway=192.168.88.1
>     
>     
>     # Setup DNS for LAN
>     /ip dns
>     set allow-remote-requests=yes cache-max-ttl=5d
>     
>     
>     # Setup WAN port
>     /ip firewall nat
>     add action=masquerade chain=srcnat comment="Default masq" out-interface=ether1
>     
>     
>     # Setup WAN firewall
>     /ip firewall filter
>     add action=accept chain=input comment="Accept established related" connection-state=established,related
>     add action=accept chain=input comment="Allow LAN access to router and Internet" in-interface=bridge-LAN
>     add action=accept chain=input comment="Allow ping ICMP from anywhere" protocol=icmp
>     add action=drop chain=input comment="Drop all other input"
>     add action=accept chain=forward comment="Accept established related" connection-state=established,related
>     add action=accept chain=forward comment="Allow LAN access to router and Internet" connection-state=new in-interface=bridge-LAN
>     add action=accept chain=forward comment="Accept Port forwards" connection-nat-state=dstnat in-interface=ether1
>     add action=drop chain=forward comment="Drop all other forward"
>     
>     
>     # Turn off unneeded helpers
>     /ip firewall service-port
>     set ftp disabled=yes
>     set tftp disabled=yes
>     set irc disabled=yes
>     set h323 disabled=yes
>     set sip disabled=yes
>     set pptp disabled=yes
>     set udplite disabled=yes
>     set dccp disabled=yes
>     set sctp disabled=yes
>     
>     
>     # Turn off unneeded services
>     /ip service
>     set telnet disabled=yes
>     set ftp disabled=yes
>     set api disabled=yes
>     set ssh address=192.168.88.0/24
>     set winbox address=192.168.88.0/24
>     set api-ssl disabled=yes
>     
>     
>     # Misc IP settings
>     /ip ssh
>     set strong-crypto=yes
>     /ip settings
>     set rp-filter=strict secure-redirects=no send-redirects=no tcp-syncookies=yes
>     

### Source
[Mikrotik - Guide for connection mikrotik router with dsl isp modem/router - https://forum.mikrotik.com/viewtopic.php?t=126158#](https://forum.mikrotik.com/viewtopic.php?t=126158#)