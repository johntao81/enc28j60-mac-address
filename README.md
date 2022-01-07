# enc28j60-mac-address
Notes for setting the MAC address on the ENC28J60 module for Raspberry Pi

Reference: https://www.raspberrypi-spy.co.uk/2020/05/adding-ethernet-to-a-pi-zero/

`sudo nano /lib/systemd/system/setmac.service`

```
[Unit]
Description=Set MAC address for ENC28J60 module
Wants=network-pre.target
Before=network-pre.target
BindsTo=sys-subsystem-net-devices-eth0.device
After=sys-subsystem-net-devices-eth0.device

[Service]
Type=oneshot
ExecStart=/sbin/ip link set dev eth0 down
ExecStart=/sbin/ip link set dev eth0 address b8:27:eb:5e:39:db
ExecStart=/sbin/ip link set dev eth0 up

[Install]
WantedBy=multi-user.target
```


```
sudo chmod 644 /lib/systemd/system/setmac.service
sudo systemctl daemon-reload
sudo systemctl enable setmac.service
```
`sudo nano /etc/systemd/network/00-eth0.link`

```
[Match]
Interface=eth0

[Link]
MACAddress=b8:27:eb:5e:39:dc
Duplex=full
AutoNegotiation=no
```
```
pi@octopi:~ $ sudo ethtool eth0
Settings for eth0:
        Supported ports: [ TP ]
        Supported link modes:   10baseT/Half 10baseT/Full
        Supported pause frame use: No
        Supports auto-negotiation: No
        Supported FEC modes: Not reported
        Advertised link modes:  Not reported
        Advertised pause frame use: No
        Advertised auto-negotiation: No
        Advertised FEC modes: Not reported
        Speed: 10Mb/s
        Duplex: Half
        Port: Twisted Pair
        PHYAD: 0
        Transceiver: internal
        Auto-negotiation: off
        MDI-X: Unknown
        Current message level: 0x00000036 (54)
                               probe link ifdown ifup

```

