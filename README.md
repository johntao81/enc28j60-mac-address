Reference: https://www.raspberrypi-spy.co.uk/2020/05/adding-ethernet-to-a-pi-zero/

sudo nano /lib/systemd/system/setmac.service

[Unit]
Description=Set MAC address for ENC28J60 module
Wants=network-pre.target
Before=network-pre.target
BindsTo=sys-subsystem-net-devices-eth0.device
After=sys-subsystem-net-devices-eth0.device
[Service]
Type=oneshot
ExecStart=/sbin/ip link set dev eth0 address b8:27:eb:00:00:01
ExecStart=/sbin/ip link set dev eth0 up
[Install]
WantedBy=multi-user.target


sudo chmod 644 /lib/systemd/system/setmac.service
sudo systemctl daemon-reload
sudo systemctl enable setmac.service
