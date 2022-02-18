# Ensure Zoom calls go smooth!

Zoom traffic should be identified and added to an address list. We need the port numbers used by zoom services. These TCP ports (3478,3479,5090,5091,8801-8810) and UDP ports (3478,3479,5090,5091,8801-8810) are used to by zoom service and we try to identify Zoom IP Addresses. Those destination addresses that are not on the list and uses the mentioned tcp/udp ports should be added to the Zoom address list.
```
/ip firewall mangle
add chain=prerouting dst-address-list=!Zoom dst-port=3478,3479,5090,5091,8801-8810 protocol=tcp action=add-dst-to-address-list address-list=Zoom comment="Adding Zoom IPs to Address List";
add chain=prerouting dst-port=3478,3479,5090,5091,8801-8810 protocol=udp action=add-dst-to-address-list address-list=Zoom;
```
## Connection Marking

We need to mark all the connection going to destination Zoom addresses and also should mark those connections that uses 80 and 443 port without marking other non-Zoom traffic.
```
/ip firewall mangle
add chain=prerouting dst-address-list=Zoom protocol=tcp dst-port=3478,3479,5090,5091,8801-8810 action=mark-connection new-connection-mark=Zoom-Connection passthrough=yes comment="Mark Zoom Connections";
add chain=prerouting dst-address-list=Zoom protocol=udp dst-port=3478,3479,5090,5091,8801-8810 action=mark-connection new-connection-mark=Zoom-Connection passthrough=yes;
/ip firewall mangle add chain=prerouting protocol=tcp dst-port=80,443 dst-address-list=Zoom action=mark-connection new-connection-mark=Zoom-Connection passthrough=yes
```

## Packet Marking

Mark all the packets and use that mark in the queue and set the priority to 1.
```
/ip firewall mangle
add chain=prerouting action=mark-packet connection-mark=Zoom-Connection new-packet-mark=Zoom-packet passthrough=no comment="Mark Zoom Packets"
/queue simple add name=Zoom packet-marks=Zoom-packet priority=1/1 target=172.20.0.0/20
```
